<script>

    import mapboxgl from "mapbox-gl"
    import "../../node_modules/mapbox-gl/dist/mapbox-gl.css"
    import { onMount } from "svelte";

    // --- Settin mapbox up ---
    mapboxgl.accessToken = "pk.eyJ1IjoiZGRhbmllbGRtYSIsImEiOiJjbWFwaGI3a2MwaWYzMmlvcW5oY2RvanpkIn0.LK2q7zGz4xf741-KEr_CQw"

    // onMount
    onMount(() => {
        let map = new mapboxgl.Map({
            container: 'map', //binding to div
            style: 'mapbox://styles/mapbox/streets-v12',
            zoom: 12,
            center: [-71.0788727679991, 42.36199480654666]
        })
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