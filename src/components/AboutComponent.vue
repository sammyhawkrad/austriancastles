<script setup>

</script>

<template>
  <div id="about-button">
  <img class="about" alt="about" src="@/assets/about.svg"/>
  </div>
  <div id="about-box">
    <div id="headline">
        <div id="left-items">
          <img class="logo" alt="logo" src="@/assets/about.svg" width="40" height="40"/>
          <h2>ABOUT</h2>
        </div>
        <div id="close-button">
          <svg xmlns="http://www.w3.org/2000/svg" width="2em" height="2em" viewBox="0 0 24 24">
            <path fill="#472d30" d="M17.59 5L12 10.59L6.41 5L5 6.41L10.59 12L5 17.59L6.41 19L12 13.41L17.59 19L19 17.59L13.41 12L19 6.41L17.59 5Z"></path>
          </svg>
        </div>
    </div>
    <p id="about-text">Text that explains the Web Map, functionalities and so on...</p>
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
  display: none;
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
  display: flex;
  margin-left: auto;
  padding: 10px;
  cursor: pointer;
  transition: all 0.2s ease-in;
}

#close-button:hover {
  opacity: 0.7;
  background-color: lightgrey;
}

#headline .logo {
  margin-right: 10px;
}

h2 {
  font-size: 25px;
  color: #723d46;
  font-weight: bold;
  text-align: center;
}

#about-text {
  padding-left: 2%;
  padding-right: 2%;
}
</style>
