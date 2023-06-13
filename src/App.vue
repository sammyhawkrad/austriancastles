<script setup>
import AboutComponent from './components/AboutComponent.vue';
import MapComponent from './components/MapComponent.vue';
import InfoBox from './components/InfoBox.vue';
</script>

<template>
  <header>
    <h1>Austrian Castles</h1>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />
  </header>
  <MapComponent />
  <InfoBox />
  <AboutComponent />
</template>

<style scoped>

</style>
