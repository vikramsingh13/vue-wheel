<script setup>
import Wheel from "./components/Wheel.vue";
import Panel from "./components/Panel.vue";
import { ref, watchEffect } from "vue";

const entries = ref(6);
const divWidth = ref('200px');


//generates the width of each partition
const getDivWidth = () => {
    let mult = 9;
    let base = 200;
    switch(entries.value){
        case 2:
            mult = 15
            base = 1000;
        case 3:
            mult = 13;
            base = 200;
            break;
        case 4:
            mult = 11;
            base = 190;
            break;
        case 5: 
            mult = 10;
            base = 175;
            break;
        case 6: 
            mult = 11;
            base = 150;
            break;
        case 8:
            mult = 9;
            base = 180;
            break;
        case 10:
            mult = 8;
            break;
        default:
            mult = 8;
            base = 200;
    }

    return (base * mult / entries.value + 'px');
}

watchEffect(() => {
    if(entries.value > 10) entries.value = 10;
    if(entries.value < 2) entries.value = 2;
    divWidth.value = getDivWidth();
});


</script>

<template>
    <header></header>

    <main class="bg-slate-200 min-h-screen">
        <div class="justify-center items-center p-4 flex flex-col">
            <p class="text-2xl">Vue wheel of fortune</p>
            <input v-model="entries" type="number" />
            <div class="flex flex-col md:flex-row gap-4">
                <Wheel :entries="entries"  :divWidth="divWidth" />
                <Panel :entries="entries"/>
            </div>
        </div>
    </main>
</template>

<style scoped></style>
