<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>Mappa con Ricerca</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <link rel="stylesheet" href="https://unpkg.com/leaflet-search@3.0.9/dist/leaflet-search.min.css" />
    <style>
        body, html { height: 100%; margin: 0; padding: 0; }
        #map { height: 100vh; width: 100vw; }
        .popup-img { width: 200px; height: auto; border-radius: 8px; margin-top: 5px; }
    </style>
</head>
<body>
    <div id="map"></div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script src="https://unpkg.com/leaflet-search@3.0.9/dist/leaflet-search.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>

    <script>
        var map = L.map('map').setView([43.55, 10.31], 8);
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

        var markersLayer = new L.LayerGroup();
        map.addLayer(markersLayer);
        // Aggiungi questo subito DOPO la definizione del 'markersLayer'
var customIcon = L.divIcon({
    className: 'custom-pin',
    html: '📍', // Puoi cambiare questa emoji con quello che preferisci
    iconSize: [25, 25],
    iconAnchor: [12, 25]
});

        // Aggiungiamo il controllo di ricerca subito, ma vuoto
        var searchControl = new L.Control.Search({
            layer: markersLayer,
            initial: false,
            zoom: 12,
            textPlaceholder: 'Cerca nome animale...'
        });
        map.addControl(searchControl);

        var csvUrl = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vTWKciLxTbcrogqHG8a4vZgNZmSR0ft_V-clBv3u3-q4Ock9FYC-Yk4P80AaX1BE7mkwCJCjNwgIJkz/pub?gid=0&single=true&output=csv';

       // E modifica il pezzo dentro il ciclo Papa.parse così:
results.data.forEach(function(row) {
    var lat = row.Latitudine ? parseFloat(row.Latitudine.replace(',', '.')) : null;
    var lng = row.Longitudine ? parseFloat(row.Longitudine.replace(',', '.')) : null;
    
    if (lat && lng) {
        // Usiamo l'icona personalizzata qui
        var marker = L.marker([lat, lng], {icon: customIcon, title: row.Nome});
        
        var popupContent = "<b>" + (row.Nome || "Senza nome") + "</b><br>" + (row.Descrizione || "");
        if (row.Foto && row.Foto.trim() !== "") {
            popupContent += "<br><img src='" + row.Foto.trim() + "' class='popup-img'>";
        }
        
        marker.bindPopup(popupContent);
        markersLayer.addLayer(marker);
    }
});
    </script>
</body>
</html>
