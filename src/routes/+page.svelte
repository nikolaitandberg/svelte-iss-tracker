<svelte:head>
    <!-- insert your google maps api-key here -->
    <script async defer 
        src="https://maps.googleapis.com/maps/api/js?key=YOUR-API-KEY">
    </script>
</svelte:head>

<script>
    import { onMount } from 'svelte';

    // declaration of variables for the map itself
    let url = "http://api.open-notify.org/iss-now.json";
    let data;
    let container;  
    let map;       
    let zoom = 4; 

    async function plot(){
        const res = await fetch(url);
        data = await res.json();

        let markerPosition = {lat: Number(data.iss_position.latitude), lng: Number(data.iss_position.longitude)};

        const marker = new google.maps.Marker({
            position: markerPosition,
            map: map,
        });
    }
    
    
    onMount(async () => {

        const res = await fetch(url);
        data = await res.json();

        let center = { lat: Number(data.iss_position.latitude), lng: Number(data.iss_position.longitude)}; 
        let markerPosition = {lat: Number(data.iss_position.latitude), lng: Number(data.iss_position.longitude)};

        // The map object itself
        map = new google.maps.Map(container, {
            zoom,
            center,
            styles: { "elementType": "geometry"}
        });

        // Marker for ISS
        const marker = new google.maps.Marker({
            position: markerPosition,
            map: map,
        });
        // plot the ISS position every 10 seconds
        setInterval(plot,10000)
    });

    
</script>

<h2>Google maps API ISS Position</h2>
<p>Remember to insert your own api key in line 4 of the +page.svelte file, you're not getting mine ;)</p>
<div class="map" bind:this={container}></div>

<style>
    .map{
        width: 60vw;
        height: 60vh;
        margin-left: auto;
        margin-right: auto;
        display: block;
        border: 5px solid red;
    }
</style>