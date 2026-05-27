<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>Mappa Immersione</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>
    <style>
        body, html { height: 100%; margin: 0; padding: 0; overflow: hidden; }
        #map { height: 100vh; width: 100vw; }
    </style>
</head>
<body>
    <div id="map"></div>
    <script>
        var map = L.map('map').setView([43.55, 10.31], 8);
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

        var csvUrl = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vTWKciLxTbcrogqHG8a4vZgNZmSR0ft_V-clBv3u3-q4Ock9FYC-Yk4P80AaX1BE7mkwCJCjNwgIJkz/pub?gid=0&single=true&output=csv';

        Papa.parse(csvUrl, {
            download: true,
            header: true,
            complete: function(results) {
                results.data.forEach(function(row) {
                    // Assicurati che le colonne nel foglio si chiamino esattamente: Latitudine, Longitudine, Nome, Descrizione
                    if (row.Latitudine && row.Longitudine) {
                        var lat = parseFloat(row.Latitudine.replace(',', '.'));
                        var lng = parseFloat(row.Longitudine.replace(',', '.'));
                        L.marker([lat, lng]).addTo(map)
                         .bindPopup("<b>" + row.Nome + "</b><br>" + row.Descrizione);
                    }
                });
            }
        });
    </script>
</body>
</html>
