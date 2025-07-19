<template>
  <section
    class="fixed top-0 left-0 w-full h-full z-50 bg-black/20 flex justify-center items-center"
  >
    <div class="bg-stone-50 rounded-xl p-4 w-3/4">
      <div class="flex justify-between w-full items-center">
        <h3 class="text-2xl font-bold text-neutral-950">Pokemon Detail</h3>
        <button
          @click="emit('close')"
          class="text-neutral-500 hover:text-neutral-700 transition-colors"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="size-6"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M6 18 18 6M6 6l12 12"
            />
          </svg>
        </button>
      </div>
      <div class="flex flex-wrap justify-between gap-4 mt-12">
        <img
          :src="pokemon.sprites.other.showdown.front_default"
          :alt="'Pokeman ' + pokemon.name + ' Image'"
          class="size-48 rounded-lg"
        />
        <div class="">
          <h6 class="text-lg font-bold">#{{ pokemon.formattedId }}</h6>
          <h2 class="font-bold text-3xl">{{ pokemon.name }}</h2>
          <div class="flex gap-1 pt-2">
            <span
              v-for="typeInfo in pokemon.types"
              :key="typeInfo.slot"
              :style="{ backgroundColor: typeColors[typeInfo.type.name] }"
              class="px-2 py-0.5 text-sm font-semibold rounded-lg size-fit bg-white"
            >
              {{ typeInfo.type.name }}
            </span>
          </div>
          <p class="">{{ pokemon.description }}</p>
          <div class="flex gap-2">
            <span class="bg-stone-200 py-1 px-4 rounded-2xl text-base">
              <strong>Height :</strong> {{ pokemon.height / 10 }} m
            </span>
            <span class="bg-stone-200 py-1 px-4 rounded-2xl text-base">
              <strong>Weight :</strong> {{ pokemon.weight / 10 }} m
            </span>
          </div>
          <div class="pt-5">
            <h1 class="font-bold text-lg">Stat</h1>
            <div class="grid grid-cols-2 gap-4 mt-2">
              <span
                v-for="stat in pokemon.stats"
                :key="stat.stat.name"
                :style="{ backgroundColor: statColors[stat.stat.name] }"
                class="bg-stone-200 py-1 px-4 rounded text-base"
              >
                <strong>{{ stat.stat.name }} :</strong> {{ stat.base_stat }}
              </span>
            </div>
          </div>
          <div class="flex gap-4 pt-5">
            <span
              v-for="ability in pokemon.abilities"
              class="bg-stone-200 py-1 px-5 rounded-2xl font-bold"
              >{{ ability.ability.name }}</span
            >
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
<script setup>
import { ref } from "vue";

const props = defineProps({
  pokemon: Object,
});

const typeColors = ref({
  normal: "#CAC99C",
  fire: "#EE8130",
  water: "#6390F0",
  electric: "#F7D02C",
  grass: "#7AC74C",
  ice: "#96D9D6",
  fighting: "#C22E28",
  poison: "#c55fc3",
  ground: "#E2BF65",
  flying: "#A98FF3",
  psychic: "#F95587",
  bug: "#A6B91A",
  rock: "#B6A136",
  ghost: "#735797",
  dragon: "#6F35FC",
  dark: "#705746",
  steel: "#B7B7CE",
  fairy: "#D685AD",
});

const statColors = ref({
  hp: "#DF2140",
  attack: "#FF994D",
  defense: "#EECD3D",
  "special-attack": "#85DDFF",
  "special-defense": "#96DA83",
  speed: "#FB94A8",
});

const statNameMap = {
  hp: "HP",
  attack: "ATK",
  defense: "DEF",
  "special-attack": "SpA",
  "special-defense": "SpD",
  speed: "SPD",
};

const emit = defineEmits(["close"]);
</script>
<style></style>
