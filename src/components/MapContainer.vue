<script setup lang="ts">
import { onMounted } from 'vue';
import Map from 'ol/Map.js';
import OSM from 'ol/source/OSM.js';
import TileLayer from 'ol/layer/Tile.js';
import View from 'ol/View.js';
import VectorLayer from 'ol/layer/Vector';
import VectorSource from 'ol/source/Vector';
import GeoJSON from 'ol/format/GeoJSON';
import { Feature, MapBrowserEvent } from 'ol';
import Style from 'ol/style/Style';
import Fill from 'ol/style/Fill';

const props = defineProps<{
    fill: string;
    stroke: string;
}>();

const emits = defineEmits<{
    (e: 'features-hover', features: Feature[]): void
    (e: 'features-click', features: Feature[]): void
}>();

function onPointerMove(event: MapBrowserEvent<PointerEvent>) {
    const pixel = event.pixel;

    if (pixel) {
        const map = event.map;

        const features = map.getFeaturesAtPixel(pixel) as Feature[];

        emits('features-hover', features);
    }
}

const hover = new VectorLayer({
    source: new VectorSource(),
    style: new Style({
        fill: new Fill({
            color: 'red'
        })
    })
})

function onPointerClick(event: MapBrowserEvent<PointerEvent>) {
    const pixel = event.pixel;

    if (pixel) {
        const map = event.map;

        const features = map.getFeaturesAtPixel(pixel) as Feature[];

        hover.getSource()?.clear();
        hover.getSource()?.addFeatures(features);

        emits('features-click', features);
    }
}

function createMap() {
    const osm = new TileLayer({
        source: new OSM(),
    });

    const countries = new VectorLayer({
        source: new VectorSource({
            url: '/countries.geo.json',
            format: new GeoJSON(),
        }),
    });

    const map = new Map({
        layers: [osm, countries, hover],
        target: 'map',
        view: new View({
            center: [0, 0],
            zoom: 2,
        }),
    });

    map.on('pointermove', onPointerMove);
    map.on('click', onPointerClick);
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