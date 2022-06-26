<script setup>
    console.log(new Date().getDate());
</script>

<template>
    <div class="pt-7 flex flex-col">
        <p class="font-bold pb-1 text-center">💡 Tip of the Day</p>
        <p class="text-center max-w-fit">{{((arr) => {return arr.at(new Date().getDate() % arr.length)})([
            "Don't ration your water! If you're feeling dehydrated, or if you're starting to feel a headache, just drink some more!", 
            "If your water supplies are limited, stay away from alcohol! Alcohol dehydrates the body and you don't want that.", 
            "Before starting your trip, make sure to drink enough water so you stay hydrated for a longer period of time.", 
            "Stay clear of caffeine on your trips if possible, because they increase fluid loss.",
            "Drinking water with increased electrolytes will help retain more water in your body.",
            "Continue drinking after you finished your trip to rehydrate your body as much as possible.",
            "If your fluids contain more sodium or potassium then rehydration will be enhanced",
            "Putting a little table salt into your drinking water will help with the electrolytes!"
        ])}}</p>
    </div>
</template>

