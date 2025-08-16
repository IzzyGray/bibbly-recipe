<template>
  <q-layout view="hHh lpR fFf" class="bg-transparent-header">

    <q-header class="bg-white text-grey-8">
      <q-toolbar class="q-py-sm q-px-lg items-center justify-between">
        <q-btn flat dense round @click="toggleLeftDrawer" aria-label="Menu" icon="menu" class="q-mr-sm" />

        <q-toolbar-title v-if="$q.screen.gt.xs" shrink class="row items-center no-wrap">
          <span class="app-logo-text q-ml-sm gradient-text">bibbly.</span>
        </q-toolbar-title>

        <q-space />

        <q-input class="GNL__toolbar-input" outlined dense v-model="search" color="bg-grey-7"
          placeholder="Search for Collections, Links & Keywords">
          <template v-slot:prepend>
            <q-icon v-if="search === ''" name="search" />
            <q-icon v-else name="clear" class="cursor-pointer" @click="search = ''" />
          </template>
        </q-input>

        <q-space />

        <div class="q-gutter-sm row items-center no-wrap">
          <!-- <q-btn v-if="$q.screen.gt.sm" round dense flat color="text-grey-7" icon="apps">
            <q-tooltip>Google Apps</q-tooltip>
          </q-btn>
          <q-btn round dense flat color="grey-8" icon="notifications">
            <q-badge color="red" text-color="white" floating>
              2
            </q-badge>
            <q-tooltip>Notifications</q-tooltip>
          </q-btn> -->
          <q-btn-dropdown id="accountMenuBtn" class="acc-btn no-hover" flat round size="lg" icon="account_circle"
            color="secondary" dropdown-icon="none" :ripple="false" @show="onAccountMenuShow" @hide="onAccountMenuHide">

            <q-list style="min-width: 260px">

              <!-- Header: Avatar + Name + Email -->
              <div class="row items-center q-pa-md q-gutter-md">
                <q-avatar>
                  <q-icon size="lg" color="secondary" name="account_circle" />
                </q-avatar>
                <div class="col">
                  <div class="text-subtitle1">{{ userName }}</div>
                  <div class="text-caption text-grey-7">john.doe@email.com</div>
                </div>
              </div>

              <!-- Get a plan -->
              <div class="q-px-md q-pb-sm">
                <q-btn label="Get a plan" color="primary" unelevated class="full-width" @click="getPlan"
                  v-close-popup />
              </div>

              <q-separator spaced />

              <!-- Account -->
              <q-item clickable v-ripple @click="showAccount" v-close-popup>
                <q-item-section avatar>
                  <q-icon name="manage_accounts" />
                </q-item-section>
                <q-item-section>
                  <q-item-label>Account</q-item-label>
                </q-item-section>
              </q-item>

              <!-- Subscription -->
              <q-item clickable v-ripple @click="openSubscription" v-close-popup>
                <q-item-section avatar>
                  <q-icon name="credit_card" />
                </q-item-section>
                <q-item-section>
                  <q-item-label>Subscription</q-item-label>
                </q-item-section>
                <q-item-section side>
                  <q-badge color="secondary" rounded class="q-ml-sm">free</q-badge>
                </q-item-section>
              </q-item>

              <!-- Language -->
              <!--             <q-item clickable v-ripple @click="openLanguage" v-close-popup>
                <q-item-section avatar>
                  <q-icon name="language" />
                </q-item-section>
                <q-item-section>
                  <q-item-label>Language</q-item-label>
                </q-item-section>
              </q-item> -->

              <!-- Help center -->
              <!--             <q-item clickable v-ripple @click="openHelpCenter" v-close-popup>
                <q-item-section avatar>
                  <q-icon name="help_center" />
                </q-item-section>
                <q-item-section>
                  <q-item-label>Help center</q-item-label>
                </q-item-section>
              </q-item> -->

              <q-separator spaced />

              <!-- Logout -->
              <q-item clickable v-ripple @click="logoutUser" v-close-popup>
                <q-item-section avatar>
                  <q-icon name="logout" />
                </q-item-section>
                <q-item-section>
                  <q-item-label>Log out</q-item-label>
                </q-item-section>
              </q-item>
            </q-list>
          </q-btn-dropdown>

          <!-- Tooltip separat, mit Ziel auf den Button -->
          <q-tooltip ref="accTooltip" :target="'#accountMenuBtn'" :disable="isAccountMenuOpen"
            class="bg-white text-grey-9 shadow-3" anchor="bottom middle" self="top middle" :offset="[0, 10]"
            transition-show="jump-down" transition-hide="jump-up">
            <div class="text-weight-medium q-mb-xs">bibbly Account</div>
            <div class="text-caption">{{ userName }}</div>
            <div class="text-caption">{{ userEmail }}</div>
          </q-tooltip>
        </div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered class="bg-white" :width="280">
      <q-scroll-area class="fit">
        <q-list padding class="text-grey-8">

          <!-- DEFAULT COLLECTIONS -->

          <q-item class="GNL__drawer-item" v-ripple v-for="link in drawerDefaultCollections" :key="link.text" clickable>
            <q-item-section avatar>
              <q-icon :name="link.icon" />
            </q-item-section>
            <q-item-section>
              <q-item-label>{{ link.text }}</q-item-label>
            </q-item-section>
          </q-item>

          <q-separator inset class="q-my-sm" />

          <!-- BIBBLY COLLECTIONS -->

          <q-expansion-item icon="bookmark" label="bibbly Collections" dense group="collectionsgroup" default-opened
            header-class="text-secondary">
            <q-list>
              <q-item class="GNL__drawer-item q-pl-lg" v-ripple v-for="link in drawerBibblyCollections" :key="link.text"
                clickable>
                <q-item-section avatar>
                  <q-icon :name="link.icon" />
                </q-item-section>

                <q-item-section>
                  <q-item-label>
                    {{ link.text }}
                  </q-item-label>
                </q-item-section>
              </q-item>
            </q-list>
          </q-expansion-item>

          <q-separator inset class="q-my-sm" />

          <!-- CUSTOM COLLECTIONS -->

          <q-expansion-item icon="bookmark" label="My Collections" dense group="collectionsgroup"
            header-class="text-primary">
            <q-list padding>
              <q-item class="GNL__drawer-item q-pl-lg" clickable v-ripple @click="doSomething">
                <q-item-section>
                  <q-icon name="create_new_folder" />
                  New Collection
                </q-item-section>
              </q-item>
              <q-item class="GNL__drawer-item q-pl-lg" v-ripple v-for="link in drawerCustomCollections" :key="link.text"
                clickable>
                <q-item-section avatar>
                  <q-icon :name="link.icon" />
                </q-item-section>

                <q-item-section>
                  <q-item-label>
                    {{ link.text }}
                  </q-item-label>
                </q-item-section>
              </q-item>
            </q-list>
          </q-expansion-item>


          <q-separator inset class="q-my-sm" />

          <!-- SETTINGS -->

          <q-item class="GNL__drawer-item" v-ripple v-for="link in drawerSettings" :key="link.text" clickable>
            <q-item-section>
              <q-item-label>{{ link.text }} <q-icon v-if="link.icon" :name="link.icon" /></q-item-label>
            </q-item-section>
          </q-item>

          <div class="q-mt-md">
            <div class="flex flex-center q-gutter-xs">
              <a class="GNL__drawer-footer-link" href="javascript:void(0)" aria-label="Privacy">Privacy</a>
              <span> · </span>
              <a class="GNL__drawer-footer-link" href="javascript:void(0)" aria-label="Terms">Terms</a>
              <span> · </span>
              <a class="GNL__drawer-footer-link" href="javascript:void(0)" aria-label="About">© 2025 Skysail</a>
            </div>
          </div>
        </q-list>
      </q-scroll-area>
    </q-drawer>



    <!-- Seiteninhalt -->
    <q-page-container>
      <router-view />
    </q-page-container>

  </q-layout>

</template>

<script setup lang="ts">

import { ref } from 'vue'
//start new  const & functions

const leftDrawerOpen = ref(false);
const search = ref('');

const userName = ref('John Doe')
const userEmail = ref('john.doe@email.com')

const drawerDefaultCollections =

  [
    { icon: 'web', text: 'Top links' },
    { icon: 'person', text: 'For you' },
    { icon: 'star_border', text: 'Favourites' },
    { icon: 'search', text: 'Saved searches' }
  ]

const drawerBibblyCollections =

  [
    { icon: 'outdoor_grill', text: 'Recipies', isbibblytemplate: true },
    { icon: 'radio', text: 'News', isbibblytemplate: true },
    { icon: 'beach_access', text: 'Travel', isbibblytemplate: true },
    { icon: 'restaurant_menu', text: 'Restaurants', isbibblytemplate: true },
  ]

const drawerCustomCollections =

  [

    { icon: '', text: 'Work', isbibblytemplate: false },
    { icon: '', text: 'My Italy trip 2025', isbibblytemplate: false },
    { icon: '', text: 'Templates', isbibblytemplate: false },
    { icon: '', text: 'Gym', isbibblytemplate: false }
  ]

const drawerSettings =
  [
    { icon: '', text: 'Language & region' },
    { icon: '', text: 'Settings' },
    //{ icon: 'open_in_new', text: 'Get the Android app' },
    //{ icon: 'open_in_new', text: 'Get the iOS app' },
    //{ icon: '', text: 'Send feedback' },
    { icon: 'open_in_new', text: 'Help' }
  ]

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value
}


function getPlan() {

}

function showAccount() {

}

function openHelpCenter() {

}

function logoutUser() {

}

const isAccountMenuOpen = ref(false)
const accTooltip = ref(null)

function onAccountMenuShow() {
  isAccountMenuOpen.value = true       // verhindert erneutes Anzeigen
  accTooltip.value?.hide()             // falls Tooltip gerade sichtbar ist -> sofort ausblenden
}
function onAccountMenuHide() {
  isAccountMenuOpen.value = false
}



//end new functions




</script>

<style scoped>
.bg-transparent-header {
  background-color: rgba(255, 255, 255, 0.8);
  /* Weiß mit 80% Deckkraft */
  backdrop-filter: blur(10px);
  /* optional für Glassmokeffekt */
}

/* Basis-Layer (vor/after) neutralisieren */
:deep(.no-hover .q-btn__wrapper::before),
:deep(.no-hover .q-btn__wrapper::after) {
  opacity: 0 !important;
  background: transparent !important;
  content: '' !important;
  /* falls ein Theme content setzt */
}

/* explizit die Hover/Active-Zustände überstimmen */
:deep(.no-hover.q-btn--actionable:hover .q-btn__wrapper::before),
:deep(.no-hover.q-btn--active .q-btn__wrapper::before),
:deep(.no-hover.q-btn--actionable:hover .q-btn__wrapper::after),
:deep(.no-hover.q-btn--active .q-btn__wrapper::after) {
  opacity: 0 !important;
}

/* Fokus-Hilfslayer entfernen (Tastaturfokus) – optional */
:deep(.no-hover .q-focus-helper) {
  opacity: 0 !important;
}
</style>
