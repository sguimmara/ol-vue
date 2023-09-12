<script setup lang="ts">
import MapContainer from './components/MapContainer.vue';
</script>

<template>
  <div class="map-container">
    <MapContainer fill="blue" stroke="black" />
  </div>
</template>

<style scoped>
.attributes {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 500px;
  height: 500px;
  background-color: white;
  color: black;
  margin: 1rem;
}
.map-container {
  background-color: darkgreen;
  width: 100%;
  height: 100%;
}
</style>
