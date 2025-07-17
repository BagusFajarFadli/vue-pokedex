<template>
  <section class="flex flex-col items-center gap-8 px-6 lg:px-12 box-border">
    <img :src="imagePath.logo" alt="" class="w-48" />
    <form
      @submit.prevent="searchEmits"
      class="flex justify-center gap-2 w-full max-w-7xl"
    >
      <input
        v-model="searchTerms"
        type="text"
        placeholder="search eg, ditto or pikachu"
        class="px-4 py-2 border border-neutral-700 rounded-lg shadow-2xl w-full"
      />
      <button
        type="submit"
        class="bg-rose-600 text-white p-2 lg:p-4 rounded-lg shadow-2xl"
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
            d="m21 21-5.197-5.197m0 0A7.5 7.5 0 1 0 5.196 5.196a7.5 7.5 0 0 0 10.607 10.607Z"
          />
        </svg>
      </button>
    </form>
  </section>
</template>
<script setup>
import { ref } from "vue";

const imagePath = ref({
  logo: "/pokedex-logo.png",
});

const searchTerms = ref("");

const emit = defineEmits(["search"]);

const searchEmits = () => {
  emit("search", searchTerms.value.trim());
};
</script>
<style></style>
