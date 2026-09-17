# Peta-jalur-spanyol-kel3.github.io
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Jalur Laut Spanyol - Kelompok 3</title>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<style>
    * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', sans-serif; }
  body { background: linear-gradient(135deg, #0a2342, #1e3c72); color: #fff; }
  header { text-align: center; padding: 20px; background: rgba(0,0,0,0.4); border-bottom: 3px solid #FFD700; }
  header h1 { font-size: 1.8em; color: #FFD700; }
.anggota { background: rgba(255,255,255,0.1); padding: 15px; margin: 15px auto; border-radius: 10px; max-width: 700px; }
.anggota ul { list-style: none; display: flex; flex-wrap: wrap; justify-content: center; gap: 10px; }
.anggota li { background: #FFD700; color: #0a2342; padding: 6px 12px; border-radius: 20px; font-weight: bold; font-size: 0.9em; }
.petunjuk { background: rgba(0,0,0,0.6); border: 2px solid #FFD700; border-radius: 15px; padding: 15px; margin: 15px auto; max-width: 900px; }
.petunjuk h4 { color: #FFD700; text-align: center; margin-bottom: 10px; }
.petunjuk ul { list-style: none; padding-left: 10px; }
.petunjuk li { margin: 8px 0; }
.petunjuk b { color: #FFD700; }
  #map { height: 550px; width: 95%; margin: 20px auto; border-radius: 15px; border: 3px solid #FFD700; position: relative; }
.kontrol { text-align: center; margin: 10px auto; }
.btn { background: #FFD700; color: #0a2342; border: none; padding: 12px 20px; margin: 5px; border-radius: 10px; font-weight: bold; font-size: 1em; cursor: pointer; transition: 0.3s; }
.btn:hover { background: #FF4500; color: #fff; transform: scale(1.05); }
.sejarah { background: rgba(0,0,0,0.5); padding: 20px; margin: 20px auto; border-radius: 15px; max-width: 900px; line-height: 1.8; }
.sejarah h3 { color: #FFD700; margin-bottom: 15px; text-align: center; }
.rute { background: rgba(255,215,0,0.1); padding: 10px; border-left: 4px solid #FFD700; margin: 10px 0; border-radius: 5px; }
.legenda { position: absolute; top: 20px; right: 20px; z-index: 1000; background: rgba(0,0,0,0.85); padding: 12px; border-radius: 10px; border: 2px solid #FFD700; font-size: 0.9em; }
.garis { width: 30px; height: 3px; display: inline-block; margin-right: 5px; }
.sumber { font-size: 0.85em; text-align: right; font-style: italic; margin-top: 15px; color: #FFD700; }
.nama-daerah { background: rgba(255,215,0,0.9); color: #0a2342; padding: 5px 12px; border-radius: 20px; font-weight: bold; font-size: 12px; transition: all 0.5s ease; border: 2px solid #fff; pointer-events: none; /* BIAR BISA KLIK LEWAT LABEL */ }
.nama-daerah.aktif { font-size: 22px!important; background: #FF4500; color: #fff; padding: 10px 18px; box-shadow: 0 0 20px #FF4500; transform: scale(1.2); }
.emoji-boat { font-size: 40px; text-shadow: 0 0 15px rgba(255,255,255,1); pointer-events: none; }
.leaflet-popup-content-wrapper { background: #f4e4bc; color: #0a2342; border-radius: 10px; border: 2px solid #FFD700; font-size: 14px; }
.leaflet-popup-content b { color: #FF4500; }
  footer { text-align: center; padding: 15px; background: rgba(0,0,0,0.4); }
</style>
</head>
<body>

<header>
  <h1>🌊 JALUR LAUT BANGSA SPANYOL KE INDONESIA ⛵</h1>
  <h2>Ekspedisi Magellan - Elcano 1519 - 1522</h2>
  <div class="anggota">
    <h3>Kelompok 3</h3>
    <ul>
      <li>Anggia Andini Arifin</li>
      <li>Bintang Zainal</li>
      <li>Devina Anastasya Putri</li>
      <li>Haifa Rina Amalina</li>
      <li>Mochamad Fiqri Suteja</li>
      <li>Vitra Apriansyah Hidayat</li>
    </ul>
  </div>
</header>

<div class="petunjuk">
  <h4>📖 PETUNJUK PENGGUNAAN PETA</h4>
  <ul>
    <li><b>1. Memperbesar/Mengecilkan Peta:</b> Gunakan scroll mouse atau tombol + / - di pojok kiri peta</li>
    <li><b>2. Menggeser Peta:</b> Klik dan tahan lalu geser mouse / drag di layar HP</li>
    <li><b>3. Keterangan Titik:</b> Klik titik bulat <span style="color:red">● MERAH</span> untuk lihat tahun & peristiwa</li>
    <li><b>4. Memajukan Kapal:</b> Tekan tombol <b>"GERAKKAN KAPAL"</b>. Nama daerah akan membesar otomatis</li>
  </ul>
</div>

<div id="map">
  <div class="legenda">
    <b>Legenda</b><br>
    <span class="garis" style="background:#FF4500"></span> Jalur Ekspedisi <br>
    🔴 Titik Persinggahan <br>
    ⛵ Kapal Magellan
  </div>
</div>

<div class="kontrol">
  <button class="btn" onclick="gerakSatuLangkah()">GERAKKAN KAPAL</button>
  <button class="btn" onclick="ulangDariAwal()">ULANGI</button>
  <button class="btn" onclick="pauseOtomatis()">AUTO ON/OFF</button>
</div>

<div class="sejarah">
  <h3>📜 SEJARAH MASUKNYA SPANYOL KE INDONESIA</h3>
  <div class="rute"><b>1492:</b> Christopher Columbus melakukan pelayaran ke arah barat atas dukungan Spanyol untuk mencari jalur baru menuju Asia dan mendapatkan rempah-rempah. Ia kemudian tiba di wilayah Amerika.</div>
  <div class="rute"><b>1519:</b> Ferdinand Magellan memimpin ekspedisi Spanyol menuju Kepulauan Rempah-rempah. Rombongannya berlayar melewati Selat Magellan dan memasuki Samudra Pasifik.</div>
  <div class="rute"><b>1521:</b> Setelah tiba di Filipina dan Magellan meninggal, pelayaran dilanjutkan oleh Juan Sebastián Elcano. Rombongan akhirnya sampai di Kepulauan Maluku, terutama Tidore, yang terkenal sebagai penghasil rempah-rempah.</div>
  <div class="rute"><b>Persaingan di Maluku:</b> Spanyol menjalin hubungan dan bersekutu dengan Kesultanan Tidore, sedangkan Portugis bersekutu dengan Kesultanan Ternate. Kedua bangsa Eropa tersebut bersaing untuk menguasai perdagangan rempah-rempah.</div>
  <div class="rute"><b>1529:</b> Persaingan Spanyol dan Portugis berakhir melalui Perjanjian Saragosa. Spanyol meninggalkan Maluku dan kemudian memusatkan kekuasaannya di Filipina, sedangkan Portugis tetap berkuasa di Maluku.</div>
  <p class="sumber">Sumber : Tirto.id</p>
</div>

<footer>Dibuat untuk Tugas Sejarah | Kelompok 3 | 2026</footer>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
  var map = L.map('map').setView([-10, -30], 3);

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; OpenStreetMap'
  }).addTo(map);

  var ruteSpanyol = [
    {koor: [36.82, -6.35], nama: "Spanyol", ket: "<b>Tahun: 20 Sept 1519</b><br>Berangkat dari Sanlucar de Barrameda dengan 5 kapal"},
    {koor: [28.3, -16.4], nama: "Kep. Canary", ket: "<b>Tahun: Sept 1519</b><br>Perhentian pertama untuk mengisi air dan makanan"},
    {koor: [15, -25], nama: "Samudra Atlantik", ket: "<b>Tahun: 1519</b><br>Menyeberangi Samudra Atlantik menuju Amerika Selatan"},
    {koor: [-8, -35], nama: "Brazil", ket: "<b>Tahun: 1519-1520</b><br>Menyusuri pantai Brazil mencari jalur ke barat"},
    {koor: [-25, -45], nama: "Argentina", ket: "<b>Tahun: 1520</b><br>Terus ke selatan menyusuri Argentina"},
    {koor: [-40, -55], nama: "Patagonia", ket: "<b>Tahun: 1520</b><br>Wilayah dingin di ujung Amerika Selatan"},
    {koor: [-52.5, -70.5], nama: "Selat Magellan", ket: "<b>Tahun: Okt 1520</b><br>Menemukan dan melewati Selat Magellan ke Samudra Pasifik"},
    {koor: [-30, -80], nama: "Samudra Pasifik", ket: "<b>Tahun: 1520-1521</b><br>98 hari berlayar tanpa melihat daratan. Banyak awak kelaparan"},
    {koor: [-15, -100], nama: "Tengah Pasifik", ket: "<b>Tahun: 1521</b><br>Terus berlayar ke arah barat di Samudra Pasifik"},
    {koor: [5, -110], nama: "Utara Pasifik", ket: "<b>Tahun: 1521</b><br>Berbelok ke utara mendekati Asia"},
    {koor: [10.3, 123.9], nama: "Filipina", ket: "<b>Tahun: Maret 1521</b><br>Tiba di Filipina. Ferdinand Magellan gugur di Pertempuran Mactan"},
    {koor: [0.66, 127.4], nama: "Tidore, Maluku", ket: "<b>Tahun: Nov 1521</b><br>Tiba di Tidore, Maluku. Bersekutu dengan Sultan Tidore untuk rempah-rempah"}
  ];

  var semuaMarker = [];
  ruteSpanyol.forEach(function(titik, index){
    // GANTI JADI MARKER BIASA BIAR POPUP PASTI MUNCUL
    var marker = L.marker(titik.koor).addTo(map);

    // Tambah lingkaran merah di belakang marker
    L.circleMarker(titik.koor, { radius: 10, color: "#FF0000", fillColor: "#FF0000", fillOpacity: 0.8 }).addTo(map);

    var label = L.tooltip({ permanent: true, direction: 'top', className: 'nama-daerah', offset: [0, -10] }).setContent(titik.nama);
    marker.bindTooltip(label).openTooltip();

    // POPUP KETERANGAN
    marker.bindPopup(titik.ket);
    semuaMarker.push(marker);
  });

  var koordinatSaja = ruteSpanyol.map(item => item.koor);
  L.polyline(koordinatSaja, {color: '#FF4500', weight: 4, dashArray: '8, 6'}).addTo(map);

  var boatIcon = L.divIcon({ className: 'emoji-boat', html: '⛵', iconSize: [40, 40], iconAnchor: [20, 20] });
  var boat = L.marker(koordinatSaja[0], {icon: boatIcon}).addTo(map);
  var step = 0;
  var autoPlay = null;

  function updateKapal() {
    document.querySelectorAll('.nama-daerah').forEach(el => el.classList.remove('aktif'));
    boat.setLatLng(koordinatSaja[step]);
    map.panTo(koordinatSaja[step], {animate: true, duration: 1.5});
    var tooltipEl = semuaMarker[step].getTooltip().getElement();
    if(tooltipEl) tooltipEl.classList.add('aktif');
  }

  function gerakSatuLangkah() {
    if(step < ruteSpanyol.length - 1) { step++; } else { step = 0; }
    updateKapal();
  }
  function ulangDariAwal() { step = 0; updateKapal(); }
  function pauseOtomatis() {
    if(autoPlay) { clearInterval(autoPlay); autoPlay = null; alert("Auto OFF"); }
    else { autoPlay = setInterval(gerakSatuLangkah, 2500); alert("Auto ON"); }
  }

  updateKapal();
  map.fitBounds(L.polyline(koordinatSaja).getBounds());
</script>
</body>
</html>
