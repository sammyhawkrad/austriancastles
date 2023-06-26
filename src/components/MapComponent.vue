<script setup>
import InfoBox from './InfoBox.vue';
// import "fuse.js/dist/fuse.js";
// import "fuse.js/dist/fuse-min.js";
import Fuse from "fuse.js"
import { ref, onMounted } from 'vue';
import "leaflet/dist/leaflet.css";
import * as L from "leaflet";
import * as myfuseSearch from "../assets/leaflet.fusesearch.js";
import "../assets/leaflet.fusesearch.css";
// import "../assets/zoom-mobile.js";

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
  iconUrl: 'src/assets/pin-defensive.svg',
  iconSize: [25, 41]
});

const fortressMarker = L.icon({
  iconUrl: 'src/assets/pin-fortress.svg',
  iconSize: [25, 41]
});

onMounted(() => {

  // let recaptchaScript = document.createElement('script')
  // recaptchaScript.setAttribute('src', '/assets/leaflet.fusesearch.js')
  // document.head.appendChild(recaptchaScript)

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

  // if (L.Browser.mobile) {
  //   map.removeControl(map.zoomControl);
  // }
  
  
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
        // layer.bindTooltip("<div style='background:blue';>" + feature.properties.name + "</div>", {direction: 'top', offset: [0, -25]});
        layer.bindTooltip(feature.properties.name, {
            direction: 'top', 
            offset: [0, -25], 
            className: 'tooltip'
          });

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
      }
    });
    
    markers.addLayer(castles)
    map.addLayer(markers)

    var overlayMaps = {
      "Castles": castlesLayer,
      "Fortresses": fortressLayer
    };

    // L.Control.Layers.TogglerIcon = L.Control.Layers.extend({
    //   options: {
    //       // Optional base CSS class name for the toggler element
    //       togglerClassName: undefined
    //   },

    //   _initLayout: function(){
    //       L.Control.Layers.prototype._initLayout.call(this);
    //       if (this.options.togglerClassName) {
    //           L.DomUtil.addClass(this._layersLink, togglerClassName);
    //       }
    //   }
    // });

    // var layerCastleTypes = new L.Control.Layers.TogglerIcon(null, overlayMaps, {togglerClassName: 'layers-castletypes'});
    // L.control.layers(null, overlayMaps, {togglerClassName: 'layers-castletypes'}).addTo(map);
    // layerCastleTypes.addTo(map);

    L.control.layers(null, overlayMaps).addTo(map);
    
    // search
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

  .layers-castletypes {
    background-image: url('../assets/layers.svg');
    width: 36px;
      height: 36px;
  }

  .leaflet-control-layers-toggle {
    background-image: url('../assets/layers.svg');
    width: 36px;
    height: 36px;
}

</style>
