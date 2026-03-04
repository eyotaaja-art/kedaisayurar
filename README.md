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
                    canvas.width = video.videoWidth;
        canvas.height = video.videoHeight;
        canvas.getContext('2d').drawImage(video, 0, 0);

        Tesseract.recognize(canvas, 'eng').then(({ data: { text } }) => {
            let angka = text.replace(/[^0-9.,]/g, "");
            // Wawa: Rumus Sakti 1=1000 menyalin angka timbangan
            let unitPrice = text.match(/Unit|Harga/i) ? text.replace(/[^0-9]/g, "") : "0";
            let totalTimb = text.match(/Total/i) ? text.replace(/[^0-9]/g, "") : "0";

            if(angka) {
                tampilkanHasilVro(angka, unitPrice, totalTimb);
            } else {
                status.innerText = "PINDAI ULANG!";
            }
        });
    }

    
// Wawa: Database 26 Barang & Logika TROODON
const KEP_DATABASE_VRO = {
    "daftar_barang": {
        "Kubis/Kol": 9000, "Selada": 8000, "Tomat": 15000, "Daun Sup": 10000,
        "Sawi Pahit": 4500, "Bawang Prai": 12000, "Cabe Merah": 44000,
        "Cabe Hijau": 25000, "Bawang Putih": 20000, "Bawang Merah": 25000,
        "Bawang Bombay": 18000, "Kentang": 14000, "Wortel": 12000, 
        "Lobak": 10000, "Lotus": 30000, "Jahe": 20000, "Kunyit": 15000,
        "Laos": 10000, "Kencur": 25000, "Timun": 8000, "Pare": 10000,
        "Gambas": 10000, "Terong": 12000, "Japan": 7000, "Brokoli": 25000, "Bunga Kol": 25000
    },
    // Logika 9 jadi 9000
    "konversi": (angka) => parseFloat(angka) * 1000 
};

// Fungsi Wawa: Menampilkan hasil ke layar
function tampilkanHasilVro(berat, hargaUnit, total) {
    let fixHarga = KEP_DATABASE_VRO.konversi(hargaUnit);
    let fixTotal = KEP_DATABASE_VRO.konversi(total);
    document.getElementById('status').innerHTML = `
        <b>${currentDetect || 'SAYUR'}</b><br>
        Berat: ${berat} KG<br>
        Harga: Rp${fixHarga}<br>
        TOTAL: Rp${fixTotal}
    `;
}
</script>

</body>
</html>
