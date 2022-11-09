<script setup>
import { ref } from "vue";
import fuzzySort from "fuzzysort";
import dictionary from "@/constants/dictionary";
import Autocomplete from "vue3-autocomplete"

const selected = ref(null);
const results = ref([])
const autocomplete = ref(null)

const search = (val) => {
  results.value = fuzzySort
    .go(val, dictionary, { keys: ["ru", "en", "other"] })
    .map((i) => i.obj);
};
const select = (val) => {
  selected.value = val;
  autocomplete.value.autocompleteRef.blur();
}
</script>

<template>
  <main class="content">
    <Autocomplete
      ref="autocomplete"
      @input="search"
      @change="search"
      @onSelect="select"
      :display-item="(item) => item.en + ' / ' + item.ru"
      :results="results"
      :input-class="['text-black w-full rounded']"
      :results-container-class="['absolute bg-white text-black']"
      :results-item-class="['cursor-pointer hover:bg-gray-100']"
    />
    <div class="results mt-4 text-gray-900 p-2 bg-orange-200 w-full">
      <div
        v-if="selected && selected.en"
      >
        <div v-if="selected.ru">
          <h1 class="font-medium text-lg">Русский</h1>
          <p>{{ selected.ru }} {{ selected.acronym && `(${selected.acronym})` }}</p>
        </div>
        <div>
          <div class="w-full bg-gray-500 h-[1px] my-4"></div>
          <h1 class="text-lg font-medium">English</h1>
          <p>{{ selected.en }}</p>
        </div>
        <div v-if="selected.other">
          <div class="w-full bg-gray-500 h-[1px] my-4"></div>
          <h1 class="text-lg font-medium">Другое</h1>
          <p>{{ selected.other }}</p>
        </div>
      </div>
      <div v-else class="text-center">Ничего не найдено...</div>
    </div>
  </main>
</template>

<style></style>
