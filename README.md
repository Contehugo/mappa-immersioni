<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <title>Mappa Naturalistica</title>
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

        // Mappa Satellitare
        L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
            attribution: 'Tiles &copy; Esri &mdash; Source: Esri, i-cubed, USDA, USGS, AEX, GeoEye, Getmapping, Aerogrid, IGN, IGP, UPR-EGP, and the GIS User Community'
        }).addTo(map);

        var markersLayer = new L.LayerGroup();
        map.addLayer(markersLayer);

        var searchControl = new L.Control.Search({
            layer: markersLayer,
            initial: false,
            zoom: 12,
            textPlaceholder: 'Cerca...'
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
                        var marker;
                        
                        // Se esiste IconaURL, usiamo quella, altrimenti un pallino di default
                        if (row.IconaURL && row.IconaURL.trim() !== "") {
                            var iconaPersonalizzata = L.icon({
                                iconUrl: row.IconaURL.trim(),
                                iconSize: [40, 40], // Regola qui la dimensione
                                iconAnchor: [20, 20]
                            });
                            marker = L.marker([lat, lng], {icon: iconaPersonalizzata, title: row.Nome});
                        } else {
                            // Pallino blu di fallback (senza bordo bianco)
                            marker = L.marker([lat, lng], {
                                icon: L.divIcon({
                                    className: 'my-custom-pin', 
                                    iconSize: [15, 15]
                                })
                            });
                        }
                        
                        var popupContent = "<b>" + (row.Nome || "Senza nome") + "</b><br>" + (row.Descrizione || "");
                        if (row.Foto && row.Foto.trim() !== "") {
                            popupContent += "<br><img src='" + row.Foto.trim() + "' class='popup-img'>";
                        }
                        marker.bindPopup(popupContent);
                        markersLayer.addLayer(marker);
                    }
                });
            }
        });
    </script>
</body>
</html>
