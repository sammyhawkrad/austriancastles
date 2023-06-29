<script setup>
import InfoBox from './InfoBox.vue';
import Fuse from "fuse.js"
import { ref, onMounted } from 'vue';
import "leaflet/dist/leaflet.css";
import * as L from "leaflet";
import "../assets/leaflet.fusesearch.css";
import "../assets/Leaflet.AnimatedSearchBox.js";
import "../assets/Leaflet.AnimatedSearchBox.css";
import "@maptiler/geocoding-control/style.css";
import "leaflet.markercluster/dist/MarkerCluster.Default.css";
import "leaflet.markercluster/dist/MarkerCluster.css";
import "leaflet.markercluster/dist/leaflet.markercluster";
import "leaflet.featuregroup.subgroup";


const clickedCastle = ref({})
const infoboxVisible = ref(false)
const geoserver = 'http://geoserver--vxkp129.bluemoss-ee5ab993.westus2.azurecontainerapps.io/geoserver/lbs/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=lbs%3Aaustriancastles&outputFormat=application%2Fjson'
const defensiveMarker = L.icon({
  iconUrl: 'src/assets/pin-defensive.svg',
  iconSize: [25, 41]
});

const fortressMarker = L.icon({
  iconUrl: 'src/assets/pin-fortress.svg',
  iconSize: [25, 41]
});

onMounted(() => {

  const apiKey = "3kTwvEjW6co3cJtocvxH";

  const map = L.map(document.getElementById("map")).setView([47.691, 13.388], 8);

  const scale = devicePixelRatio > 1.5 ? "@2x" : ""; // can change this

  map.fitBounds([[45.77579087036024, 7.899644901157423],[49.981425155587345, 19.77734080646181]]);

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

  const loadData = async () => {
    const response = await fetch(geoserver);
    const data = await response.json();
    return data
  }
  
  const addCastles = async () => {
    const data = await loadData();
    
    const markers = L.markerClusterGroup()
    const defensiveLayer = L.featureGroup.subGroup(markers).addTo(map);
    const fortressLayer = L.featureGroup.subGroup(markers).addTo(map);
    const overlayMaps = {
      'Defensive': defensiveLayer,
      "Fortresses": fortressLayer
    };
    L.control.layers(null, overlayMaps, {position: 'topleft'}).addTo(map);

    const searchbox = L.control.searchbox({
    position: 'topleft',
    expand: 'left',
  }).addTo(map);

    const castlesnames = data.features.map(({properties}) => properties.name)
    const fuse = new Fuse(castlesnames, {
        shouldSort: true,
        threshold: 0.6,
        location: 0,
        distance: 100,
        minMatchCharLength: 1
    });

    searchbox.onInput("keyup", function (e) {
        if (e.keyCode == 13) {
            search();
        } else {
            const value = searchbox.getValue();
            if (value != "") {
                const results = fuse.search(value);
                searchbox.setItems(results.map(res => res.item).slice(0, 5));
            } else {
                searchbox.clearItems();
            }
        }
    });


    function search() {
      setTimeout(function () {
                searchbox.hide();
                searchbox.clear();
            }, 600);
      const value = searchbox.getValue();
      console.log("value is " + value)
      const searchedItem = data.features.filter((item) => item.properties['name'] === value)
      const searchedItemCoordinates = searchedItem[0].geometry.coordinates
      console.log(`Searched item: ${searchedItem[0].geometry.coordinates}`)
      map.setView([searchedItemCoordinates[1], searchedItemCoordinates[0]], 17)
    }


    const castles = L.geoJSON(data, {
      onEachFeature: function (feature, layer) {
        layer.bindTooltip(feature.properties.name, {
            direction: 'top', 
            offset: [0, -25], 
            className: 'tooltip'
          });

        feature.layer = layer;
          
        if(feature.properties.castle_type === "defensive" || feature.properties.castle_type === "defensive;stately") {
          defensiveLayer.addLayer(layer);
          layer.setIcon(defensiveMarker);
        } else {
          fortressLayer.addLayer(layer)
          layer.setIcon(fortressMarker);
        }
          
        layer.on('click', function() {
          clickedCastle.value = feature.properties;
          infoboxVisible.value = true;
        })
        
      }
    });
    
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

.marker-cluster-small {
  background-color: #aa4d4499;
}
.marker-cluster-small div {
  background-color: #aa4d4499;
}

.marker-cluster-medium {
    background-color: #723d4699;
}
.marker-cluster-medium div {
  background-color: #723d4699;
}

.marker-cluster-large {
  background-color: #472d3099;
}
.marker-cluster-large div {
  background-color: #472d3099;
}

.marker-cluster span {
  color: white;
}

.tooltip {
  background: #ffe1a8;
  border: #ffe1a8;
  color: 2px solid #472d30;
  border-top-color: #ffe1a8
}

.leaflet-tooltip-top:before {
  border-top-color: #ffe1a8
}


.leaflet-retina .leaflet-control-layers-toggle { 
    background-image: url('../assets/layers.svg');
    width: 33px;
    height: 33px;

}

</style>
