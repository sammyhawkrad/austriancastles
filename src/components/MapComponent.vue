<script setup>
import { onMounted } from 'vue'
import "leaflet/dist/leaflet.css"
import L from "leaflet"


onMounted(() => {
  const map = L.map('map').setView([47.691, 13.388], 8);
  map.setMaxBounds(map.getBounds());

  L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
    minZoom: 8,
  }).addTo(map);

  L.control.scale({position:'bottomleft', metric: true, imperial: false}).addTo(map);

  // L.Control.geocoder({
  //   geocoder: L.Control.Geocoder.nominatim({
  //       geocodingQueryParams: {countrycodes: 'at'}
  //   })
  // }).addTo(map);

  // var options = {
  //       position: 'topright',
  //       geocoder: new L.Control.Geocoder.nominatim({
  //           geocodingQueryParams: {
  //               "countrycodes": "at"
  //           }
  //       })
  //   };

  //  L.Control.geocoder(options).addTo(map);
  // console.log(map.getBounds())

})


</script>

<template>
  <div id="map"></div>
</template>

<style scoped>

</style>
