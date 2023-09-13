<script setup lang="ts">
import type { FeatureLike } from 'ol/Feature';
import MapContainer from './components/MapContainer.vue';
import AttributeTable from './components/AttributeTable.vue';
import { ref } from 'vue';
import type Feature from 'ol/Feature';

const hoveredFeatures = ref<FeatureLike[]>([]);

function onFeaturesHover(features: FeatureLike[]) {
  hoveredFeatures.value = features;
}
</script>

<template>
  <div class="map-container">
    <MapContainer fill="red" stroke="black" @features-click="onFeaturesHover" />

    <div class="attributes" v-if="hoveredFeatures.length > 0">
      <AttributeTable :feature="(hoveredFeatures[0] as Feature)" />
    </div>
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
  overflow-y: scroll;
}
.map-container {
  background-color: darkgreen;
  width: 100%;
  height: 100%;
}
</style>
