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
                if (row.IconaURL && row.IconaURL.trim() !== "") {
                    var iconaPersonalizzata = L.icon({
                        iconUrl: row.IconaURL.trim(),
                        iconSize: [40, 40],
                        iconAnchor: [20, 20]
                    });
                    marker = L.marker([lat, lng], {icon: iconaPersonalizzata, title: row.Nome});
                } else {
                    marker = L.marker([lat, lng], {title: row.Nome});
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
