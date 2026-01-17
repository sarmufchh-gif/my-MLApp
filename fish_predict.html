<div>Teachable Machine Image Model</div>

<button type="button" onclick="init()">Start</button>
<br><br>

<!-- ปุ่มเลือกภาพ -->
<label for="imageUpload"
       style="padding:8px 16px;
              background:#4CAF50;
              color:white;
              cursor:pointer;
              border-radius:5px;">
    เลือกภาพ
</label>
<input type="file" id="imageUpload" accept="image/*"
       onchange="loadImage(event)" style="display:none;">

<br><br>

<!-- แสดงภาพที่อัปโหลด -->
<img id="imageInput" width="200" height="200">

<div id="label-container"></div>

<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest/dist/tf.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@teachablemachine/image@latest/dist/teachablemachine-image.min.js"></script>

<script type="text/javascript">
    const URL = "https://teachablemachine.withgoogle.com/models/G-LrHVPk0/";
    let model;

    // โหลดโมเดล
    async function init() {
        const modelURL = URL + "model.json";
        const metadataURL = URL + "metadata.json";

        model = await tmImage.load(modelURL, metadataURL);
        document.getElementById("label-container").innerHTML = "Model loaded";
    }

    // อ่านไฟล์ภาพด้วย FileReader
    function loadImage(event) {
        const file = event.target.files[0];
        if (!file) return;

        const reader = new FileReader();
        reader.onload = function () {
            const img = document.getElementById("imageInput");
            img.src = reader.result;
            img.onload = function () {
                predict(img);
            };
        };
        reader.readAsDataURL(file);
    }

    // ทำนายผลจากรูปภาพ
    async function predict(image) {
        const predictions = await model.predict(image);

        // หา class ที่มีค่าความน่าจะเป็นสูงที่สุด
        let best = predictions[0];
        for (let i = 1; i < predictions.length; i++) {
            if (predictions[i].probability > best.probability) {
                best = predictions[i];
            }
        }

        document.getElementById("label-container").innerHTML =
            best.className + " : " + best.probability.toFixed(2);
    }
</script>