<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Image Classifier</title>
    
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f0f2f5;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
        }

        .card {
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            text-align: center;
            max-width: 450px;
            width: 90%;
        }

        h2 { color: #333; margin-bottom: 20px; }

        .btn-group {
            display: flex;
            gap: 10px;
            justify-content: center;
            margin-bottom: 20px;
        }

        button, .upload-label {
            padding: 12px 20px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .btn-start { background: #2196F3; color: white; }
        .btn-start:hover { background: #1976D2; transform: translateY(-2px); }

        .upload-label { background: #4CAF50; color: white; display: inline-block; }
        .upload-label:hover { background: #388E3C; transform: translateY(-2px); }

        .image-preview-wrapper {
            border: 3px dashed #cbd5e0;
            border-radius: 12px;
            margin: 20px 0;
            min-height: 200px;
            display: flex;
            align-items: center;
            justify-content: center;
            background: #f8fafc;
            overflow: hidden;
        }

        #imageInput {
            max-width: 100%;
            max-height: 300px;
            display: none; /* Hidden until image is loaded */
        }

        #result-box {
            padding: 15px;
            background: #eef2ff;
            border-radius: 10px;
            border-left: 5px solid #4f46e5;
        }

        #status { color: #64748b; font-size: 0.9rem; }
        #label-container {
            font-size: 1.4rem;
            font-weight: bold;
            color: #1e293b;
            margin-top: 8px;
        }
    </style>
</head>
<body>

<div class="card">
    <h2>Teachable Machine</h2>
    
    <div class="btn-group">
        <button type="button" class="btn-start" onclick="init()">1. Load AI Model</button>
        
        <label for="imageUpload" class="upload-label">
            2. Choose Image
        </label>
        <input type="file" id="imageUpload" accept="image/*" onchange="loadImage(event)" style="display:none;">
    </div>

    <div class="image-preview-wrapper">
        <img id="imageInput" src="" alt="Preview">
        <p id="placeholder-text" style="color: #94a3b8;">Image will appear here</p>
    </div>

    <div id="result-box">
        <div id="status">Waiting for Model...</div>
        <div id="label-container">---</div>
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest/dist/tf.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@teachablemachine/image@latest/dist/teachablemachine-image.min.js"></script>

<script type="text/javascript">
    const URL = "https://teachablemachine.withgoogle.com/models/G-LrHVPk0/";
    let model;

    // 1. Initialize Model
    async function init() {
        const status = document.getElementById("status");
        status.innerHTML = "Loading Model... 🔄";
        
        try {
            const modelURL = URL + "model.json";
            const metadataURL = URL + "metadata.json";
            model = await tmImage.load(modelURL, metadataURL);
            status.innerHTML = "✅ Model Loaded. Ready for input.";
        } catch (e) {
            status.innerHTML = "❌ Error loading model.";
        }
    }

    // 2. Load and Display Image
    function loadImage(event) {
        if (!model) {
            alert("Please click 'Load AI Model' first!");
            return;
        }

        const file = event.target.files[0];
        if (!file) return;

        const reader = new FileReader();
        reader.onload = function (e) {
            const img = document.getElementById("imageInput");
            const placeholder = document.getElementById("placeholder-text");
            
            img.src = e.target.result;
            img.style.display = "block";
            placeholder.style.display = "none";

            img.onload = function () {
                predict(img);
            };
        };
        reader.readAsDataURL(file);
    }

    // 3. Prediction Logic
    async function predict(imageElement) {
        document.getElementById("status").innerHTML = "Analyzing Image...";
        const predictions = await model.predict(imageElement);

        // Sort to get the highest probability
        let best = predictions.reduce((prev, current) => 
            (prev.probability > current.probability) ? prev : current
        );

        document.getElementById("status").innerHTML = "Result Found:";
        document.getElementById("label-container").innerHTML = 
            `${best.className} (${(best.probability * 100).toFixed(1)}%)`;
    }
</script>

</body>
</html>
