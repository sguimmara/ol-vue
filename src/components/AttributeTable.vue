<script setup lang="ts">
import type Feature from 'ol/Feature';

defineProps<{
    feature: Feature;
}>();

const exclude = ['geometry'];

function getFilteredKeys(feature: Feature) {
    const names = [];

    for (const key of feature.getKeys()) {
        if (!exclude.includes(key)) {
            names.push(key);
        }
    }

    return names;
}
</script>

<template>
    <div>
        <table>
            <thead>
                <td>Name</td>
                <td>Value</td>
            </thead>
            <tbody>
                <tr v-for="item in getFilteredKeys(feature)" :key="item">
                    <td>{{item}}</td>
                    <td>{{feature.get(item)}}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>