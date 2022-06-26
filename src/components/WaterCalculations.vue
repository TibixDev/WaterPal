<template>
    <div class="text-2xl container mx-auto flex flex-col gap-4 p-2">
        <fieldset>
            <legend class="text-3xl font-semibold pb-2">Select your unit:</legend>

            <div class="flex flex-row gap-3 justify-center">
                <div class="inline-flex flex-row gap-1">
                    <input type="radio" id="liter" value="liter" v-model="currentUnit" checked>
                    <label for="liter">Liter</label>
                </div>
    
                <div class="inline-flex flex-row gap-1">
                    <input type="radio" id="gallon" value="gallon" v-model="currentUnit">
                    <label for="gallon">Gallon</label>
                </div>
    
                <div class="inline-flex flex-row gap-1">
                    <input type="radio" id="quart" value="quart" v-model="currentUnit">
                    <label for="quart">Quart</label>
                </div>
            </div>
        </fieldset>
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
            <span class="highlight">{{ (dailyWaterPerPerson / 0.25).toFixed(0) }} cups</span>
            daily. (<span class="highlight">250ml</span>)
        </p>
        <p>
            You will need
            <span class="highlight">{{ bottles }}</span>
            bottles that are
            <span class="highlight">{{ convert(2.5) }}</span> each.
        </p>
        <div class="flex flex-row flex-wrap mx-auto">
            <svg v-for="n, index in bottles" :key="index" class="w-16 h-16" xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink" width="1em" height="1em" viewBox="0 0 24 24">
                <path fill="#888888"
                    d="M15 11v9a2 2 0 0 1-2 2h-2a2 2 0 0 1-2-2v-9a2 2 0 0 1 .6-1.42C11.1 7.89 11 4 11 4h-1V2h4v2h-1s-.1 3.89 1.4 5.58A2 2 0 0 1 15 11Z">
                </path>
            </svg>
        </div>
    </div>
</template>

<script setup>
// Imports
import { ref } from "vue";

const waterMenL = 3.7;
const waterWomenL = 2.7;
const waterAvg = (waterMenL + waterWomenL) / 2;
const currentUnit = ref("liter");

const props = defineProps({
    tripData: {
        type: Object,
        default: () => ({
            people: 5,
            days: 3
        }),
    },
})

const converters = {
    liter: (liter) => liter,
    gallon: (liter) => (liter * 0.26417).toFixed(2),
    quart: (liter) => (liter * 0.946353).toFixed(2),
}

function convert(liter) {
    const value = converters[currentUnit.value](liter);
    return value + " " + currentUnit.value + (value > 1 ? "s" : "");
}

function getWater(people, days) {
    return people * days * waterAvg;
}

const waterQty = getWater(props.tripData.people, props.tripData.days);
const waterPerPerson = waterQty / props.tripData.people;
const dailyWaterPerPerson = (waterPerPerson / props.tripData.days).toFixed(2);
const bottles = Math.round(waterQty / 2.5)
console.log(bottles)
</script>

<style scoped>
.highlight {
    @apply text-cyan-500 font-bold;
}
</style>