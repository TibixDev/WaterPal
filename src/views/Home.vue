<template>
    <div class="flex flex-col items-center justify-center min-h-screen mx-auto gap-10">
        <div class="flex flex-col">
            <div class="flex flex-col sm:flex-row justify-center items-center mt-10 sm:mt-0">
                <img class="w-20" src="../assets/waterpal.svg" alt="waterpal logo">
                <h1 class="align-center font-extrabold font-sans antialiased text-5xl sm:text-7xl">WATERPAL</h1>
            </div>
            <h2 class="text-right italic">Never out of water</h2>
        </div>
        <div class="border-solid rounded-md drop-shadow-md px-12 pt-10 pb-2 bg-white m-2 max-w-screen md:max-w-3xl justify-center">
            <div class="flex flex-col items-center" v-if="!isDone">
                <h1 class="text-center text-3xl font-bold pb-7">Enter your trip details</h1>
                <div class="flex flex-col sm:flex-row gap-1 justify-center items-center">
                    <p class="pb-2">I'm going on a trip with </p>
                    <p class="pb-2">
                        <input
                            class="w-10 text-center border-2 rounded-md border-sky-500 focus:outline-none focus:border-sky-600"
                            type="number"
                            min="1"
                            placeholder="1"
                            v-model="tripData.people"
                        >
                        people
                    </p>
                </div>
                <p class="pb-10">for a total of
                    <input
                        class="w-10 text-center border-2 rounded-md border-sky-500 focus:outline-none focus:border-sky-600" 
                        type="number"
                        min="1"
                        placeholder="1"
                        v-model="tripData.days"
                    >
                    day(s)
                </p>
                <button
                    class="transition duration-300 text-white font-bold py-2 px-4 rounded"
                    @click="isDone = !isDone"
                    :disabled="!canCalculate"
                    :class="{ 'cursor-not-allowed bg-gray-500': !canCalculate, 
                            'bg-blue-500 hover:bg-blue-700': canCalculate }"
                >Calculate</button>
                
            </div>
            <WaterCalculations :trip-data="tripData" @back="isDone = !isDone" v-if="isDone"></WaterCalculations>
            <Tip/>
        </div>
    </div>

</template>

<script setup>
import { ref, computed } from "vue";
import WaterCalculations from "../components/WaterCalculations.vue";
import Tip from "../components/TipOfTheDay.vue";

let isDone = ref(false);
let tripData = ref({
    people: null,
    days: null
});
let canCalculate = computed(() => !(tripData.value.people === null || tripData.value.days === null));

</script>

<style scoped>
</style>