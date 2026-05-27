<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Mappa Dati</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>
    <style>
        body, html { height: 100%; margin: 0; padding: 0; }
        #map { height: 100vh; width: 100vw; }
    </style>
</head>
<body>
    <div id="map"></div>
    <script>
        var map = L.map('map').setView([43.55, 10.31], 8);
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

        // Inserisci QUI il link al tuo file CSV (File > Condividi > Pubblica sul web > CSV)
        var csvUrl = 'URL_DEL_TUO_CSV_PUBBLICO_QUI';

        Papa.parse(csvUrl, {
            download: true,
            header: true,
            complete: function(results) {
                results.data.forEach(function(row) {
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
