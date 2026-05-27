<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mappa Immersione</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <style>
        /* Forza la pagina a non avere margini e occupare tutto lo spazio */
        body, html { 
            height: 100%; 
            margin: 0; 
            padding: 0; 
            overflow: hidden; 
        }
        #map { 
            height: 100vh; 
            width: 100vw; 
        }
    </style>
</head>
<body>

    <div id="map"></div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script>
        // Inizializza la mappa su una posizione di default (es. Livorno)
        var map = L.map('map').setView([43.55, 10.31], 10);

        // Aggiunge la base della mappa (OpenStreetMap)
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '© OpenStreetMap contributors'
        }).addTo(map);

        // Aggiungi un Marker di esempio
        var marker = L.marker([43.55, 10.31]).addTo(map);
        marker.bindPopup("<b>Avvistamento Test</b><br>Dettagli qui.");
    </script>
</body>
</html>
