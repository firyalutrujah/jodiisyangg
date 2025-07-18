<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Kepulauan Seribu - Gen Z Explorer</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background-color: #e0f7fa;
    }

    header {
      background-color: #006064;
      color: white;
      padding: 20px;
      text-align: center;
    }

    .map-container {
      position: relative;
      text-align: center;
    }

    .map-container img {
      width: 100%;
      max-width: 1000px;
    }

    .pulau-btn {
      position: absolute;
      background-color: #26c6da;
      color: white;
      border: none;
      border-radius: 10px;
      padding: 5px 10px;
      cursor: pointer;
      font-size: 0.9em;
      transform: translate(-50%, -50%);
    }

    .info-box {
      max-width: 900px;
      margin: 30px auto;
      background: white;
      padding: 20px;
      border-radius: 16px;
      box-shadow: 0 5px 20px rgba(0,0,0,0.1);
      display: none;
      animation: fadeIn 0.5s ease;
    }

    .info-box h2 {
      color: #004d40;
    }

    .img-row {
      display: flex;
      gap: 15px;
      margin-top: 10px;
    }

    .img-row img {
      flex: 1;
      border-radius: 10px;
      max-height: 200px;
      object-fit: cover;
    }

    @keyframes fadeIn {
      from {opacity: 0;}
      to {opacity: 1;}
    }

    /* Posisi tombol pulau */
    #tidung-btn { top: 60%; left: 30%; }
    #pari-btn { top: 65%; left: 35%; }
    #pramuka-btn { top: 40%; left: 50%; }
    #macan-btn { top: 50%; left: 60%; }
    #harapan-btn { top: 30%; left: 70%; }

  </style>
</head>
<body>

  <header>
    <h1>🗺️ Jelajahi Pulau-Pulau di Kepulauan Seribu</h1>
    <p>Geser, klik, dan eksplor kaya SDA & keunikan tiap pulau!</p>
  </header>

  <div class="map-container">
    <img src="https://upload.wikimedia.org/wikipedia/commons/0/0f/Kepulauan_Seribu_map.png" alt="Peta Kepulauan Seribu">
    <button class="pulau-btn" id="tidung-btn" onclick="tampilkan('tidung')">Tidung</button>
    <button class="pulau-btn" id="pari-btn" onclick="tampilkan('pari')">Pari</button>
    <button class="pulau-btn" id="pramuka-btn" onclick="tampilkan('pramuka')">Pramuka</button>
    <button class="pulau-btn" id="macan-btn" onclick="tampilkan('macan')">Macan</button>
    <button class="pulau-btn" id="harapan-btn" onclick="tampilkan('harapan')">Harapan</button>
  </div>

  <div id="info" class="info-box">
    <h2 id="judul"></h2>
    <p id="deskripsi"></p>
    <div class="img-row">
      <img id="gambarPulau" src="" alt="Pulau">
      <img id="gambarSDA" src="" alt="SDA">
      <img id="gambarUnik" src="" alt="Keunikan">
    </div>
  </div>

  <script>
    const dataPulau = {
      tidung: {
        judul: "Pulau Tidung",
        deskripsi: "Pulau dengan Jembatan Cinta ikonik. Cocok buat sepedaan, snorkeling, dan healing bareng teman.",
        gambarPulau: "https://upload.wikimedia.org/wikipedia/commons/d/d2/Jembatan_Cinta_Tidung.jpg",
        gambarSDA: "https://img.freepik.com/free-photo/underwater-reef-coral_23-2150317752.jpg", // ilustrasi terumbu karang
        gambarUnik: "https://upload.wikimedia.org/wikipedia/commons/thumb/d/d2/Jembatan_Cinta_Tidung.jpg/800px-Jembatan_Cinta_Tidung.jpg"
      },
      pari: {
        judul: "Pulau Pari",
        deskripsi: "Pulau Instagramable dengan Pantai Pasir Perawan. Air jernih, tenang, cocok buat rebahan dan foto-foto.",
        gambarPulau: "https://www.garuda-indonesia.com/assets/images/pariwisata/destination/pari_island.jpg",
        gambarSDA: "https://img.freepik.com/free-photo/seaweed_23-2150425618.jpg", // rumput laut
        gambarUnik: "https://cdn.idntimes.com/content-images/community/2021/11/1a8f485f63e0191580a8c6726db28a61-5c08d1ecfb69757fd9280a4719d1e0c7_600x400.jpg"
      },
      pramuka: {
        judul: "Pulau Pramuka",
        deskripsi: "Pusat konservasi penyu dan edukasi lingkungan. Ada hutan mangrove juga loh!",
        gambarPulau: "https://upload.wikimedia.org/wikipedia/commons/b/bd/Pulau_Pramuka.jpg",
        gambarSDA: "https://cdn0-production-images-kly.akamaized.net/8pKDFM9NmLR9DEduWYDaEjdEvpc=/0x146:799x597/469x260/filters:quality(75):strip_icc():format(webp)/kly-media-production/medias/2289453/original/084510900_1537851455-penyu.jpeg", // penyu
        gambarUnik: "https://asset.kompas.com/crops/Yzv7n9HuEVkg0ufA--bgwCpi2p4=/0x0:1000x667/750x500/data/photo/2022/06/26/62b84b959df0c.jpg"
      },
      macan: {
        judul: "Pulau Macan",
        deskripsi: "Eco-resort tropis yang super aesthetic. Bisa nginep di pondok kayu terapung!",
        gambarPulau: "https://media-cdn.tripadvisor.com/media/photo-s/0e/d7/c2/89/pulau-macan-resort.jpg",
        gambarSDA: "https://www.fao.org/fileadmin/user_upload/newsroom/img/DSC_0584_01.jpg", // solar panel
        gambarUnik: "https://pulau-macan.com/assets/images/floating-villa.jpg"
      },
      harapan: {
        judul: "Pulau Harapan",
        deskripsi: "Deket pulau-pulau kecil yang cocok buat island hopping. Banyak spot snorkeling kece!",
        gambarPulau: "https://wisatakepulauanseribu.com/wp-content/uploads/2018/11/Pulau-Harapan-Kepulauan-Seribu.jpg",
        gambarSDA: "https://www.rimbakita.com/wp-content/uploads/2020/09/terumbu-karang.jpg",
        gambarUnik: "https://liburanyuk.co.id/wp-content/uploads/2020/06/snorkeling-di-Pulau-Seribu.jpg"
      }
    };

    function tampilkan(nama) {
      const data = dataPulau[nama];
      document.getElementById("judul").innerText = data.judul;
      document.getElementById("deskripsi").innerText = data.deskripsi;
      document.getElementById("gambarPulau").src = data.gambarPulau;
      document.getElementById("gambarSDA").src = data.gambarSDA;
      document.getElementById("gambarUnik").src = data.gambarUnik;
      document.getElementById("info").style.display = "block";
    }
  </script>

</body>
</html>
