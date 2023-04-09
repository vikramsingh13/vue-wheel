<script setup>
import { ref, watchEffect, watch } from 'vue';
const { entries, divWidth } = defineProps({
  entries: {
    type: Number,
    required: true
  },
  divWidth: {
    type: String,
  }
});

const angle = ref(Math.ceil(360 + Math.random() * 800));

const colorPalette = ref(['#f29e4c', '#f1c453', '#efea5a', '#b9e769', '#83e377', '#16db93', '#0db39e', '#048ba8', '#2c699a', '#54478c']);

//get random light background color
//not needed, better to use static palette
const getRandomBG = () => {
    return `hsl(${1 + Math.random() * 359}, ${60 + Math.random() * 19}%, ${40 + Math.random() * 39}%`;
};

//handles click spin button
const handleSpin = () => {
	angle.value += Math.ceil(360 + Math.random() * 800);
}

</script>

<template>
    <div class="flex justify-center items-center">
        <button id="spin" class="spin" @click="handleSpin">Spin</button>
        <div
            class="container rounded-full border-white border-4 relative overflow-hidden flex justify-center"
            :style="{ width: 480 + 'px', height: 480 + 'px', transform: 'rotate(' + angle + 'deg)', transitionDuration: '2s', transitionProperty: 'transform'}"
        >
            <div
                v-for="entry in entries"
                :key="entry"
                class="h-1/2 absolute flex justify-center items-center text-center text-lg text-white font-bold font-sans  cursor-default"
                :style="{
                    zIndex: entries - entry,
                    transform:
                        'rotate(' + (360 / entries) * (entry - 1) + 'deg)',
                    backgroundColor: colorPalette[entry - 1],
                    width: divWidth,
                    'clip-path': entries !== 2 ? 'polygon(0 0, 100% 0, 50% 100%)' : '',
                    'transform-origin': 'bottom',
                }"
            >
                {{ entry }}
            </div>
        </div>
    </div>
</template>
