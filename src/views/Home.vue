<template>
    <WaterCalculations v-if="isDone"></WaterCalculations>
</template>

<script setup>
import { ref } from "vue";
import WaterCalculations from "../components/WaterCalculations.vue";

let isDone = ref(true);
</script>

<style scoped>

</style>