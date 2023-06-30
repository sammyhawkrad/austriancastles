<script setup></script>

<template>
  <div id="toolbar-m">
    <img class="layer-m" alt="about" src="@/assets/layers-mobile.svg" width="60" height="60" />
    <img class="region-m" alt="about" src="@/assets/region-mobile.svg" width="60" height="60" />
    <img class="search-m" alt="about" src="@/assets/search-mobile.svg" width="60" height="60" />
    <img class="about-m" alt="about" src="@/assets/about-mobile.svg" width="60" height="60" />
  </div>
  <div id="buttons">
    <img class="search" alt="search" src="@/assets/search.svg" width="40" height="40" />
    <div @click.once="filterCastles">
      <img
        class="filterbyregion"
        alt="region"
        src="@/assets/filterbyregion.svg"
        width="40"
        height="40"
      />
    </div>
  </div>
</template>

<style scoped>
.search {
  top: 15%;
  left: 1%;
  position: absolute;
  z-index: 3;
  cursor: pointer;
}

.filterbyregion {
  top: 25%;
  left: 1%;
  position: absolute;
  z-index: 3;
  cursor: pointer;
}

#toolbar-m {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  width: 100vw;
  background-color: #c8caa3;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 15vw;
}

.layer-m,
.region-m,
.search-m,
.about-m {
  cursor: pointer;
}

@media (min-width: 421px) {
  #toolbar-m {
    display: none;
  }
}

@media (max-width: 420px) {
  html,
  body {
    margin: 0;
    padding: 0;
    height: 100%;
    overflow: hidden;
  }
}
</style>
