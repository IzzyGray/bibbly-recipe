<template>
  <q-page class="flex flex-center bg-grey-2 q-pa-md">
    <q-card class="q-pa-md" style="max-width: 500px; width: 100%;">
      <q-card-section class="text-center">
        <q-icon name="restaurant_menu" size="48px" color="teal" />
        <div class="text-h5 q-mt-sm">Rezeptvorschau</div>
        <div class="text-subtitle2 text-grey-7 q-mt-xs">
          URL: {{ recipeUrl }}
        </div>
      </q-card-section>

      <q-separator />

      <q-card-section>
        <div class="text-body1">
          Hier könnte dein Rezept erscheinen – diese Seite ist eine Vorschau.
        </div>
      </q-card-section>

      <q-card-actions align="right">
        <q-btn flat label="Zurück" icon="arrow_back" @click="goBack" />
      </q-card-actions>
    </q-card>
  </q-page>
</template>

<script setup lang="ts">
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

const recipeUrl = route.query.url as string || 'Keine URL angegeben'

async function goBack() {
  await router.push('/')
}
</script>
