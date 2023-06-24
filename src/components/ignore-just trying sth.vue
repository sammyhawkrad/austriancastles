<script setup>

</script>



<template>
    <div id="toolbar-m">
        <img class="layer-m" alt="about" src="@/assets/layers.svg" width="60" height="60"/>
        <img class="region-m" alt="about" src="@/assets/filterbyregion.svg" width="60" height="60"/>
        <img class="search-m" alt="about" src="@/assets/search.svg" width="60" height="60"/>
        <img class="about-m" alt="about" src="@/assets/about.svg" width="60" height="60"/>
    </div>
    <div id="buttons">
        <img class="search" alt="search" src="@/assets/search.svg" width="40" height="40"/>
        <img class="filterbyregion" alt="region" src="@/assets/filterbyregion.svg" width="40" height="40"/>
    </div>
</template>



<style scoped>
.search {
  top: 15%;
  left: 1%;
  position: absolute;
  z-index: 3;
  cursor: pointer;
  border: solid 2px #472d30;
}

.filterbyregion {
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
  width: 100vw;
  background-color: #c8caa3;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: 60px;
}

.layer-m, .region-m, .search-m, .about-m {
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
    height: 100vh;
    overflow: hidden;
  }

  template {
    display: flex;
    flex-direction: column;
    height: 100vh;
    overflow: hidden;
  }
  .search {
      display: none;
    }
  .filterbyregion {
      display: none;
  }
}
</style>