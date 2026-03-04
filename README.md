<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <title>KASIR KEDAI SAYUR AR</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <style>
        body { font-family: 'Courier New', Courier, monospace; background: #000; color: #00ff00; margin: 0; padding: 10px; }
        .vro-card { border: 2px solid #00ff00; border-radius: 10px; padding: 15px; background: #111; box-shadow: 0 0 15px #00ff00; }
        h2 { text-align: center; color: #00ff00; text-shadow: 2px 2px #004400; margin-bottom: 20px; }
        
        /* Tombol Utama */
        .btn { width: 100%; padding: 15px; margin: 5px 0; border: none; border-radius: 5px; font-weight: bold; font-size: 16px; cursor: pointer; }
        .btn-mata { background: #ff0000; color: white; animation: pulse 2s infinite; }
        .btn-green { background: #00ff00; color: #000; }
        .btn-blue { background: #008cff; color: white; }
        
        /* Kotak Live & Kamera */
        #camera-wrap { display: none; position: relative; width: 100%; border: 2px solid #fff; margin-top: 10px; }
        video { width: 100%; height: auto; display: block; }
        .focus-box { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); width: 200px; height: 100px; border: 2px dashed #00ff00; box-shadow: 0 0 100px rgba(0,255,0,0.2); }
        
        /* Area Input & Output */
        select, textarea { width: 100%; background: #000; color: #00ff00; border: 1px solid #00ff00; padding: 10px; margin-top: 10px; box-sizing: border-box; }
        textarea { height: 120px; font-size: 14px; }
        
        @keyframes pulse { 0% { box-shadow: 0 0 0 0 rgba(255, 0, 0, 0.7); } 70% { box-shadow: 0 0 0 15px rgba(255, 0, 0, 0); } 100% { box-shadow: 0 0 0 0 rgba(255, 0, 0, 0); } }
    </style>
</head>
<body>

<div class="vro-card">
    <h2>KEDAI SAYUR AR 🛒</h2>

    <label>Pilih Sayuran:</label>
    <select id="pilihSayur">
        <option value="60">Cabe Merah (60/gr)</option>
        <option value="70">Cabe Rawit (70/gr)</option>
        <option value="45">Bawang Merah (45/gr)</option>
        <option value="40">Bawang Putih (40/gr)</option>
        <option value="20">Tomat (20/gr)</option>
        <option value="15">Wortel (15/gr)</option>
        <option value="18">Kentang (18/gr)</option>
        <option value="12">Kol (12/gr)</option>
        <option value="10">Sawi (10/gr)</option>
        <option value="15">Terong (15/gr)</option>
        <option value="10">Timun (10/gr)</option>
        <option value="25">Buncis (25/gr)</option>
        <option value="15">Kacang Panjang (15/gr)</option>
        <option value="18">Pare (18/gr)</option>
        <option value="5">Bayam (5/gr)</option>
        <option value="5">Kangkung (5/gr)</option>
        <option value="80">Daun Sop (80/gr)</option>
        <option value="80">Seledri (80/gr)</option>
        <option value="40">Jahe (40/gr)</option>
        <option value="30">Kunyit (30/gr)</option>
        <option value="25">Lengkuas (25/gr)</option>
        <option value="10">Serai (10/gr)</option>
        <option value="35">Bawang Bombai (35/gr)</option>
        <option value="20">Jeruk Nipis (20/gr)</option>
        <option value="12">Labu Siam (12/gr)</option>
        <option value="50">Petai (50/gr)</option>
    </select>

    <button class="btn btn-mata" onclick="mulaiScan()">👁️ AKTIFKAN MATA AI</button>

    <div id="camera-wrap">
        <video id="video" autoplay playsinline></video>
        <div class="focus-box"></div>
        <button onclick="stopScan()" style="background:#444; color:white; width:100%; border:none; padding:5px;">TUTUP</button>
    </div>

    <div style="display:flex; gap:10px;">
        <button class="btn btn-green" onclick="tempelTeks()">📋 TEMPEL</button>
        <button class="btn btn-blue" onclick="buatBon()">📝 BUAT BON</button>
    </div>

    <textarea id="output" placeholder="Hasil perhitungan akan muncul di sini..."></textarea>
</div>

<script>
    const video = document.getElementById('video');
    const wrap = document.getElementById('camera-wrap');
    const output = document.getElementById('output');

    async function mulaiScan() {
        try {
            const stream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: "environment" } });
            video.srcObject = stream;
            wrap.style.display = 'block';
        } catch (err) {
            alert("Izin kamera ditolak. Pastikan pakai HTTPS GitHub!");
        }
    }

    function stopScan() {
        const stream = video.srcObject;
        if(stream) stream.getTracks().forEach(t => t.stop());
        wrap.style.display = 'none';
    }

    function tempelTeks() {
        navigator.clipboard.readText().then(text => {
            // Logika 1 = 1000 (Misal angka timbangan 50gr)
            let angka = parseFloat(text) || 0;
            let harga = document.getElementById('pilihSayur').value;
            let nama = document.getElementById('pilihSayur').options[document.getElementById('pilihSayur').selectedIndex].text.split('(')[0];
            let total = angka * harga;
            
            output.value += `${nama}: ${angka}gr x Rp${harga} = Rp${total}\n`;
        });
    }

    function buatBon() {
        // Kep: Hanya menyalin isi bon saja
        output.select();
        document.execCommand('copy');
        alert("Bon berhasil dicopy ke memori!");
    }
</script>

</body>
</html>
