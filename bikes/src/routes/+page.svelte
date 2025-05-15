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

        try {
            const response = await fetch("/data/Existing_Bike_Network_2022.geojson"); // Adjust the path if your file name is different
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            const bikeLaneData = await response.json();

            console.log("bikelanedata", bikeLaneData)

            map.addSource("boston_route", {
                type: "geojson",
                data: bikeLaneData,
            });
        } catch (error) {
            console.error("Error loading local bike lane data:", error);
        }

        map.addLayer({
            id: "bike_lanes_layer",
            type: "line", // one of the supported layer types, e.g. line, circle, etc.
            source: "boston_route", // The id we specified in `addSource()`
            paint: {
                "line-color": "black",
                "line-width": 2
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