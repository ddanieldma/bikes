<script>

    import mapboxgl from "mapbox-gl"
    import "../../node_modules/mapbox-gl/dist/mapbox-gl.css"
    import { onMount } from "svelte";

    // --- Settin mapbox up ---
    mapboxgl.accessToken = "pk.eyJ1IjoiZGRhbmllbGRtYSIsImEiOiJjbWFwaGI3a2MwaWYzMmlvcW5oY2RvanpkIn0.LK2q7zGz4xf741-KEr_CQw"

    // --- Variables ---
    let map

    async function initMap() {
        map = new mapboxgl.Map({
            container: 'map', //binding to div
            style: 'mapbox://styles/mapbox/streets-v12',
            zoom: 12,
            center: [-71.0788727679991, 42.36199480654666]
        })

        await new Promise(resolve => map.on("load", resolve))

        map.addSource("boston_route", {
            type: "geojson",
            data: "https://bostonopendata-boston.opendata.arcgis.com/datasets/boston::existing-bike-network-2022.geojson?outSR=%7B%22latestWkid%22%3A3857%2C%22wkid%22%3A102100%7D",
        })
    }
    
    // onMount
    onMount(() => {
        initMap()
    })
    

</script>

<div id="map">

</div>

<style>
    @import url("$lib/global.css");

    #map {
        flex: 1;
    }
</style>