<script setup>
import InfoBox from './InfoBox.vue';
// import "fuse.js/dist/fuse.js";
// import "fuse.js/dist/fuse-min.js";
import Fuse from "fuse.js"
import { ref, onMounted } from 'vue';
import "leaflet/dist/leaflet.css";
import * as L from "leaflet";
import "../assets/leaflet.fusesearch.js";
import "../assets/leaflet.fusesearch.css";

import "../assets/Leaflet.AnimatedSearchBox.js";
import "../assets/Leaflet.AnimatedSearchBox.css";
import { GeocodingControl } from "@maptiler/geocoding-control/leaflet";
import "@maptiler/geocoding-control/style.css";
import "leaflet.markercluster/dist/MarkerCluster.Default.css";
import "leaflet.markercluster/dist/MarkerCluster.css";
import "leaflet.markercluster/dist/leaflet.markercluster";
import "leaflet.featuregroup.subgroup";

const clickedCastle = ref({})
const infoboxVisible = ref(false)
const geoserver = 'http://geoserver--vxkp129.bluemoss-ee5ab993.westus2.azurecontainerapps.io/geoserver/lbs/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=lbs%3Aaustriancastles&outputFormat=application%2Fjson'
const castleMarker = L.icon({
  iconUrl: 'src/assets/pin.svg',
  iconSize: [25, 41]
});

const fortressMarker = L.icon({
  iconUrl: 'src/assets/castlelocation.svg',
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

  // //first search
  // var searchCtrl = L.control.fuseSearch()
  // console.log("fusesearch")
  // searchCtrl.addTo(map);
  
  // second search
  var searchbox = L.control.searchbox({
    position: 'topright',
    expand: 'left'
  }).addTo(map);
  
  
  const addCastles = async () => {
    const data = await loadData();
    
    
    console.log("search ctrl")
    console.log(data)
    // searchCtrl.indexFeatures(data, ['name']); // first search


    var castlesnames = data.features.map(({properties}) => properties.name)
    var fuse = new Fuse(castlesnames, {
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
            var value = searchbox.getValue();
            if (value != "") {
                var results = fuse.search(value);
                searchbox.setItems(results.map(res => res.item).slice(0, 5));
            } else {
                searchbox.clearItems();
            }
        }
    });
    searchbox.onButton("click", search);

    function search() {
      var value = searchbox.getValue();
      if (value != "") {
          var results = fuse.search(value);
          $('#results').text(JSON.stringify(results, null, 2));
          $('html, body').animate({
              // scrollTop: $(".results-container").offset().top
          }, 1000);
      }
    }

    const markers = L.markerClusterGroup()
    var castlesLayer = L.featureGroup.subGroup(markers).addTo(map);
    var fortressLayer = L.featureGroup.subGroup(markers).addTo(map);

    const castles = L.geoJSON(data, {
      onEachFeature: function (feature, layer) {
        layer.bindTooltip(feature.properties.name);
        feature.layer = layer;

        if (feature.properties.name) {
          
          if(feature.properties.castle_type == "defensive" || feature.properties.castle_type == "defensive;stately") {
            console.log("defensive");
            castlesLayer.addLayer(layer);
            layer.setIcon(castleMarker);
          } else {
            fortressLayer.addLayer(layer)
            layer.setIcon(fortressMarker);
          }
        }
        
        
        
        layer.on('click', function() {
          clickedCastle.value = feature.properties;
          infoboxVisible.value = true;
        })
       
        // L.geoJson(data, {
        //   onEachFeature: function (feature, layer) {
        //       feature.layer = layer;
        //   }
        // });
      }
    });
    
    
    // const markers = L.markerClusterGroup()
    markers.addLayer(castles)
    map.addLayer(markers)
    // var castlesLayer = L.featureGroup.subGroup(markers).addTo(map);
    // var fortressLayer = L.featureGroup.subGroup(markers).addTo(map);
    // filter
    // var castlesLayer = L.layerGroup();
    // var fortressLayer = L.layerGroup();
    var overlayMaps = {
      "Castles": castlesLayer,
      "Fortresses": fortressLayer
    };
    L.control.layers(null, overlayMaps).addTo(map);
    
    var options = {
      position: 'topright',
      title: 'Chercher',
      placeholder: 'cdvdf',
      maxResultLength: 7,
      threshold: 0.2,
      showInvisibleFeatures: false,
      showResultFct: function(feature, container) {
          var props = feature.properties,
              name = L.DomUtil.create('b', null, container);
          name.innerHTML = props.name;

          container.appendChild(L.DomUtil.create('br', null, container));
          container.appendChild(document.createTextNode(props.memory));
      }
    };
    var fuseSearchCtrl = L.control.fuseSearch(options);
    fuseSearchCtrl.indexFeatures(data.features, ['name']);
    map.addControl(fuseSearchCtrl);

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
