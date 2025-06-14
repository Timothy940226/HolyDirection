<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <title>聖墓教堂方向指南</title>
  <style>
    body { font-family: sans-serif; text-align: center; padding: 40px; }
    #arrow { font-size: 80px; transform-origin: center center; display: block; margin: 20px auto; transition: transform 0.5s ease; }
  </style>
</head>
<body>
  <h1>面對耶路撒冷聖墓教堂的方向</h1>
  <p>目前位置：<span id="location">取得中...</span></p>
  <p>聖墓教堂方位角：<span id="bearing">計算中...</span>°</p>
  <div id="arrow">🧭</div>
  <p>請轉身面向🧭方向，即面向聖墓教堂</p>

  <script>
    // 聖墓教堂座標（耶路撒冷）
    const targetLat = 31.7784;
    const targetLng = 35.2294;

    function toRadians(deg) {
      return deg * Math.PI / 180;
    }

    function toDegrees(rad) {
      return rad * 180 / Math.PI;
    }

    function computeBearing(lat1, lon1, lat2, lon2) {
      const φ1 = toRadians(lat1);
      const φ2 = toRadians(lat2);
      const Δλ = toRadians(lon2 - lon1);

      const y = Math.sin(Δλ) * Math.cos(φ2);
      const x = Math.cos(φ1) * Math.sin(φ2) -
                Math.sin(φ1) * Math.cos(φ2) * Math.cos(Δλ);
      let θ = Math.atan2(y, x);
      return (toDegrees(θ) + 360) % 360;
    }

    function updateArrow(deg) {
      document.getElementById('arrow').style.transform = `rotate(${deg}deg)`;
    }

    navigator.geolocation.getCurrentPosition(
      pos => {
        const lat = pos.coords.latitude;
        const lng = pos.coords.longitude;
        document.getElementById("location").textContent = `${lat.toFixed(5)}, ${lng.toFixed(5)}`;

        const bearing = computeBearing(lat, lng, targetLat, targetLng);
        document.getElementById("bearing").textContent = bearing.toFixed(2);
        updateArrow(bearing);
      },
      err => {
        document.getElementById("location").textContent = "定位失敗：" + err.message;
      }
    );
  </script>
</body>
</html>
