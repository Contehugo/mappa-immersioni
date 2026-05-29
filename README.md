<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>Mappa Naturalistica</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <link rel="stylesheet" href="https://unpkg.com/leaflet-search@3.0.9/dist/leaflet-search.min.css" />
    <style>
        body, html { height: 100%; margin: 0; padding: 0; overflow: hidden; }
        #map { position: absolute; top: 0; bottom: 0; left: 0; right: 0; }
        .popup-img { width: 200px; height: auto; border-radius: 8px; margin-top: 5px; }
        
        /* Trasparenza icone */
        .leaflet-marker-icon { background: transparent !important; border: none !important; box-shadow: none !important; }
        
        /* Stile barra di ricerca */
        .leaflet-control-search .search-input { border: 1px solid #ccc; }
    </style>
</head>
<body>
    <div id="map"></div>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script src="https://unpkg.com/leaflet-search@3.0.9/dist/leaflet-search.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>

    <script>
        var map = L.map('map').setView([43.55, 10.31], 8);

        L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
            attribution: 'Tiles &copy; Esri'
        }).addTo(map);

        var markersLayer = new L.LayerGroup();
        map.addLayer(markersLayer);

        // Inizializzazione Ricerca
        var searchControl = new L.Control.Search({
            layer: markersLayer,
            initial: false,
            zoom: 12,
            propertyName: 'title', 
            textPlaceholder: 'Cerca per nome...'
        });
        map.addControl(searchControl);

        var csvUrl = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vTWKciLxTbcrogqHG8a4vZgNZmSR0ft_V-clBv3u3-q4Ock9FYC-Yk4P80AaX1BE7mkwCJCjNwgIJkz/pub?gid=0&single=true&output=csv';

        Papa.parse(csvUrl, {
            download: true,
            header: true,
            skipEmptyLines: true,
            complete: function(results) {
                results.data.forEach(function(row) {
                    var lat = row.Latitudine ? parseFloat(row.Latitudine.replace(',', '.')) : null;
                    var lng = row.Longitudine ? parseFloat(row.Longitudine.replace(',', '.')) : null;
                    
                    if (lat && lng) {
                        var myIcon = (row.Icona && row.Icona.trim() !== "") ? 
                                     L.icon({iconUrl: row.Icona.trim(), iconSize: [40, 40], iconAnchor: [20, 20]}) : 
                                     new L.Icon.Default();

                        // Il 'title' permette alla lente di cercare il nome correttamente
                        var marker = L.marker([lat, lng], {
                            icon: myIcon,
                            title: row.Nome || "Senza nome"
                        });
                        
                        var popup = "<b>" + (row.Nome || "Senza nome") + "</b><br>" + (row.Descrizione || "");
                        if (row.Foto && row.Foto.trim() !== "") {
                            popup += "<br><img src='" + row.Foto.trim() + "' class='popup-img'>";
                        }
                        marker.bindPopup(popup);
                        markersLayer.addLayer(marker);
                    }
                });
            }
        });
    </script>
</body>
</html>
