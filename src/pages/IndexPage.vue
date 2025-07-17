<template>
  <q-page class="q-pa-md">
    <div class="row q-col-gutter-md">

      <!-- Linker Leerbereich (nur Desktop sichtbar) -->
      <div class="col-0 col-md-1" />

      <!-- Hauptinhalt -->
      <div class="col-12 col-md-8">
        <q-input filled v-model="search" label="Suche nach Rezept" />

        <q-card flat class="q-pa-sm q-my-md">
          <div class="row q-gutter-sm">
            <q-btn v-for="cat in categories" :key="cat" :label="cat" flat rounded size="md" :class="[
              'badge-green',
              { active: selectedCategory === cat },
              'q-mb-sm',
              'q-mr-sm'
            ]" @click="selectedCategory = cat" />
          </div>
        </q-card>

        <div class="row q-col-gutter-md">
          <div v-for="recipe in filteredRecipes" :key="recipe.id" class="col-12 col-sm-6 col-md-4 col-lg-3">
            <RecipeCard :recipe="recipe" />
          </div>
        </div>
      </div>

      <!-- Werbung rechts (nur auf Desktop sichtbar) -->
      <div class="col-0 col-md-3">
        <div class="q-pa-md bg-grey-2 text-grey-7 rounded-borders">
          <p>🔸 Werbung</p>
          <p style="font-size: 0.85em;">Hier könnte Ihre Anzeige stehen.</p>
        </div>
      </div>

    </div>
  </q-page>
</template>


<script setup lang="ts">
import RecipeCard from 'components/RecipeCard.vue'


import recipesData from 'src/data/recipes.json'

import { ref, computed } from 'vue'

const search = ref('')
const selectedCategory = ref('')
const recipes = ref(recipesData)

const categories = ['Alle Rezepte', ...new Set(recipes.value.map(r => r.category))];

const filteredRecipes = computed(() => {
  return recipes.value.filter(r =>
    r.title.toLowerCase().includes(search.value.toLowerCase()) &&
    (
      selectedCategory.value === 'Alle Rezepte' ||
      selectedCategory.value === '' ||
      r.category === selectedCategory.value
    )
  );
});
</script>
<style lang="scss" scoped>
.badge-green {
  border: 1px solid;
  border-color: $aspargus;
  color: $aspargus;
}

.badge-green.active {
  background-color: $aspargus;
  color: white;
}
</style>
