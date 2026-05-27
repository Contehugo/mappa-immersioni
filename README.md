<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>Mappa Immersione Finale</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>
    <style>
        body, html { height: 100%; margin: 0; padding: 0; overflow: hidden; }
        #map { height: 100vh; width: 100vw; }
        .popup-img { width: 200px; height: auto; border-radius: 8px; margin-top: 5px; display: block; }
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
            skipEmptyLines: true,
            complete: function(results) {
                results.data.forEach(function(row) {
                    // Pulizia nomi colonne: cerchiamo la colonna Foto ignorando spazi extra
                    var lat = row.Latitudine ? parseFloat(row.Latitudine.replace(',', '.')) : null;
                    var lng = row.Longitudine ? parseFloat(row.Longitudine.replace(',', '.')) : null;
                    
                    // Cerchiamo la chiave 'Foto' anche se ci fossero spazi nel CSV
                    var fotoKey = Object.keys(row).find(key => key.trim() === 'Foto');
                    var fotoUrl = fotoKey ? row[fotoKey] : "";

                    if (lat && lng) {
                        var popupContent = "<b>" + (row.Nome || "Senza nome") + "</b><br>" + (row.Descrizione || "");
                        
                        if (fotoUrl && fotoUrl.trim() !== "") {
                            popupContent += "<br><img src='" + fotoUrl.trim() + "' class='popup-img' onerror='this.style.display=\"none\"'>";
                        }

                        L.marker([lat, lng]).addTo(map).bindPopup(popupContent);
                    }
                });
            }
        });
    </script>
</body>
</html>
