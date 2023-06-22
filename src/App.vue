<script setup>
import AboutComponent from './components/AboutComponent.vue';
import MapComponent from './components/MapComponent.vue';
import InfoBox from './components/InfoBox.vue';
import Toolbar from './components/Toolbar.vue';
</script>

<template>
  <header>
    <h1 id="title">Austrian Castles</h1>
    <img alt="fortress" class="logo" src="@/assets/fortress.svg" width="50" height="50" />
  </header>
  <MapComponent />
  <InfoBox />
  <AboutComponent />
  <Toolbar />
</template>

<style scoped>
#title {
  margin-right: 2rem;
}
</style>
