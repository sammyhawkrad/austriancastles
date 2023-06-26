<script setup>
import { ref } from 'vue';
const aboutboxVisible = ref(false);
const openWindow = () => {
  aboutboxVisible.value = true;
};

const closeWindow = () => {
  aboutboxVisible.value = false;
}
</script>

<template>
  <div id="about-button" @click="openWindow">
  <img class="about" alt="about" src="@/assets/about.svg"/>
  </div>
  <div v-if="aboutboxVisible" id="about-box">
    <div id="headline">
        <div id="castleLogo">
          <img class="about2" alt="about" src="@/assets/about.svg" width="40" height="40"/>
        </div>
          <h2>ABOUT</h2>
        <div id="close-button" @click="closeWindow">
          <svg xmlns="http://www.w3.org/2000/svg" width="2em" height="2em" viewBox="0 0 24 24">
            <path fill="#472d30" d="M17.59 5L12 10.59L6.41 5L5 6.41L10.59 12L5 17.59L6.41 19L12 13.41L17.59 19L19 17.59L13.41 12L19 6.41L17.59 5Z"></path>
          </svg>
        </div>
    </div>
    <div id="about-text">
      <h3>The Project</h3>
      <p id="about-paragraph">We built an interactive map of castles in Austria, focusing on two types: fortresses and defensive castles. 
      Instead of learning about castles from various sources, this application combines relevant information in one user-friendly platform. 
      The interactive map gives an overview over the distribution of castles across Austria and provides a better understanding of their historical 
      and cultural significance within a specific area. The map is addressed to tourists and castle enthusiasts who are looking for information 
      about castles they like or might want to visit. </p>
      <h3>Functions</h3>
      <p id="about-paragraph">The map includes interactive elements for an optimal user experience. By clicking on a marker in the map, users get 
      access to information about a castle. They can search for a specific castle and filter by castle type.</p>
      <h3>Tools</h3>
      <p id="about-paragraph">Leaflet, Vue, Geoserver, Postgres, Adobe Illustrator</p>
      <h3>Sources and Licences</h3>
      <p id="link"><a href="https://www.openstreetmap.org/">Openstreetmap</a><br><a href="https://www.wikipedia.org/">Wikipedia</a><br><a href="https://www.wikidata.org/">Wikidata</a></p>
      <br>
      <p id="created-by">Created by: Yi Zhen Chew, Ulrike Holfeld, Samuel Darkwah (TU Wien, 2023)</p>
    </div>
  </div>

  <div id="toolbar-m">
    <img class="layer-m" alt="about" src="@/assets/layers.svg" width="60" height="60"/>
    <img class="search-m" alt="about" src="@/assets/search.svg" width="60" height="60"/>
    <img class="about-m" alt="about" src="@/assets/about.svg" width="60" height="60" @click="openWindow"/>
    </div>
    <div id="buttons">
        <img class="search" alt="search" src="@/assets/search.svg" width="40" height="40"/>
        <img class="layers" alt="about" src="@/assets/layers.svg" width="40" height="40"/>
    </div>
</template>

<style scoped>
#about-button {
  position: absolute;
  z-index: 3;
  width: 40px;
  height: 40px;
  left: 1%;
  top: 87%;
  cursor: pointer;
  border: solid #472d30 2px;
}

#about-box {
  background-color: #ffe1a8;
  position: fixed;
  width: 30%;
  height: 75%;
  top: 8rem;
  right: 2rem;
  z-index: 3;
  border-radius: 2%;
  display: flex;
  flex-direction: column;
}

#headline {
  display: flex;
  justify-content: space-between;
}

#castleLogo {
  padding: 10px;
  align-self: center;
}

#close-button {
  padding: 10px;
  align-self: flex-start;
  cursor: pointer;
  transition: all 0.2s ease-in;
  color: #472d30;
  margin: 0;
}

#close-button>svg:hover{
  border: 2px solid #472d30;
  border-radius: 50%;
}

#headline {
  margin-right: 10px;
}

.about2 {
  margin-right: 10px;
  border: 2px solid #472d30;
}

h2 {
  font-family: MedievalSharp;
  color: #723d46;
  font-weight: bold;
  text-align: center;
  align-self: center;
  padding: 10px;
}

h3 {
  font-family: MedievalSharp;
  color: #723d46;
  font-weight: bold;
  text-align: center;
}

#about-text {
    overflow: auto;
}

#about-paragraph {
  padding: 10px;
  color: #472d30;
}

a {
  padding: 10px;
  color: #472d30;
  font-style: italic;
}

a:hover {
  color: #e26d5c;
}

#created-by {
  font-style: italic;
  padding: 10px;
  color: #472d30;
  font-size: 80%;
}

@media (max-width: 900px) {
  #about-box {
    width: 40%;
  }
}

@media (max-width: 650px) {
  #about-box {
    width: 60%;
  }
}

@media (max-width: 420px) {
    #about-button {
      display: none;
    } 
    #about-box {
      width: 90%;
      height: 76%;
      top: 5.5rem;
      left: 5%;
      right: 5%;
    } 
}

.search {
  top: 15%;
  left: 1%;
  position: absolute;
  z-index: 3;
  cursor: pointer;
  border: solid 2px #472d30;
}

.layers {
  top: 25%;
  left: 1%;
  position: absolute;
  z-index: 3;
  cursor: pointer;
  border: solid 2px #472d30;
}


#toolbar-m {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  background-color: #c8caa3;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 60px;
}

.layer-m, .search-m, .about-m {
  cursor: pointer;
}

@media (min-width: 421px) {
    #toolbar-m {
      display: none;
    } 
}

@media (max-width: 420px) {
  html, body {
    margin: 0;
    padding: 0;
    height: 100%;
    overflow: hidden;
  }

  template {
    display: flex;
    flex-direction: column;
    height: 100%;
    overflow: hidden;
  }
  .search, .layers {
      display: none;
    }
}
</style>
