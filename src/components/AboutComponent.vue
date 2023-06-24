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
          <img class="logo" alt="logo" src="@/assets/about.svg" width="40" height="40"/>
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
      access to information about a castle. They can search for a specific castle/town??? and filter by castle type.</p>
      <h3>Tools</h3>
      <p id="about-paragraph">Leaflet, Vue???, Adobe Illustrator</p>
      <h3>Sources and Licences</h3>
      <p id="about-paragraph"><a href="https://www.openstreetmap.org/">Openstreetmap</a>, <a href="https://www.wikipedia.org/">Wikipedia</a>, <a href="https://www.wikidata.org/">Wikidata</a></p>
      <br>
      <p id="created-by">Created by: Yi Zhen Chew, Ulrike Holfeld, Samuel Darkwah (TU Wien)</p>
    </div>
  </div>
  div
  <img class="about-m" alt="about" src="@/assets/about-mobile.svg" width="60" height="60"/>
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
}

#about-box {
  background-color: #ffe1a8;
  position: absolute;
  width: 25%;
  height: 75%;
  left: 72vw;
  top: 9vw;
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

#headline .logo {
  margin-right: 10px;
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

#created-by {
  font-style: italic;
  padding: 10px;
}

@media (max-width: 420px) {
    #about-button {
      display: none;
    } 
    #about-box {
      display: none;
    }
}
</style>
