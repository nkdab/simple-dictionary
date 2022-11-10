<script setup>
import { computed, ref } from "vue";
import fuzzySort from "fuzzysort";
import dictionary from "@/constants/dictionary";

const query = ref("");

const results = computed(() => {
  return fuzzySort.go(query.value, dictionary, {
    keys: ["ru", "en", "other"],
    threshold: -10000,
  });
});
</script>

<template>
  <main class="content">
    <div class="fixed h-20 w-full flex items-center justify-center bg-gray-800">
      <input type="text" v-model="query" class="text-black w-11/12" />
    </div>
    <div class="text-gray-900 pt-20">
      <div
        v-if="results.length"
        class="flex flex-col items-center justify-center"
      >
        <div
          v-for="(selected, idx) in results"
          :key="selected.obj.en + idx"
          class="bg-orange-200 w-11/12 mt-3 p-2"
        >
          <div v-if="selected.obj.ru">
            <p class="font-medium text-lg">
              {{ selected.obj.ru }}
              {{ selected.acronym && `(${selected.acronym})` }}
            </p>
          </div>
          <div>
            <div class="w-full bg-gray-500 h-[1px] my-4"></div>
            <p class="font-medium text-lg">{{ selected.obj.en }}</p>
          </div>
          <div v-if="selected.obj.other">
            <div class="w-full bg-gray-500 h-[1px] my-4"></div>
            <p class="font-medium text-lg">{{ selected.obj.other }}</p>
          </div>
        </div>
      </div>
      <div v-else class="text-center text-gray-400">Ничего не найдено...</div>
    </div>
  </main>
</template>

<style></style>
