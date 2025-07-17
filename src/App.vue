<template>
  <div class="bg-stone-100 min-h-screen">
    <Navbar />
    <Hero @search="handleSearch" />
    <CardGallery :keyword="currentKeyword" class="mt-8" />
  </div>
</template>
<script setup>
import Navbar from "./components/Navbar.vue";
import Hero from "./components/Hero.vue";
import CardGallery from "./components/CardGallery.vue";
import { ref } from "vue";

const currentKeyword = ref("");

const handleSearch = (keyword) => {
  currentKeyword.value = keyword;
};
</script>
<style scoped></style>
