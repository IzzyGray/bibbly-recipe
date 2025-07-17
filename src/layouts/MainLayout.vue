<template>
  <q-layout view="hHh pR fFf">

    <!-- Variante 1: Kleiner Header für bestimmte Seiten -->
    <q-header v-if="isSimpleHeader" class="bg-white text-primary" elevated>
      <q-toolbar class="justify-between">
        <div class="row items-center">
          <q-avatar icon="restaurant_menu" color="primary" text-color="white" />
          <q-toolbar-title class="q-ml-sm">Bibbly</q-toolbar-title>
        </div>
        <div class="row items-center q-gutter-sm q-pr-sm">
          <q-btn flat label="Login" color="primary" @click="goToLogin" />
          <q-btn flat label="Sign Up" color="secondary" @click="goToRegister" />
        </div>
      </q-toolbar>
    </q-header>

    <!-- Variante 2: Voller Header mit Menü-Button -->
    <q-header v-else elevated style="background-color: #087F8C; color: white;">
      <q-toolbar class="q-gutter-sm justify-between">
        <div class="row items-center">
          <q-btn v-if="showBackButton" flat dense icon="arrow_back" @click="goBack"
            :label="$q.screen.gt.sm ? 'Zurück' : undefined" :round="$q.screen.lt.sm" />
        </div>

        <q-toolbar-title class="text-center app-title">
          bibbly <span class="subtitle">recipes</span>
        </q-toolbar-title>

        <q-btn flat round dense icon="menu" @click="rightDrawerOpen = !rightDrawerOpen" />
      </q-toolbar>
    </q-header>

    <!-- Right Drawer -->
    <q-drawer v-if="!isSimpleHeader" side="right" v-model="rightDrawerOpen" overlay bordered behavior="desktop">
      <q-list>

        <q-item clickable v-ripple @click="onImport">
          <q-item-section avatar><q-icon name="file_upload" /></q-item-section>
          <q-item-section>Importieren</q-item-section>
        </q-item>

        <q-item clickable v-ripple @click="onShare">
          <q-item-section avatar><q-icon name="share" /></q-item-section>
          <q-item-section>Teilen</q-item-section>
        </q-item>

        <q-item clickable v-ripple @click="onAgb">
          <q-item-section avatar><q-icon name="gavel" /></q-item-section>
          <q-item-section>AGB</q-item-section>
        </q-item>

        <q-item clickable v-ripple @click="onImpressum">
          <q-item-section avatar><q-icon name="info" /></q-item-section>
          <q-item-section>Impressum</q-item-section>
        </q-item>

        <q-separator spaced />

        <q-item clickable v-ripple @click="onLogout">
          <q-item-section avatar><q-icon name="logout" /></q-item-section>
          <q-item-section>Abmelden</q-item-section>
        </q-item>

      </q-list>
    </q-drawer>

    <!-- Seiteninhalt -->
    <q-page-container>
      <router-view />
    </q-page-container>

  </q-layout>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { useRoute } from 'vue-router';
import { useRouter } from 'vue-router'

const rightDrawerOpen = ref(false);
const route = useRoute();
const router = useRouter();
// Hier gibst du die Routen-Namen an, bei denen der Zurück-Button angezeigt werden soll
const showBackButton = computed(() =>
  ['recipe-detail'].includes(route.name as string)
);

const simpleHeaderRoutes = ['welcome', 'login', 'register']

const isSimpleHeader = computed(() =>
  typeof route.name === 'string' && simpleHeaderRoutes.includes(route.name)
)

function goBack() {
  void router.push('/home') // oder router.back()
}

function goToLogin() {
  router.push('/login')
}


function goToRegister() {
  router.push('/register')
}

// Beispielaktionen:
function onImport() {
  console.log('Importieren geklickt')
}

function onShare() {
  console.log('Teilen geklickt')
}

function onAgb() {
  console.log('AGB geklickt')
}

function onImpressum() {
  console.log('Impressum geklickt')
}


function onLogout() {
  console.log('Logout geklickt')
}
</script>

<style scoped></style>
