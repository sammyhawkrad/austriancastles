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
    <p id="about-text">about our project<br>functionalities<br>tools<br>sources with link, licenses</p>
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

#about-text {
    width: 80%;
    overflow: auto;
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
