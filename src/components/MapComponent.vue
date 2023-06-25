<script setup>
import InfoBox from './InfoBox.vue';
import { ref, onMounted } from 'vue';
import "leaflet/dist/leaflet.css";
import * as L from "leaflet";
import { GeocodingControl } from "@maptiler/geocoding-control/leaflet";
import "@maptiler/geocoding-control/style.css";
import "leaflet.markercluster/dist/MarkerCluster.Default.css";
import "leaflet.markercluster/dist/MarkerCluster.css";

import "leaflet.markercluster/dist/leaflet.markercluster";

const clickedCastle = ref({})
const infoboxVisible = ref(false)
const geoserver = 'http://geoserver--vxkp129.bluemoss-ee5ab993.westus2.azurecontainerapps.io/geoserver/lbs/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=lbs%3Aaustriancastles&outputFormat=application%2Fjson'
const castleMarker = L.icon({
  iconUrl: 'src/assets/pin-fortress.svg',
  iconSize: [25, 41]
});

onMounted(() => {

  const apiKey = "3kTwvEjW6co3cJtocvxH";

  const map = L.map(document.getElementById("map")).setView([47.691, 13.388], 8);

  const scale = devicePixelRatio > 1.5 ? "@2x" : ""; // can change this

  map.setMaxBounds(map.getBounds());

  L.tileLayer(
    `https://api.maptiler.com/maps/basic-v2/{z}/{x}/{y}${scale}.png?key=` + apiKey,
    {
      tileSize: 512,
      zoomOffset: -1,
      minZoom: 7,
      maxZoom: 19,
      attribution:
        '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a>, ' +
        '<a href="https://www.openstreetmap.org/copyright" target="_blank">&copy; OpenStreetMap contributors</a>',
      crossOrigin: true,
      country: 'at'
    }
  ).addTo(map);

  L.control.scale({position:'bottomleft', metric: true, imperial: false}).addTo(map);
  

  const maptiler = L.control.maptilerGeocoding({ apiKey });
  maptiler.setPosition('topleft');
  maptiler.addTo(map);


  const loadData = async () => {
    const response = await fetch(geoserver);
    const data = await response.json();
    return data
  }

  const addCastles = async () => {
    const data = await loadData();
    const castles = L.geoJSON(data, {
      onEachFeature: function (feature, layer) {
        layer.bindTooltip(feature.properties.name);

        if (feature.properties.name) {
          layer.setIcon(castleMarker);
        }
        
        layer.on('click', function() {
          clickedCastle.value = feature.properties;
          infoboxVisible.value = true;
        })
        
      }
    });
    const markers = L.markerClusterGroup()
    markers.addLayer(castles)
    map.addLayer(markers)
  }
  addCastles();

})


</script>

<template>
  <div id="map"></div>
  <InfoBox v-if="infoboxVisible" @close='infoboxVisible = false' :clickedCastle="clickedCastle" :infoboxVisible="infoboxVisible"/>
</template>

<style>
  .leaflet-ctrl-geocoder{
    border-color: red;
    border-width: 0.3em;
    border-style: solid;
  }

</style>
