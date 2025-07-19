<template>
  <div class="bg-stone-100 min-h-screen relative">
    <Navbar />
    <Hero @search="handleSearch" />
    <CardGallery
      :keyword="currentKeyword"
      @selected-pokemon="handleSelectedPokemon"
      class="mt-8"
    />
    <modalPokemonDetail
      v-if="currentPokemonSelected !== null"
      :pokemon="currentPokemonSelected"
      @close="currentPokemonSelected = null"
    />
  </div>
</template>
<script setup>
import Navbar from "./components/Navbar.vue";
import Hero from "./components/Hero.vue";
import CardGallery from "./components/CardGallery.vue";
import modalPokemonDetail from "./components/modalPokemonDetail.vue";
import { ref } from "vue";

const currentKeyword = ref("");
const currentPokemonSelected = ref(null);
const handleSearch = (keyword) => {
  currentKeyword.value = keyword;
};

const handleSelectedPokemon = (pokemon) => {
  currentPokemonSelected.value = pokemon;
};

//selectedPokemon.value.id
</script>
<style scoped></style>
