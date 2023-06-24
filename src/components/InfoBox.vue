<script setup>

defineProps({
  clickedCastle: Object,
  infoboxVisible: Boolean
});

</script>

<template>
  <div v-if="infoboxVisible" id="infobox">
    <div id="headline">
      <div id="left-items">
        <img class="logo" alt="logo" src="@/assets/fortress.svg" width="40" height="40"/>
        <h2>{{ clickedCastle.name }}</h2>
      </div>
      <div id="close-button" @click="$emit('close')">
        <svg xmlns="http://www.w3.org/2000/svg" width="2em" height="2em" viewBox="0 0 24 24">
          <path fill="#472d30" d="M17.59 5L12 10.59L6.41 5L5 6.41L10.59 12L5 17.59L6.41 19L12 13.41L17.59 19L19 17.59L13.41 12L19 6.41L17.59 5Z"></path>
        </svg>
      </div>
    </div>
    <div id="time-place-line">
      <img class="location" alt="location" src="@/assets/castlelocation.svg" width="20" height="20"/>
      <p>{{ `${clickedCastle.location}, ${clickedCastle.state}` }}</p>
      <img class="time" alt="time" src="@/assets/time.svg" width="20" height="20"/>
      <p>{{ clickedCastle.start_date? clickedCastle.start_date: clickedCastle['time of origin'] }}</p>
    </div>
    <img class="img-castle" alt="castle" src="@/assets/disney-castle.jpg" />
    <!-- <img class="img-castle" alt="castle" :src="clickedCastle.image_url" /> -->
    <div id="description"> 
      <p>{{ clickedCastle['description-translated'] }}</p>
    </div>
  </div>
</template>

<style scoped>
#infobox {
  background-color: #ffe1a8;
  position: absolute;
  width: 25%;
  height: 75%;
  left: 72vw;
  top: 9vw;
  z-index: 3;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  border-radius: 2%;
}

#headline {
  display: flex;
  justify-content: space-between;
}

#left-items {
  display: flex;
  padding: 10px;
  align-items: center;
  width: 80%;
}

#close-button {
  margin-left: auto;
  padding: 10px;
  cursor: pointer;
  transition: all 0.2s ease-in;
  color: #472d30;
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
  font-size: 25px;
  color: #723d46;
  font-weight: bold;
  text-align: center;
}

#time-place-line {
  display: flex;
  align-items: center;
  justify-content: space-around;
  padding-top: 10px;
  padding-bottom: 10px;
  margin: auto;
  width: 90%;
}

.img-castle {
  width: 90%;
  display: block;
  margin: 0 auto;
  max-width: 100%;
  height: auto;
}

#description {
  width: 90%;
  margin: auto;
  overflow: auto;
}

p {
  color: #472d30;
}

@media (max-width: 900px) {
  #infobox {
    width: 40%;
    left: 57vw;
    top: 15vw;
  }
}

@media (max-width: 420px) {
    #infobox {
      width: 90%;
      height: 80%;
      left: 5vw;
      top: 10vw;
    } 
}


</style>
