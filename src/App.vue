<script setup>
import Wheel from "./components/Wheel.vue";
import Panel from "./components/Panel.vue";
import { ref, watchEffect } from "vue";

const entries = ref(6);
const data = ref({});
const divWidth = ref("200px");
const refs = [];

//generates the width of each partition
const getDivWidth = () => {
    let mult = 9;
    let base = 200;
    switch (entries.value) {
        case 2:
            mult = 15;
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

    return (base * mult) / entries.value + "px";
};

// Generate a unique ref for each entry
const getRef = (entry) => {
    if (!refs[entry]) {
        console.log(entry);
        refs[entry] = ref(null);
    }

    return refs[entry];
};

//handle change event for input
const handleInputChange = (entry) => {
    data.value[entry] = refs[entry].value[0].value;
    console.log(data.value[entry])
}

watchEffect(() => {
    if (entries.value > 10) entries.value = 10;
    if (entries.value < 2) entries.value = 2;
    divWidth.value = getDivWidth();
});
</script>

<template>
    <header></header>

    <main class="bg-slate-200 min-h-screen items-center p-4 flex flex-col">
        <p class="text-2xl">Vue wheel of fortune</p>
        <div class="flex flex-col md:flex-row gap-6 m-10">
            <div class="">
                <Wheel :entries="entries" :divWidth="divWidth" :data="data"/>
            </div>
            <div class="panel flex flex-col items-center">
                <div>
                    <label>Max partitions: </label>
                    <input v-model="entries" type="number" size="5" />
                </div>
                <div v-for="(entry, index) in entries" :key="entry" >
                    <input
                        type="text"
                        class="m-4"
                        :ref="getRef(entry)"
                        @change="handleInputChange(entry)"
                    />
                </div>
            </div>
        </div>
    </main>
</template>

<style scoped></style>
