<script setup lang="ts">
import { onMounted } from 'vue';
import Map from 'ol/Map.js';
import OSM from 'ol/source/OSM.js';
import TileLayer from 'ol/layer/Tile.js';
import View from 'ol/View.js';
import VectorLayer from 'ol/layer/Vector';
import VectorSource from 'ol/source/Vector';
import GeoJSON from 'ol/format/GeoJSON';
import Style from 'ol/style/Style';
import Fill from 'ol/style/Fill';
import Stroke from 'ol/style/Stroke';

const props = defineProps<{
    fill: string;
    stroke: string;
}>();

let map: Map;

function createMap() {
    const osm = new TileLayer({
        source: new OSM(),
    });

    const countries = new VectorLayer({
        source: new VectorSource({
            url: '/countries.geo.json',
            format: new GeoJSON(),
        }),
        style: new Style({
            fill: new Fill({
                color: props.fill,
            }),
            stroke: new Stroke({
                color: props.stroke,
                width: 1,
            })
        })
    });

    map = new Map({
        layers: [osm, countries],
        target: 'map',
        view: new View({
            center: [0, 0],
            zoom: 2,
        }),
    });
}

onMounted(createMap);
</script>

<template>
    <div id="map" class="map">
    </div>
</template>

<style scoped>
.map {
    background-color: black;
    color: black;
    height: 100%;
    width: 100%;
}
</style>