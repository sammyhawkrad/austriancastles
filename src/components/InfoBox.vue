<script setup>
defineProps({
  clickedCastle: Object,
  infoboxVisible: Boolean
})
</script>

<template>
  <div v-if="infoboxVisible" id="infobox">
    <div id="headline">
      <div id="left-items">
        <img class="logo" alt="logo" src="@/assets/fortress.svg" width="40" height="40" />
        <h2>{{ clickedCastle.name }}</h2>
      </div>
      <div id="close-button" @click="$emit('close')">
        <svg xmlns="http://www.w3.org/2000/svg" width="2em" height="2em" viewBox="0 0 24 24">
          <path
            fill="#472d30"
            d="M17.59 5L12 10.59L6.41 5L5 6.41L10.59 12L5 17.59L6.41 19L12 13.41L17.59 19L19 17.59L13.41 12L19 6.41L17.59 5Z"
          ></path>
        </svg>
      </div>
    </div>
    <div id="time-place-line">
      <img
        class="location"
        alt="location"
        src="@/assets/castlelocation.svg"
        width="20"
        height="20"
      />
      <p>{{ `${clickedCastle.location}, ${clickedCastle.state}` }}</p>
      <img class="time" alt="time" src="@/assets/time.svg" width="20" height="20" />
      <p>
        {{ clickedCastle.start_date ? clickedCastle.start_date : clickedCastle['time of origin'] }}
      </p>
    </div>
    <div class="img-castle-box">
      <img
        v-if="!clickedCastle.img_file"
        class="img-castle"
        alt="castle"
        src="@/assets/disney-castle.jpg"
      />
      <img v-else class="img-castle" alt="castle" :src="`https:${clickedCastle.img_file}`" />
    </div>

    <div id="description">
      <p>{{ clickedCastle['description-translated'] }}</p>
    </div>
  </div>
</template>

<style scoped>
#infobox {
  background-color: #ffe1a8;
  position: fixed;
  width: 30%;
  height: 75%;
  top: 8rem;
  right: 2rem;
  z-index: 3;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
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

#close-button > svg:hover {
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
}

#time-place-line {
  display: flex;
  align-items: center;
  justify-content: space-around;
  padding-top: 10px;
  padding-bottom: 10px;
  margin-left: auto;
  margin-right: auto;
  width: 90%;
}

.img-castle-box {
  display: block;
  position: relative;
}

.img-castle-box:before {
  content: '';
  display: block;
  width: 90%;
  padding-top: 56.25%;
}

.img-castle {
  position: absolute;
  width: auto;
  height: auto;
  max-width: 90%;
  max-height: 100%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

#description {
  width: 90%;
  overflow: auto;
  margin-right: auto;
  margin-left: auto;
  margin-top: 10px;
}

p {
  color: #472d30;
  margin-left: 7px;
}

@media (max-width: 900px) {
  #infobox {
    width: 40%;
  }
}

@media (max-width: 650px) {
  #infobox {
    width: 60%;
  }
}

@media (max-width: 420px) {
  #infobox {
    width: 90%;
    height: 76%;
    top: 5.5rem;
    left: 5%;
    right: 5%;
  }
}
</style>
