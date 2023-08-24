<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated class="bg-purple">
      <q-toolbar>
        <q-avatar>
          <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg" />
        </q-avatar>
        <q-space></q-space>
        <q-input standout v-model="text" class="q-mr-sm" rounded dense>
          <template v-slot:append>
            <q-avatar>
              <q-icon size="md" name="search" />
            </q-avatar>
          </template>
        </q-input>
        <q-btn flat round dense icon="group_add" />
      </q-toolbar>
    </q-header>

    <q-page-container>
      <index-page-vue :meals="mealsList.value" />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import axios from "axios";
import { onMounted, ref, watch, computed } from "vue";
import IndexPageVue from "src/pages/IndexPage.vue";

let meals = ref(null);
let text = ref("");
let filteredMeals = ref([]);

watch(text, (val) => {
  filteredMeals.value = [];
  for (let meal of meals.value) {
    if (meal.strCategory.toLowerCase().includes(val.toLocaleLowerCase())) {
      filteredMeals.value.push(meal);
    }
  }
});

const mealsList = computed(() => {
  return text.value.length ? filteredMeals : meals;
});

onMounted(() => {
  axios
    .get("https://www.themealdb.com/api/json/v1/1/categories.php")
    .then((res) => {
      meals.value = res.data.categories;
      console.log("finished");
    });
});
</script>

<style lang="scss" scoped></style>
