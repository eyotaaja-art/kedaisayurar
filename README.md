<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>KASIR VRO - KEDAI AR</title>
    <script src="https://unpkg.com/tesseract.js@v2.1.0/dist/tesseract.min.js"></script>
    <style>
        body { font-family: sans-serif; background: #1a1a1a; color: #00ff00; text-align: center; padding: 10px; }
        #cameraView { width: 100%; border: 2px solid #00ff00; border-radius: 10px; background: #000; }
        .hasil { font-size: 2.5em; margin: 20px; padding: 10px; border: 2px dashed #00ff00; font-weight: bold; }
        .btn { background: #00ff00; color: #000; padding: 20px; width: 100%; border-radius: 10px; font-weight: bold; font-size: 1.2em; border: none; }
    </style>
</head>
<body>
    <h2 style="color:#fff;">MATA AI KEDAI AR</h2>
    <video id="cameraView" autoplay playsinline></video>
    <div class="hasil" id="status">SIAP PINDAI...</div>
    <button class="btn" onclick="bacaTimbangan()">BACA TIMBANGAN (SCAN)</button>

    <script>
        const video = document.getElementById('cameraView');
        const status = document.getElementById('status');

        navigator.mediaDevices.getUserMedia({ video: { facingMode: 'environment' } })
            .then(s => video.srcObject = s)
            .catch(e => status.innerText = "ERROR: GUNAKAN HTTPS!");

        async function bacaTimbangan() {
            status.innerText = "MEMPROSES...";
            const canvas = document.createElement('canvas');
            canvas.width = video.videoWidth; canvas.height = video.videoHeight;
            canvas.getContext('2d').drawImage(video, 0, 0);
            
            Tesseract.recognize(canvas, 'eng').then(({ data: { text } }) => {
                let angka = text.replace(/[^0-9.,]/g, "");
                status.innerText = angka ? angka + " KG" : "ULANGI SCAN!";
            });
        }
    </script>
</body>
</html>
