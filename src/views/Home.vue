<template>
    <div class="flex flex-col items-center justify-center h-screen mx-auto gap-10">
        <div class="flex flex-col sm:flex-row justify-center items-center">
            <img class="w-20" src="../assets/waterpal.svg" alt="waterpal logo">
            <h1 class="align-center font-extrabold font-sans antialiased text-5xl sm:text-7xl">WATERPAL</h1>
        </div>
        <div class="flex flex-col items-center border-solid rounded-md drop-shadow-md p-10 bg-white m-2">
            <h1 class="text-center text-2xl font-bold pb-10">Enter your trip details</h1>
            <div class="flex flex-col sm:flex-row gap-1 justify-center items-center">
                <p class="pb-2">I'm going on a trip with </p>
                <p class="pb-2">
                    <input
                        class="w-10 text-center border-2 rounded-md border-sky-500 focus:outline-none focus:border-sky-600"
                        type="text"
                        placeholder="1"
                    >
                    family member(s)
                </p>
            </div>
            <p class="pb-10">for
                <input
                    class="w-10 text-center border-2 rounded-md border-sky-500 focus:outline-none focus:border-sky-600" 
                    type="text"
                    placeholder="1"
                >
                days
            </p>
            <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Calculate</button>
        </div>

        <!-- <WaterCalculations v-if="isDone"></WaterCalculations> -->
    </div>

</template>

<script setup>
import { ref } from "vue";
import WaterCalculations from "../components/WaterCalculations.vue";

let isDone = ref(true);
</script>

<style scoped>
</style>