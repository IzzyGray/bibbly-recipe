<template>
  <q-layout view="hHh lpR fFf" class="bg-transparent-header">

    <q-header elevated class="bg-white text-grey-8">
      <q-toolbar class="toolbar-grid q-py-sm q-px-lg">
        <!-- Links -->
        <div class="toolbar-left">
          <q-btn flat dense round @click="toggleLeftDrawer"
            :icon="leftDrawerOpen ? 'sym_o_left_panel_close' : 'sym_o_left_panel_open'" class="q-mr-sm" />
          <q-toolbar-title v-if="$q.screen.gt.xs" shrink class="row items-center no-wrap">
            <span class="app-logo-text q-ml-sm gradient-text">bibbly.</span>
          </q-toolbar-title>
        </div>

        <!-- Mitte: SEARCH (zentriert) -->
        <div class="toolbar-center">
          <q-input dense standout="bg-primary" v-model="search" placeholder="Search for Collections, Links & Keywords">
            <template #prepend>
              <q-icon v-if="search === ''" name="search" />
              <q-icon v-else name="clear" class="cursor-pointer" @click="search = ''" />
            </template>
          </q-input>
        </div>

        <!-- Rechts -->
        <div class="toolbar-right">
          <q-btn-dropdown split outline rounded no-caps size="1rem" color="warning" icon="add"
            class="q-px-md add-link-gap" label="Add link">
            <q-list>
              <q-item clickable v-close-popup>
                <q-item-section avatar>
                  <q-icon color="warning" name="add" />
                </q-item-section>
                <q-item-section>
                  <q-item-label class="text-warning">Add Collection</q-item-label>
                </q-item-section>
              </q-item>
            </q-list>
          </q-btn-dropdown>

          <!-- Account rechtsbündig -->
          <q-btn-dropdown id="accountMenuBtn" class="acc-btn no-hover push-right" flat round icon="account_circle"
            size="lg" color="secondary" dropdown-icon="none" :ripple="false" @show="onAccountMenuShow"
            @hide="onAccountMenuHide">
            <q-list style="min-width: 260px">

              <!-- Header: Avatar + Name + Email -->
              <div class="row items-center q-pa-md q-gutter-md">
                <q-avatar>
                  <q-icon size="lg" color="secondary" name="account_circle" />
                </q-avatar>
                <div class="col">
                  <div class="text-subtitle1">{{ userName }}</div>
                  <div class="text-caption text-grey-7">{{ userEmail }}</div>
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

          <q-item v-ripple v-for="link in drawerDefaultCollections" :key="link.text" clickable>
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
              <q-item class="q-pl-lg hover-actions" v-ripple v-for="link in drawerBibblyCollections" :key="link.text"
                clickable>
                <q-item-section avatar>
                  <q-icon :name="link.icon" />
                </q-item-section>

                <q-item-section>
                  <q-item-label>
                    {{ link.text }}
                  </q-item-label>
                </q-item-section>

                <!-- rechtsbündiger More-Button -->
                <q-item-section side class="actions">
                  <q-btn class="more-btn" flat dense round size="sm" icon="more_vert" aria-label="More actions"
                    @click.stop @mousedown.stop>
                    <q-menu anchor="top right" self="top left" :offset="[0, 0]">
                      <q-list dense style="min-width: 100px">
                        <q-item clickable v-close-popup @click="pinCollection(link)">
                          <q-item-section side class="icon-tight">
                            <q-icon size="xs" name="push_pin" color="grey-8" />
                          </q-item-section>
                          <q-item-section><q-item-label
                              class="text-grey-8 text-caption">Pin</q-item-label></q-item-section>
                        </q-item>
                        <q-item clickable v-close-popup @click="hideCollection(link)">
                          <q-item-section side class="icon-tight">
                            <q-icon size="xs" name="visibility_off" color="grey-8" />
                          </q-item-section>
                          <q-item-section><q-item-label
                              class="text-grey-8 text-caption">Hide</q-item-label></q-item-section>
                        </q-item>
                      </q-list>
                    </q-menu>
                  </q-btn>
                </q-item-section>

              </q-item>
            </q-list>
          </q-expansion-item>

          <q-separator inset class="q-my-sm" />

          <!-- CUSTOM COLLECTIONS -->

          <q-expansion-item icon="bookmark" label="My Collections" dense group="collectionsgroup"
            header-class="text-primary">
            <q-list padding>
              <q-item class="q-pl-lg bg-primary-soft" clickable v-ripple @click="doSomething">
                <q-item-section avatar class="icon-tight">
                  <q-icon name="add" />
                </q-item-section>
                <q-item-section>
                  <q-item-label>New Collection</q-item-label>
                </q-item-section>
              </q-item>

              <q-item class="q-pl-lg hover-actions" v-ripple v-for="link in drawerCustomCollections" :key="link.text"
                clickable>
                <q-item-section avatar>
                  <q-icon :name="link.icon" />
                </q-item-section>

                <q-item-section>
                  <q-item-label>
                    {{ link.text }}
                  </q-item-label>
                </q-item-section>

                <!-- rechtsbündiger More-Button -->
                <q-item-section side class="actions">
                  <q-btn class="more-btn" flat dense round size="sm" icon="more_vert" aria-label="More actions"
                    @click.stop @mousedown.stop>
                    <q-menu anchor="top right" self="top left" :offset="[0, 0]">
                      <q-list dense style="min-width: 100px">
                        <q-item clickable v-close-popup @click="pinCollection(link)">
                          <q-item-section side class="icon-tight">
                            <q-icon size="xs" name="push_pin" color="grey-8" />
                          </q-item-section>
                          <q-item-section><q-item-label
                              class="text-grey-8 text-caption">Pin</q-item-label></q-item-section>
                        </q-item>
                        <q-item clickable v-close-popup @click="editCollection(link)">
                          <q-item-section side class="icon-tight">
                            <q-icon size="xs" name="edit" color="grey-8" />
                          </q-item-section>
                          <q-item-section><q-item-label
                              class="text-grey-8 text-caption">Edit</q-item-label></q-item-section>
                        </q-item>

                        <q-item clickable v-close-popup @click="deleteCollection(link)">
                          <q-item-section side class="icon-tight">
                            <q-icon size="xs" name="delete" color="grey-8" />
                          </q-item-section>
                          <q-item-section><q-item-label
                              class="text-grey-8 text-caption">Delete</q-item-label></q-item-section>
                        </q-item>
                      </q-list>
                    </q-menu>
                  </q-btn>
                </q-item-section>
              </q-item>
            </q-list>
          </q-expansion-item>


          <q-separator inset class="q-my-sm" />

          <!-- SETTINGS -->

          <q-item v-ripple v-for="link in drawerSettings" :key="link.text" clickable>
            <q-item-section>
              <q-item-label>{{ link.text }} <q-icon v-if="link.icon" :name="link.icon" /></q-item-label>
            </q-item-section>
          </q-item>

          <div class="q-mt-md">
            <div class="flex flex-center q-gutter-xs">
              <a class="drawer-footer-link text-grey-6" href="javascript:void(0)" aria-label="About">bibbly · created
                by
                Skysail · ©
                {{ new Date().getFullYear() }}</a>
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
    { icon: 'menu_book', text: 'Bibliography', isbibblytemplate: true },
    { icon: 'shopping_cart', text: 'Shopping', isbibblytemplate: true }
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

    { icon: '', text: 'Settings' },
    { icon: '', text: 'About' },
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

.q-item__label {
  color: #3c4043;
  letter-spacing: .01785714em;
  font-size: .86rem;
  font-weight: 500;
  line-height: 1.25rem;
}

.drawer-footer-link {
  text-decoration: none;
  font-weight: 500;
  font-size: .75rem;
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

/* für more actions button im Drawer
/* Platz rechts reservieren, damit nichts springt */
:deep(.hover-actions .actions) {
  width: 40px;
  /* konstante Breite für den Action-Bereich */
  display: flex;
  justify-content: flex-end;
}

/* Standard: Button verstecken */
:deep(.hover-actions .more-btn) {
  opacity: 0;
  pointer-events: none;
  /* nicht anklickbar, solange unsichtbar */
  transition: opacity .15s ease;
}

/* Auf Hover ODER Tastaturfokus einblenden */
:deep(.hover-actions:hover .more-btn),
:deep(.hover-actions:focus-within .more-btn) {
  opacity: 1;
  pointer-events: auto;
}

/* Blasse Primary als Hintergrund von new collection*/
.bg-primary-soft {
  background: color-mix(in srgb, var(--q-primary) 8%, white);
  /* 8–16% wirkt oft gut */
  border-radius: 0px;
  /* optional, hübscher */
  transition: background .15s ease;
  /* optional */
}

.bg-primary-soft:hover {
  background: color-mix(in srgb, var(--q-primary) 12%, white);
  /* etwas mehr bei Hover */
}



/* 3-Spalten-Grid: links | MITTE (Search) | rechts */
.toolbar-grid {
  display: grid;
  grid-template-columns: 1fr clamp(320px, 40vw, 680px) 1fr;
  /* Mitte bekommt feste Spalte */
  align-items: center;
  column-gap: 16px;
}

/* Links/Rechts als Flex-Zeilen */
.toolbar-left,
.toolbar-right {
  display: flex;
  align-items: center;
  gap: 10px;
  /* Abstand zwischen Button(s) rechts */
}

/* Search füllt die mittlere Spalte vollständig */
.toolbar-center .q-field {
  width: 100%;
}

/* Optional: auf sehr kleinen Screens schmaler werden */
@media (max-width: 700px) {
  .toolbar-grid {
    grid-template-columns: 1fr minmax(260px, 1fr) 1fr;
    column-gap: 12px;
  }
}

/* Right-Spalte als Flex-Zeile */
.toolbar-right {
  display: flex;
  align-items: center;
  gap: 10px;
  min-width: 0;
  /* verhindert komisches Schrumpfen */
}

/* Schiebt genau dieses Element ganz nach rechts */
.push-right {
  margin-left: auto;
}

/* extra Abstand nur links vom Add-Link-Button */
.add-link-gap {
  margin-left: clamp(24px, 5vw, 96px);
}
</style>
