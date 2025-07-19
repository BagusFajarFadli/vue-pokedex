<template>
  <section
    class="flex flex-wrap max-xl:justify-center justify-start gap-3 w-full max-w-7xl px-6 lg:px-12 mx-auto"
  >
    <div
      v-if="isLoading"
      class="text-center text-neutral-600 text-lg py-10 w-full"
    >
      Loading Pokémon data...
    </div>
    <div
      v-else-if="errorMessage"
      class="text-center text-red-600 text-lg py-10 w-full"
    >
      {{ errorMessage }}
    </div>
    <div
      v-else-if="filteredPokemons.length === 0 && !isLoading"
      class="text-center text-neutral-600 text-lg py-10 w-full"
    >
      No Pokémon found matching your search.
    </div>
    <div
      v-else
      v-for="pokemon in filteredPokemons"
      :key="pokemon.id"
      class="w-full max-w-sm p-2 bg-white rounded-xl relative"
    >
      <div class="flex justify-between mb-3">
        <div class="flex gap-2">
          <span
            v-for="typeInfo in pokemon.types"
            :key="typeInfo.slot"
            :style="{ backgroundColor: typeColors[typeInfo.type.name] }"
            class="px-2 py-0.5 text-sm font-semibold rounded-lg size-fit bg-white"
          >
            {{ typeInfo.type.name }}
          </span>
        </div>
        <div class="">
          <h6 class="font-bold text-2xl">{{ pokemon.formattedId }}</h6>
        </div>
      </div>
      <div class="flex flex-col gap-2 w-1/2">
        <h1 class="text-4xl font-bold">{{ pokemon.name }}</h1>
        <p class="text-neutral-500 text-sm">{{ pokemon.description }}</p>
        <button
          @click="emit('selected-pokemon', pokemon)"
          class="py-2 px-4 font-bold bg-emerald-100 w-fit rounded-xl"
        >
          Know More
        </button>
      </div>
      <img
        :src="pokemon.sprites.other.showdown.front_default"
        :alt="'Pokeman' + pokemon.name + ' Image'"
        class="absolute bottom-2 end-2 w-32"
      />
    </div>
  </section>
</template>
<script setup>
import axios from "axios";
import { computed, onMounted, ref, watch } from "vue";

const typeColors = ref({
  normal: "#A8A77A",
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

const props = defineProps({
  keyword: String,
});

const allPokemons = ref([]);
const errorMessage = ref("");
const isLoading = ref(false);

const fetchPokemon = async (nameOrId = "") => {
  isLoading.value = true;
  errorMessage.value = "";
  allPokemons.value = [];
  try {
    let pokemonList = [];

    if (nameOrId) {
      const url = `https://pokeapi.co/api/v2/pokemon/${nameOrId.toLowerCase()}/`;
      const res = await axios.get(url);

      res.data.url = url;
      pokemonList = [res.data];
    } else {
      const response = await axios.get("https://pokeapi.co/api/v2/pokemon");
      pokemonList = response.data.results;
    }

    const detailPromises = pokemonList.map(async (poke) => {
      const pokeUrl = poke.url;
      const res = await axios.get(pokeUrl);
      const pokemonData = res.data;

      const speciesRes = await axios.get(pokemonData.species.url);
      const speciesData = speciesRes.data;

      let description = "No description available.";

      const englishDescription = speciesData.flavor_text_entries.find(
        (entry) =>
          entry.language.name === "en" && entry.version.name === "sword-shield"
      );
      if (!englishDescription) {
        const anyEnglishDescription = speciesData.flavor_text_entries.find(
          (entry) => entry.language.name === "en"
        );
        if (anyEnglishDescription) {
          description = anyEnglishDescription.flavor_text.replace(
            /[\n\f]/g,
            " "
          );
        }
      } else {
        description = englishDescription.flavor_text.replace(/[\n\f]/g, " ");
      }

      const formattedId = String(pokemonData.id).padStart(3, "0");
      return {
        ...pokemonData,
        formattedId: formattedId,
        description: description,
      };
    });
    allPokemons.value = await Promise.all(detailPromises);
  } catch (error) {
    console.error("API Error:", error);
    if (error.response) {
      errorMessage.value = `Pokémon "${nameOrId}" not found. Please try another name or ID.`;
    } else {
      errorMessage.value =
        "Failed to fetch Pokémon data. Please check your internet connection and try again.";
    }
  } finally {
    isLoading.value = false;
  }
};

onMounted(() => {
  fetchPokemon();
});

const filteredPokemons = computed(() => {
  if (!props.keyword) {
    return allPokemons.value;
  }
  const lowerCaseKeyword = props.keyword.toLowerCase();

  return allPokemons.value.filter(
    (pokemon) =>
      pokemon.name.toLowerCase().includes(lowerCaseKeyword) ||
      pokemon.formattedId.includes(lowerCaseKeyword) ||
      String(pokemon.id).includes(lowerCaseKeyword)
  );
});

watch(
  () => props.keyword,
  (newKeyword) => {
    const cleanedKeyword =
      typeof newKeyword === "string" ? newKeyword.trim() : "";

    fetchPokemon(cleanedKeyword);
  },

  { immediate: true }
);

const emit = defineEmits(["selected-pokemon"]);
</script>
<style></style>
