<script setup>
import ButtonsComponent from './components/ButtonsComponent.vue';
import MapComponent from './components/MapComponent.vue';
</script>

<template>
  <header>
    <h1 id="title">Austrian Castles</h1>
    <img alt="title-img" class="logo" src="@/assets/title-image.svg" width="50" height="50" />
  </header>
  <MapComponent />
  <ButtonsComponent />
  
</template>

<style scoped>
</style>
