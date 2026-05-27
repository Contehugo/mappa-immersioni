<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Mappa Test</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <style>
        #map { height: 500px; width: 100%; }
        .popup-img { width: 150px; }
    </style>
</head>
<body>
    <div id="map"></div>
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        var map = L.map('map').setView([43.47, 10.33], 8);
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
        
        // Pin di test
        var marker = L.marker([43.47, 10.33]).addTo(map);
        marker.bindPopup("<b>Test Immagine</b><br><img src='https://via.placeholder.com/150' class='popup-img'>");
    </script>
</body>
</html>
