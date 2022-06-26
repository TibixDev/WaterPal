<template>
    <div class="text-xl flex flex-col gap-4 p-2">
        <div>
            <button class="transition bg-blue-500 hover:bg-blue-700 duration-300 text-white font-bold py-1 px-2 rounded"
                @click="$emit('back')">Back</button>
        </div>

        <!-- UNIT SELECTOR -->
        <fieldset>
            <legend class="text-3xl font-semibold pb-2">Change your unit:</legend>

            <div class="flex flex-row gap-3">
                <div class="inline-flex flex-row gap-1">
                    <input type="radio" id="liter" value="liter" v-model="currentUnit" @input="recalcChart()" checked>
                    <label for="liter">Liter</label>
                </div>

                <div class="inline-flex flex-row gap-1">
                    <input type="radio" id="gallon" value="gallon" v-model="currentUnit" @input="recalcChart()">
                    <label for="gallon">Gallon</label>
                </div>

                <div class="inline-flex flex-row gap-1">
                    <input type="radio" id="quart" value="quart" v-model="currentUnit" @input="recalcChart()">
                    <label for="quart">Quart</label>
                </div>
            </div>
        </fieldset>
        <hr>

        <!-- WATER DATA -->
        <h1>Water Data</h1>
        <p>
            In order to keep
            <span class="highlight">{{ props.tripData.people }} people</span>
            well-hydrated for a
            <span class="highlight">{{ props.tripData.days }} day trip,</span>
            you are going to need at least
            <span class="highlight">{{ convert(waterQty) }}</span>
            of water.
        </p>
        <p>
            You can use
            <span class="highlight">{{ convert(waterPerPerson) }}</span>
            of water per person.
            Each person has to roughly drink
            <span class="highlight">{{ convert(dailyWaterPerPerson) }}</span>
            every day. That's about
            <span class="highlight">{{ (dailyWaterPerPerson / bottleSizeL).toFixed(0) }} cups</span>
            daily. (<span class="highlight">{{ convert(cupSizeL) }} cup</span>)
        </p>
        <p>
            You will need
            <span class="highlight">{{ bottles }}</span>
            bottles that are
            <span class="highlight">{{ convert(2.5) }}</span> each.
        </p>
        <div class="flex flex-row flex-wrap mx-auto">
            <svg v-for="n, index in clamp(bottles, 0, 500)" :key="index" class="w-8 h-8" xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink" width="1em" height="1em" viewBox="0 0 24 24">
                <path fill="#109BFF"
                    d="M15 11v9a2 2 0 0 1-2 2h-2a2 2 0 0 1-2-2v-9a2 2 0 0 1 .6-1.42C11.1 7.89 11 4 11 4h-1V2h4v2h-1s-.1 3.89 1.4 5.58A2 2 0 0 1 15 11Z">
                </path>
            </svg>
            <p v-if="bottles > 500">and {{ bottles-500 }} more...</p>
        </div>

        <!-- CHART DATA -->
        <h1>Statistics</h1>
        <apexchart width="500" type="bar" class="mx-auto" :options="chartOptions" :series="series"></apexchart>
    </div>
</template>

<script setup>
// Imports
import { ref } from "vue";

// Constants
const waterMenL = 3.7;
const waterWomenL = 2.7;
const waterAvg = (waterMenL + waterWomenL) / 2;
const currentUnit = ref("liter");
const bottleSizeL = 0.25;
const cupSizeL = 0.25;

// Props and emits
const props = defineProps({
    tripData: {
        type: Object,
        default: () => ({
            people: 5,
            days: 3
        }),
    },
})

const emits = defineEmits(["back"])

// Converters convert liter to other units
const converters = {
    liter: (liter) => liter,
    gallon: (liter) => (liter * 0.26417),
    quart: (liter) => (liter * 0.946353),
}

// Helper functions
function convert(liter) {
    const value = Number(converters[currentUnit.value](liter));
    return value.toFixed(2) + " " + currentUnit.value + (value > 1 ? "s" : "");
}

function convertRaw(liter) {
    const value = Number(converters[currentUnit.value](liter));
    return Number(value.toFixed(2));
}

function getWater(people, days) {
    return people * days * waterAvg;
}

function clamp(value, min, max) {
    return Math.min(Math.max(value, min), max);
}

// Data
const waterQty = getWater(props.tripData.people, props.tripData.days);
const waterPerPerson = waterQty / props.tripData.people;
const dailyWaterPerPerson = (waterPerPerson / props.tripData.days).toFixed(2);
const bottles = Math.round(waterQty / bottleSizeL);

const chartX = [...Array(props.tripData.days).keys()].map(i => `Day ${i + 1}`);
let chartY = ref([]);

for (let i = 0; i < props.tripData.days; i++) {
    chartY.value.push(convertRaw(getWater(props.tripData.people, props.tripData.days - i)));
}

function recalcChart() {
    // This is needed because otherwise the reactive variable
    // for currentUnit doesn't change soon enough.
    setTimeout(() => {
        for (let i = 0; i < props.tripData.days; i++) {
            chartY.value[i] = convertRaw(getWater(props.tripData.people, props.tripData.days - i));
        }
    }, 100)
}

// Chart
const chartOptions = ({
    chart: {
        id: "water-chart",
    },
    xaxis: {
        categories: chartX,
    },
});

const series = ref([
    {
        name: "Water",
        data: chartY.value
    },
]);
</script>

<style scoped>
.highlight {
    @apply text-cyan-500 font-bold;
}

h1 {
    @apply text-3xl font-bold;
}
</style>