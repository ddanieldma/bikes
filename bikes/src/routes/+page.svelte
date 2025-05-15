<script>

    import mapboxgl from "mapbox-gl"
    import "../../node_modules/mapbox-gl/dist/mapbox-gl.css"
    import { onMount } from "svelte";

    // --- Settin mapbox up ---
    mapboxgl.accessToken = "pk.eyJ1IjoiZGRhbmllbGRtYSIsImEiOiJjbWFwaGI3a2MwaWYzMmlvcW5oY2RvanpkIn0.LK2q7zGz4xf741-KEr_CQw"

    // --- Variables ---
    let map
    
    // --- Initiating the map and adding bike lanes data ---
    async function initMap() {
        map = new mapboxgl.Map({
            container: 'map', //binding to div
            style: 'mapbox://styles/mapbox/streets-v12',
            zoom: 12,
            center: [-71.0788727679991, 42.36199480654666]
        });

        await new Promise(resolve => map.on("load", resolve));

        // Adding Boston bike lanes data
        const responseBoston = await fetch("/data/Existing_Bike_Network_2022.geojson"); // Adjust the path if your file name is different
        const bikeLaneDataBoston = await responseBoston.json();
        map.addSource("boston_route", {
            type: "geojson",
            data: bikeLaneDataBoston,
        });
        
        // Adding Cambridge bike lanes data
        const responseCambridge = await fetch("/data/cambridge_bike.geojson"); // Adjust the path if your file name is different
        const bikeLaneDataCambridge = await responseCambridge.json();
        map.addSource("cambridge_route", {
            type: "geojson",
            data: bikeLaneDataCambridge,
        });

        map.addLayer({
            id: "bike_lanes_layer_boston",
            type: "line", // one of the supported layer types, e.g. line, circle, etc.
            source: "boston_route", // The id we specified in `addSource()`
            paint: {
                "line-color": "green",
                "line-opacity": 0.4,
                "line-width": 3
            },
        });
        map.addLayer({
            id: "bike_lanes_layer_cambridge",
            type: "line", // one of the supported layer types, e.g. line, circle, etc.
            source: "cambridge_route", // The id we specified in `addSource()`
            paint: {
                "line-color": "green",
                "line-opacity": 0.4,
                "line-width": 3
            },
        });
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