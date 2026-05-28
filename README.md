<html lang="ru">

<head>
    <button onclick="location.href='index.html'">
  Назад
</button>
    <meta charset="UTF-8" />
    <title>Моё местоположение</title>

    <!-- Leaflet CSS -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />

    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            text-align: center;
        }
        
        #map {
            height: 90vh;
            width: 100%;
        }
        
        #status {
            padding: 10px;
            background: lightcyan;
            color: rgba(0, 0, 0, 0.591);
        }
    </style>
</head>

<body>

    <div id="status">Определяем местоположение...</div>
    <div id="map"></div>

    <!-- Leaflet JS -->
    <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>

    <script>
        let map;
        let marker;

        function initMap(lat, lng) {
            map = L.map('map').setView([lat, lng], 15);

            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                maxZoom: 19,
            }).addTo(map);

            marker = L.marker([lat, lng]).addTo(map)
                .bindPopup("Ты здесь :)")
                .openPopup();
        }

        function updatePosition(position) {
            const lat = position.coords.latitude;
            const lng = position.coords.longitude;

            document.getElementById("status").innerText =
                `Широта: ${lat.toFixed(6)}, Долгота: ${lng.toFixed(6)}`;

            if (!map) {
                initMap(lat, lng);
            } else {
                marker.setLatLng([lat, lng]);
                map.setView([lat, lng]);
            }
        }

        function error() {
            document.getElementById("status").innerText =
                "Не удалось получить местоположение :(";
        }

        if (navigator.geolocation) {
            navigator.geolocation.watchPosition(updatePosition, error, {
                enableHighAccuracy: true
            });
        } else {
            document.getElementById("status").innerText =
                "Геолокация не поддерживается браузером";
        }
    </script>

</body>

</html>
