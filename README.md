# 🗺️ Small/Medium Armies World Map

This is a fully **interactive Club Penguin-style SMA League map**.  
You can copy all the code below into a GitHub Pages repo or your website — it works as a single project.

**Live Map Preview:**  
[![Map Preview](https://sdmntprcentralus.oaiusercontent.com/files/00000000-62b8-61f5-ba36-f6c45dbf4fbe/raw?se=2025-10-19T13%3A41%3A12Z&sp=r&sv=2024-08-04&sr=b&scid=9f39cb74-b444-440d-832c-4aef161ba074&skoid=77636ecc-ad8d-44df-baa7-163b524a0261&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2025-10-19T12%3A26%3A42Z&ske=2025-10-20T12%3A26%3A42Z&sks=b&skv=2024-08-04&sig=gC0/rc/ueEkmcvn1be9GxMvP9rk5CiTYZ3hylhrMlA0%3D)

---

## 📦 Full Code

### 1️⃣ HTML (`index.html`)
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Small/Medium Armies World Map</title>
  <style>
    body {
      font-family: "Poppins", sans-serif;
      background-color: #0e1621;
      color: white;
      text-align: center;
    }
    #map-container {
      position: relative;
      display: inline-block;
    }
    #base-map {
      width: 100%;
      max-width: 1200px;
      border: 2px solid white;
    }
    #map-overlay {
      position: absolute;
      top: 0;
      left: 0;
      pointer-events: none;
    }
    #territory-info {
      margin-top: 20px;
      background: rgba(255,255,255,0.1);
      padding: 15px;
      border-radius: 10px;
      width: 300px;
      margin-left: auto;
      margin-right: auto;
    }
    #territory-flag {
      width: 60px;
      height: 40px;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <h1>Small/Medium Armies World Map</h1>
  <div id="map-container">
    <img id="base-map" src="https://i.imgur.com/3I2Cj0x.png" alt="SMA Map">
    <canvas id="map-overlay"></canvas>
  </div>
  <div id="territory-info">
    <h2 id="territory-name">Select a Territory</h2>
    <p><strong>Region:</strong> <span id="territory-region">–</span></p>
    <p><strong>Owner:</strong> <span id="territory-owner">–</span></p>
    <img id="territory-flag" src="" alt="Flag">
  </div>

  <script>
    const canvas = document.getElementById('map-overlay');
    const ctx = canvas.getContext('2d');
    const img = document.getElementById('base-map');
    const infoName = document.getElementById('territory-name');
    const infoRegion = document.getElementById('territory-region');
    const infoOwner = document.getElementById('territory-owner');
    const infoFlag = document.getElementById('territory-flag');

    let territories = [
      {
        "id": "T1",
        "name": "Frost Keep",
        "region": "Frost Frontier",
        "owner": "Ice Warriors",
        "flag": "https://via.placeholder.com/40x25/1e90ff/ffffff?text=IW",
        "coordinates": [350, 420],
        "special": true
      },
      {
        "id": "T2",
        "name": "Molten Ridge",
        "region": "Molten Highlands",
        "owner": "Red Rebellion",
        "flag": "https://via.placeholder.com/40x25/ff4040/ffffff?text=RR",
        "coordinates": [620, 470],
        "special": false
      }
    ];

    function drawFlags() {
      const mapRect = img.getBoundingClientRect();
      canvas.width = mapRect.width;
      canvas.height = mapRect.height;
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      territories.forEach(t => {
        const [x, y] = t.coordinates;
        const flag = new Image();
        flag.src = t.flag;
        flag.onload = () => ctx.drawImage(flag, x - 20, y - 20, 40, 25);
      });
    }

    document.getElementById('base-map').addEventListener('click', (e) => {
      const rect = e.target.getBoundingClientRect();
      const x = e.clientX - rect.left;
      const y = e.clientY - rect.top;

      let nearest = territories.reduce((a, b) => {
        const distA = Math.hypot(x - a.coordinates[0], y - a.coordinates[1]);
        const distB = Math.hypot(x - b.coordinates[0], y - b.coordinates[1]);
        return distA < distB ? a : b;
      });

      infoName.textContent = nearest.name;
      infoRegion.textContent = nearest.region;
      infoOwner.textContent = nearest.owner;
      infoFlag.src = nearest.flag;
    });

    img.onload = drawFlags;
  </script>
</body>
</html>
 
