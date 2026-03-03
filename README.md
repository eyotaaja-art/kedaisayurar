<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>AR VRO-ULTIMATE LIMITED LIVE</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        :root { --hijau-ar: #1b5e20; --biru-vro: #0288d1; --emas-vro: #ff9800; --merah-vro: #b71c1c; }
        body { font-family: 'Segoe UI', sans-serif; background: #eceff1; margin: 0; padding: 10px; color: #333; }
        .container { max-width: 480px; margin: auto; background: white; padding: 15px; border-radius: 15px; box-shadow: 0 10px 25px rgba(0,0,0,0.1); }
        
        .vro-header { text-align: center; border-bottom: 3px double var(--hijau-ar); padding-bottom: 10px; margin-bottom: 15px; }
        .vro-header h2 { margin: 0; color: var(--hijau-ar); font-size: 24px; letter-spacing: -1px; }
        .vro-header span { font-size: 11px; font-weight: 900; color: var(--biru-vro); text-transform: uppercase; }

        /* GRID TOP BUTTONS */
        .grid-top { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px; margin-bottom: 15px; }
        .btn-top { padding: 10px 5px; border: none; border-radius: 8px; color: white; font-weight: 900; font-size: 10px; cursor: pointer; text-transform: uppercase; transition: 0.2s; }
        .btn-top:active { transform: scale(0.95); }

        /* LIVE WINDOW (PABRIKAN WAWA) */
        #liveWindow { display:none; position:fixed; bottom: 20px; right: 20px; width: 220px; height: 350px; background: #000; border: 3px solid var(--emas-vro); border-radius: 15px; z-index: 8000; overflow: hidden; box-shadow: 0 5px 15px rgba(0,0,0,0.5); }
        .live-header { background: var(--emas-vro); padding: 5px 10px; font-size: 10px; font-weight: bold; display: flex; justify-content: space-between; align-items: center; }
        #liveContent { width: 100%; height: calc(100% - 25px); border: none; }

        /* MEDIA OVERLAY (MATA AI) */
        #mediaOverlay { display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:black; z-index: 8500; }
        #cameraView { width:100%; height:80%; object-fit: cover; }
        .media-controls { height:20%; display:flex; justify-content:space-around; align-items:center; background:#111; }

        /* MODAL TEMPEL */
        .modal { display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.85); z-index: 9000; align-items: center; justify-content: center; }
        .modal-content { background: white; padding: 20px; border-radius: 15px; width: 90%; max-width: 400px; box-sizing: border-box; border: 3px solid var(--biru-vro); }
        textarea#inputTempel { width: 100%; height: 220px; border: 1px solid #ccc; border-radius: 8px; padding: 12px; font-weight: bold; box-sizing: border-box; resize: none; font-size: 14px; background: #f9f9f9; }

        /* INPUT LIST */
        #listContainer { max-height: 350px; overflow-y: auto; border: 1px solid #ddd; padding: 5px; border-radius: 10px; background: #fdfdfd; margin-bottom: 10px; }
        .row-item { display: flex; gap: 4px; align-items: center; margin-bottom: 4px; background: #fff; padding: 5px; border-radius: 6px; border: 1px solid #eee; }
        .in-nama { flex: 1; border: none; font-weight: bold; text-transform: uppercase; font-size: 13px; outline: none; }
        .in-kg { width: 45px; text-align: center; background: #fffde7; border: 1px solid #ffe082; font-weight: 900; border-radius: 4px; }
        .in-hrg { width: 60px; text-align: center; background: #e3f2fd; border: 1px solid #bbdefb; font-weight: 900; border-radius: 4px; }

        /* BON WRAPPER */
        #bonWrapper { display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.98); z-index:9999; overflow-y:auto; }
        #bonCaptureArea { display: flex; justify-content: center; padding: 20px 10px; }
        #bonDigital { background:#fff; border: 1px solid #000; padding: 15px; width: 100%; max-width: 380px; box-sizing: border-box; position: relative; }
        .seni-ar-bg { position: absolute; bottom: 80px; left: 50%; transform: translateX(-50%); font-family: serif; font-size: 110px; font-weight: 900; color: rgba(27, 94, 32, 0.07); z-index: 0; pointer-events: none; letter-spacing: 20px; }
        .item-row { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #eee; padding: 2px 0; }
        
        .btn-vro { padding: 14px; border: none; border-radius: 10px; color: white; font-weight: 900; cursor: pointer; text-transform: uppercase; width: 100%; }
    </style>
</head>
<body>

<div class="container" id="mesinUtama">
    <div class="vro-header">
        <h2>KEDAI SAYUR AR</h2>
        <span>VRO-ULTIMATE LIMITED LIVE</span>
    </div>

    <div class="grid-top">
        <button class="btn-top" style="background:var(--merah-vro);" onclick="aktifkanMata()">👁️ MATA AI</button>
        <button class="btn-top" style="background:var(--emas-vro);" onclick="bukaGaleri()">🖼️ GALERI</button>
        <button class="btn-top" style="background:var(--biru-vro);" onclick="bukaTempel()">📋 TEMPEL</button>
    </div>

    <input type="text" id="manualNama" placeholder="NAMA PEMBELI..." style="width:100%; padding:14px; margin-bottom:10px; border-radius:8px; border:1px solid #ccc; font-weight: 900; box-sizing: border-box; text-transform: uppercase;">

    <div id="listContainer"></div>
    
    <div style="display:grid; grid-template-columns: 2fr 1fr; gap:10px;">
        <button onclick="tambahBaris()" style="padding:12px; background:#f0f0f0; border:1px dashed #999; border-radius:8px; font-weight:bold;">+ BARANG</button>
        <button class="btn-vro" style="background:var(--hijau-ar);" onclick="generateBon()">🚀 BON</button>
    </div>

    <button onclick="toggleLive()" style="width:100%; margin-top:10px; background:#000; color:#fff; border-radius:8px; padding:8px; font-size:11px; font-weight:900;">📺 LIVE MEDIA ECOSYSTEM</button>
</div>

<div id="liveWindow">
    <div class="live-header">
        <span>LIVE VRO ECOSYSTEM</span>
        <span onclick="toggleLive()" style="cursor:pointer; font-size:14px;">[ X ]</span>
    </div>
    <iframe id="liveContent" src="https://m.facebook.com"></iframe>
</div>

<div id="mediaOverlay">
    <video id="cameraView" autoplay playsinline></video>
    <div class="media-controls">
        <button class="btn-vro" style="background:#666; width:100px;" onclick="tutupMedia()">KELUAR</button>
        <button id="btnAmbil" class="btn-vro" style="background:var(--hijau-ar); width:100px;">AMBIL</button>
    </div>
    <input type="file" id="fileInput" accept="image/*" style="display:none;" onchange="prosesGambar(this)">
</div>

<div id="modalTempel" class="modal">
    <div class="modal-content">
        <h3 style="margin:0 0 10px 0; color:var(--biru-vro); text-align:center;">JALUR TEMPEL TXT</h3>
        <textarea id="inputTempel" placeholder="Salin list di sini..."></textarea>
        <div style="display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-top:15px;">
            <button class="btn-vro" style="background:#666;" onclick="tutupTempel()">BATAL</button>
            <button class="btn-vro" style="background:var(--hijau-ar);" onclick="prosesTempel()">PROSES</button>
        </div>
    </div>
</div>

<div id="bonWrapper">
    <div id="bonCaptureArea">
        <div id="bonDigital">
            <div class="seni-ar-bg">AR</div>
            <div style="display: flex; justify-content: space-between; border-bottom: 2px solid #000; padding-bottom: 5px; margin-bottom: 5px;">
                <div style="z-index:1;">
                    <h3 style="margin:0; color:var(--hijau-ar); font-size:20px;">KEDAI AR</h3>
                    <div style="font-size:9px; font-weight:bold;">Pasar Pulau Payung, Dumai, Riau</div>
                    <div style="font-size:11px; font-weight:900;">📞 082252939502</div>
                </div>
                <div style="text-align:right; z-index:1;">
                    <div style="font-size:16px; font-weight:900;" id="outNama"></div>
                    <div style="font-size:10px; font-weight:900; color:var(--biru-vro);">VRO LIMITED</div>
                </div>
            </div>
            <div id="outBody" style="z-index:1; position:relative;"></div>
            <div style="margin-top:10px; border-top:2px solid #000; padding-top:5px; display:flex; justify-content:space-between; align-items:flex-end; z-index:1; position:relative;">
                <div>
                    <div style="font-size:11px; font-weight:900;">TOTAL BAYAR</div>
                    <div style="font-size:30px; font-weight:900; color:var(--hijau-ar); line-height:1;" id="outTotal"></div>
                    <div style="font-size:10px; font-weight:bold; font-style:italic; color:#1b5e20;">"Segar Barokah, Belanja Berkah"</div>
                </div>
                <div style="text-align:right;">
                    <div style="font-size:11px; font-weight:900; color:var(--biru-vro);">WAWA GEMINI</div>
                </div>
            </div>
        </div>
    </div>
    <div style="display:grid; grid-template-columns:1fr 1fr; gap:12px; padding:20px; max-width:400px; margin:auto;">
        <button class="btn-vro" style="background:#333;" onclick="tutupBon()">KEMBALI</button>
        <button class="btn-vro" style="background:#25d366;" onclick="shareWA()">KIRIM WA</button>
    </div>
</div>

<script>
    // MESIN UTAMA
    function tambahBaris(n='', k='', h='') {
        const container = document.getElementById('listContainer');
        const div = document.createElement('div');
        div.className = 'row-item';
        div.innerHTML = `<div style="font-size:9px; font-weight:900; width:15px; color:#999;">${container.children.length + 1}</div>
            <input type="text" class="in-nama" value="${n}" placeholder="NAMA">
            <input type="text" class="in-kg" value="${k}" placeholder="Kg">
            <input type="number" class="in-hrg" value="${h}" placeholder="Hrg">
            <button style="color:red; border:none; background:none; font-weight:bold; cursor:pointer;" onclick="this.parentElement.remove();">×</button>`;
        container.appendChild(div);
        container.scrollTop = container.scrollHeight;
    }

    // JALUR TEMPEL
    function bukaTempel() { document.getElementById('modalTempel').style.display = 'flex'; }
    function tutupTempel() { document.getElementById('modalTempel').style.display = 'none'; }
    function prosesTempel() {
        const lines = document.getElementById('inputTempel').value.split('\n');
        lines.forEach(l => { if(l.trim()){ let p = l.split(/\s+/); tambahBaris(p[0], p[1]||'', ''); }});
        tutupTempel();
    }

    // JALUR MATA AI (KAMERA)
    function aktifkanMata() {
        const overlay = document.getElementById('mediaOverlay');
        const video = document.getElementById('cameraView');
        overlay.style.display = 'block';
        navigator.mediaDevices.getUserMedia({ video: { facingMode: "environment" } })
            .then(s => video.srcObject = s)
            .catch(err => alert("Kamera Error: " + err));
        
        document.getElementById('btnAmbil').onclick = () => {
            tambahBaris("SCAN MATA AI", "1", "");
            tutupMedia();
        };
    }

    function bukaGaleri() { document.getElementById('fileInput').click(); }
    function prosesGambar(input) {
        if(input.files && input.files[0]) {
            tambahBaris("DARI GALERI", "1", "");
            alert("Gambar berhasil dimasukkan ke sistem!");
        }
    }

    function tutupMedia() {
        const video = document.getElementById('cameraView');
        if(video.srcObject) video.srcObject.getTracks().forEach(t => t.stop());
        document.getElementById('mediaOverlay').style.display = 'none';
    }

    // LIVE SYSTEM
    function toggleLive() {
        const win = document.getElementById('liveWindow');
        win.style.display = (win.style.display === 'block') ? 'none' : 'block';
    }

    // GENERATE BON
    function generateBon() {
        const rows = document.querySelectorAll('.row-item');
        let html = ""; let total = 0;
        rows.forEach((row, i) => {
            const n = row.querySelector('.in-nama').value;
            const k = parseFloat(row.querySelector('.in-kg').value) || 0;
            const h = parseFloat(row.querySelector('.in-hrg').value) || 0;
            if(n) {
                const sub = k * h * 1000; total += sub;
                html += `<div class="item-row">
                    <div style="flex:1;">
                        <div style="font-size:14px; font-weight:900; text-transform:uppercase;">${i+1}. ${n}</div>
                        <div style="font-size:14px; font-weight:bold;"><span style="color:var(--biru-vro); font-size:16px;">${k} kg</span> x Rp ${h}.000</div>
                    </div>
                    <div style="font-size:19px; font-weight:900;">${sub.toLocaleString('id-ID')}</div>
                </div>`;
            }
        });
        document.getElementById('outNama').innerText = document.getElementById('manualNama').value || "PELANGGAN SETIA";
        document.getElementById('outBody').innerHTML = html;
        document.getElementById('outTotal').innerText = "Rp " + total.toLocaleString('id-ID');
        document.getElementById('bonWrapper').style.display = 'block';
    }

    function tutupBon() { document.getElementById('bonWrapper').style.display = 'none'; }

    async function shareWA() {
        const area = document.getElementById('bonCaptureArea');
        const canvas = await html2canvas(area, { scale: 3 });
        canvas.toBlob(blob => {
            const file = new File([blob], "BonAR_Vro_Limited.png", { type: "image/png" });
            if (navigator.share) {
                navigator.share({ files: [file], title: 'Bon Kedai AR', text: 'Terima kasih telah berbelanja!' });
            } else {
                alert("Browser tidak mendukung fitur Share.");
            }
        });
    }
</script>
</body>
</html>
