<template>
  <q-layout view="hHh lpR fFf">

    <!-- Variante 1: Kleiner Header für bestimmte Seiten -->
    <q-header v-if="isSimpleHeader" class="bg-transparent-header">
      <q-toolbar class="justify-between q-py-md">
        <div class="row items-center cursor-pointer q-ml-xl" @click="goToWelcome">

          <img src="../assets/images/logo.png" alt="Bibbly Logo" class="logo-image" />
          <q-toolbar-title class="app-logo-text">
            bibbly.
          </q-toolbar-title>
        </div>
        <div class="row items-center q-gutter-sm q-pr-sm q-mr-xl">
          <!-- Desktop: Zwei Buttons -->
          <div class="row q-gutter-sm items-center" v-if="$q.screen.gt.sm">
            <q-btn flat size="lg" style="color:#191919" class="text-capitalize" @click="goToLogin">
              <span style="font-size: 1rem;">
                Login
              </span>
            </q-btn>
            <q-btn size="md" color="secondary" class="text-capitalize" @click="goToRegister">
              <span style="font-size: 1rem;">
                Sign Up
              </span>
              <q-icon name="arrow_forward" size="16px" class="q-ml-sm" />
            </q-btn>
          </div>

          <!-- Mobile: Kombinierter Button mit Menü -->
          <q-btn dense round flat icon="login" color="secondary" v-else>
            <q-menu>
              <q-list style="min-width: 120px">
                <q-item clickable v-close-popup @click="goToLogin">
                  <q-item-section>Login</q-item-section>
                </q-item>
                <q-item clickable v-close-popup @click="goToRegister">
                  <q-item-section>Sign Up</q-item-section>
                </q-item>
              </q-list>
            </q-menu>
          </q-btn>
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
    <q-drawer v-if="!isSimpleHeader" side="right" v-model="rightDrawerOpen" overlay bordered behavior="mobile">
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

        <q-item clickable v-ripple @click="openImpressum">
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
    <footer class="q-pa-sm q-px-lg q-mt-lg bg-grey-1 text-grey-9">

      <!-- Desktop/Footer links horizontal -->
      <div v-if="$q.screen.gt.sm" class="column items-center q-mb-md">
        <div class="text-subtitle1 text-uppercase text-grey-6 text-weight-bold q-mb-sm">
          Where to?
        </div>

        <div class="row q-gutter-md justify-center">
          <q-btn flat dense label="FAQ" class="text-capitalize text-caption" />
          <q-btn flat dense label="Legal Notice" class="text-capitalize text-caption" @click="openImpressum" />
          <q-btn flat dense label="Privacy" class="text-capitalize text-caption" />
          <q-btn flat dense label="Disclaimer" class="text-capitalize text-caption" />
          <q-btn flat dense label=" About" class="text-capitalize text-caption" />
          <q-btn flat dense label="Contact" class="text-capitalize text-caption" @click="contactDialog = true" />
        </div>
        <!-- Trennlinie -->
        <q-separator spaced class="q-mt-md" />

        <!-- Copyright -->
        <div class="row justify-end q-px-md q-mt-sm full-width">
          <div class="text-caption text-grey-5 text-right">
            © 2025 Skysail Consulting GmbH. All rights reserved.
          </div>
        </div>
      </div>

      <!-- Mobile/Tablet/Footer links vertikal -->
      <div v-else class="column items-center q-mb-md text-center">
        <!-- Titel -->
        <div class="text-subtitle2 text-uppercase text-grey-6 text-weight-bold q-mb-sm">
          Where to?
        </div>

        <!-- Vertikale Linkliste -->
        <div class="column items-center text-caption q-gutter-xs">
          <div class="cursor-pointer" @click="contactDialog = true">Contact</div>
          <div class="cursor-pointer" @click="openImpressum()">Legal Notice</div>
          <div class="cursor-pointer">Disclaimer</div>
          <div class="cursor-pointer">Privacy</div>
        </div>
        <!-- Trennlinie -->
        <q-separator spaced class="q-mt-md" />

        <!-- Copyright -->
        <div class="text-caption text-grey-5 text-center q-mt-xs">
          © 2025 Skysail Consulting GmbH. All rights reserved.
        </div>
      </div>

    </footer>

  </q-layout>
  <!-- Impressum Dialog -->
  <q-dialog v-model="showImpressum" persistent full-width transition-show="fade" transition-hide="fade">
    <q-card class="q-pa-md" style="max-width: 800px; width: 90vw; max-height: 90vh;">
      <q-card-section class="row items-center justify-between">
        <div class="text-h6">Information according to section 5 DDG (Digitale Dienste Gesetz)
        </div>
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>

      <q-separator />

      <q-card-section class="scroll" style="max-height: 70vh;">
        <div>
          <p><i>This legal notice applies to the online service Bibbly (www.bibbly.io), a product of Skysail
              Consulting GmbH.</i></p>
          <p>
            Skysail Consulting GmbH <br>
            Spielwang 7<br />
            83377 Vachendorf<br />
            Germany</p>

          <p><strong>Contact:</strong></p>
          <p>Email: info@skysail.io<br />
            Phone: + 49 (0) 861 166 267 81</p>

          <p><strong>Managing Director:</strong></p>
          <p>Carsten Gräf</p>

          <p><strong>Register Entry:</strong></p>
          <p>Registration in the commercial register.<br />
            Register court: Commercial Register B Traunstein<br />
            Register number: HRB 27170</p>

          <p><strong>VAT Number:</strong></p>
          <p>DE319194915</p>
        </div>
      </q-card-section>

      <q-separator />

      <q-card-actions align="right">
        <q-btn flat label="Close" color="teal" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>

  <!-- Kontakt Dialog -->
  <q-dialog v-model="contactDialog" persistent>
    <q-card style="min-width: 300px; max-width: 600px; width: 90vw;">
      <q-card-section class="text-h6">
        Get in Touch
      </q-card-section>

      <q-card-section>
        <q-form @submit.prevent="submitContactForm">
          <div class="q-mx-auto column q-gutter-md" style="max-width: 500px;">
            <q-input v-model="name" label="Name" filled required />
            <q-input v-model="email" label="Email" filled type="email" :rules="[validateEmail]" required />
            <q-input v-model="message" label="Message" filled type="textarea" autogrow :rules="[validateMessage]"
              required bottom-slots>
              <template #hint>
                Hint: Please enter between 80 - 300 characters.
              </template>
            </q-input>

            <q-input v-model="captchaAnswer" :label="captchaQuestion" filled type="number" :rules="[validateCaptcha]"
              required class="q-mt-md" />

            <div class="q-mt-md row justify-end q-gutter-sm">
              <q-btn flat label="Cancel" @click="contactDialog = false" />
              <q-btn label="Send" type="submit" color="primary" />
            </div>
          </div>
        </q-form>
      </q-card-section>
    </q-card>
  </q-dialog>

</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import { useRouter } from 'vue-router'
import { useQuasar } from 'quasar'


const rightDrawerOpen = ref(false);
const route = useRoute();
const router = useRouter();

// Hier gibst du die Routen-Namen an, bei denen der Zurück-Button angezeigt werden soll
const showBackButton = computed(() =>
  ['recipe-detail'].includes(route.name as string)
);
const showImpressum = ref(false);

//Kontaktformular
const contactDialog = ref(false);
const name = ref('')
const email = ref('')
const message = ref('')
const $q = useQuasar()

//Captcha
const captchaAnswer = ref('')
const captchaQuestion = ref('')
const correctAnswer = ref(0)

const simpleHeaderRoutes = ['welcome', 'login', 'register', 'preview']

const isSimpleHeader = computed(() =>
  typeof route.name === 'string' && simpleHeaderRoutes.includes(route.name)
)

async function goToWelcome() {
  await router.push('/')
}

async function goBack() {
  await router.push('/home') // oder router.back()
}

async function goToLogin() {
  await router.push('/login')
}


async function goToRegister() {
  await router.push('/register')
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

function openImpressum() {
  showImpressum.value = true;
}

function submitContactForm() {

  // Hier könntest du eine API ansprechen

  console.log({ name: name.value, email: email.value, message: message.value })
  // Felder zurücksetzen
  name.value = ''
  email.value = ''
  message.value = ''
  captchaAnswer.value = ''
  generateCaptcha()

  // Dialog schließen

  contactDialog.value = false;

  // Bestätigung anzeigen

  $q.notify({
    type: 'positive',
    message: 'Message sent! 🐦 Even the pigeons are impressed.',
    color: 'secondary',
    position: 'top',
    timeout: 4000
  })
}

function validateEmail(val: string) {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(val) || 'Bitte gültige E-Mail-Adresse eingeben'
}

function validateMessage(val: string) {
  if (!val) return 'Message is required'
  if (val.length < 80) return 'Message must be at least 80 characters'
  if (val.length > 300) return 'Message must not exceed 300 characters'

  const blacklist = ["select", "insert", "update", "delete", "drop", "--", ";", "' OR", "\" OR", "1=1"]
  const lowerVal = val.toLowerCase()

  if (blacklist.some(keyword => lowerVal.includes(keyword))) {
    return 'Please rephrase your message.'
  }

  return true
}

onMounted(() => {
  generateCaptcha()
})

function generateCaptcha() {
  const a = Math.floor(Math.random() * 10) + 1
  const b = Math.floor(Math.random() * 10) + 1
  captchaQuestion.value = `What is ${a} + ${b}?`
  correctAnswer.value = a + b
  captchaAnswer.value = ''
}

function validateCaptcha(val: string) {
  return parseInt(val) === correctAnswer.value || 'Incorrect captcha answer'
}



function onLogout() {
  console.log('Logout geklickt')
}

</script>

<style scoped>
.bg-transparent-header {
  background-color: rgba(255, 255, 255, 0.8);
  /* Weiß mit 80% Deckkraft */
  backdrop-filter: blur(10px);
  /* optional für Glassmokeffekt */
}

.logo-image {
  height: 30px;
  object-fit: contain;
}

.q-layout {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.q-page-container {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.q-page {
  flex: 1;
  display: flex;
  flex-direction: column;
}
</style>
