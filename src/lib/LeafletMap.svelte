<script>
    
    import { onMount } from "svelte";
    import { browser } from '$app/env';
    import "leaflet/dist/leaflet.css";

    import { features } from "../data/regions.json";
    import { countries } from "../data/europe.json";
    let map;
    let info;
    let geojsonfrance;
    
    onMount(async() => {
        if (browser) {
            function highlightFeature(e) {
                let layer = e.target;
                layer.setStyle({
                    color: "gray"
                });
                if (!L.Browser.ie && !L.Browser.opera && !L.Browser.edge) {
                    layer.bringToFront();
                }
                info.update(layer.feature.properties);
            }

            function resetHighlight(e) {
                geojsonfrance.resetStyle(e.target);
                info.update();
            }

            function zoomToFeature(e) {
                map.fitBounds(e.target.getBounds());
            }

            function onEachFeature(feature, layer) {
                layer.on({
                    mouseover: highlightFeature,
                    mouseout: resetHighlight,
                    click: zoomToFeature,
                });
            }
            const L = await import('leaflet');
            map = L.map('map').setView([46.65, 2], 5);
            var myIcon = L.icon({
                iconUrl: '../../static/meteo.png',
                // iconSize: [38, 75],
                iconAnchor: [30, 45],
                popupAnchor: [-3, -76],
                shadowSize: [68, 95],
                shadowAnchor: [22, 94]
            });
            L.tileLayer("http://{s}.tile.osm.org/{z}/{x}/{y}.png", {
                maxZoom: 18,
                minZoom: 6,
            }).addTo(map);
            L.geoJSON(countries,  { fillColor: "#E2F4FD", color: "#E2F4FD", weight: 2, fillOpacity: 1 }).addTo(map);
            features.forEach(element => {
                geojsonfrance = L.geoJSON(element, {
                    style: { fillColor: "#FFCD5A", color:"#FFE099", weight: 2, fillOpacity: 1 },
                    onEachFeature: onEachFeature,
                }).addTo(map);
            });
            L.marker([48.835797, 2.39502], {icon: myIcon}).addTo(map);
            L.marker([48.122101, -1.691895], {icon: myIcon}).addTo(map);
            L.marker([46.149394, -1.120605], {icon: myIcon}).addTo(map);
            L.marker([48.385442, -4.493408], {icon: myIcon}).addTo(map);
            L.marker([45.752193, 4.833984], {icon: myIcon}).addTo(map);
            L.marker([43.325178, 5.383301], {icon: myIcon}).addTo(map);
            L.marker([50.611132, 3.054199], {icon: myIcon}).addTo(map);
            L.marker([48.545705, 7.734375], {icon: myIcon}).addTo(map);
            L.control.zoom({
                position: 'bottomleft'
            }).addTo(map);
        }
    })


</script>

<div class="content">
    <div id="map"></div>
</div>

<style>
    
    .content {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    #map { 
        margin-top: 2em;
        height: 700px;
        width: 800px;
        background: linear-gradient(#4BA6FF, #D6CFF0);
     }
    
</style>