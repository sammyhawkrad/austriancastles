<script setup>
import { onMounted } from 'vue';
import "leaflet/dist/leaflet.css";
import L from "leaflet";


const geoserver = 'http://geoserver--vxkp129.bluemoss-ee5ab993.westus2.azurecontainerapps.io/geoserver/lbs/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=lbs%3Aaustriancastles&outputFormat=application%2Fjson'



onMounted(() => {
  const map = L.map('map').setView([47.691, 13.388], 8);
  map.fitBounds([[49.824, 19.666],[46.453, 8.498]]);

  L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
    minZoom: 7,
  }).addTo(map);

  L.control.scale({position:'bottomleft', metric: true, imperial: false}).addTo(map);


  const loadData = async () => {
    const response = await fetch(geoserver);
    const data = await response.json();
    return data
  }

  const addCastles = async () => {
    const castles = await loadData();
    L.geoJSON(castles).addTo(map);

  }
  addCastles();

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
