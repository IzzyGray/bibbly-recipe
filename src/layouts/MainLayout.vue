<template>
  <q-layout view="hHh lpR fFf">

    <!-- Variante 1: Header für Welcome Seiten ohne Anmeldung -->
    <q-header v-if="isSimpleHeader" class="bg-transparent-header">
      <q-toolbar class="q-py-md q-px-lg q-gutter-md items-center">

        <!-- Logo links -->
        <div class="row items-center cursor-pointer q-gutter-sm" @click="goToWelcome">
          <img src="../assets/images/logo.png" alt="Bibbly Logo" class="logo-image" />
          <q-toolbar-title class="app-logo-text q-ml-sm">
            bibbly.
          </q-toolbar-title>
        </div>

        <!-- Zentrale Navigation -->
        <div class="row q-gutter-md justify-center items-center q-mx-auto" v-if="$q.screen.gt.sm">
          <q-btn flat class="text-grey text-capitalize" @click="() => scrollToSection('collections')">
            <span style="font-size: 1rem;">Collections</span>
          </q-btn>
          <q-btn flat class="text-grey text-capitalize" @click="() => scrollToSection('product')">
            <span style="font-size: 1rem;">Product</span>
          </q-btn>
          <q-btn flat class="text-grey text-capitalize" @click="() => scrollToSection('about')">
            <span style=" font-size: 1rem;">About</span>
          </q-btn>
          <q-btn outline rounded color="warning" class="text-capitalize" @click="() => scrollToSection('extension')">
            <span style=" font-size: 1rem;">{{ extensionLabel }}</span>
          </q-btn>
        </div>

        <!-- Rechte Buttons (Login / Sign Up) -->
        <div class="row items-center q-gutter-sm">
          <!-- Desktop -->
          <div class="row q-gutter-sm items-center" v-if="$q.screen.gt.sm">
            <q-btn flat class="text-capitalize" style="color:#191919" @click="goToLogin">
              <span style="font-size: 1rem;">Login</span>
            </q-btn>
            <q-btn color="secondary" class="text-capitalize" @click="goToRegister">
              <span style="font-size: 1rem;">Sign Up</span>
              <q-icon name="arrow_forward" size="16px" class="q-ml-sm" />
            </q-btn>
          </div>

          <!-- Mobile -->
          <q-btn dense round flat icon="login" color="secondary" v-else>
            <q-menu>
              <q-list style="min-width: 120px">
                <q-item clickable v-close-popup @click="() => scrollToSection('collections')">
                  <q-item-section>Collections</q-item-section>
                </q-item>
                <q-item clickable v-close-popup @click="() => scrollToSection('product')">
                  <q-item-section>Product</q-item-section>
                </q-item>
                <q-item clickable v-close-popup @click="() => scrollToSection('about')">
                  <q-item-section>About</q-item-section>
                </q-item>
                <q-item clickable v-close-popup @click="() => scrollToSection('extension')"
                  class="text-warning text-italic">
                  <q-item-section>Add to Chrome</q-item-section>
                </q-item>
                <q-separator spaced />
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
          <q-item-section>Import</q-item-section>
        </q-item>

        <q-item clickable v-ripple @click="onShare">
          <q-item-section avatar><q-icon name="share" /></q-item-section>
          <q-item-section>Share</q-item-section>
        </q-item>

        <q-item clickable v-ripple @click="onAgb">
          <q-item-section avatar><q-icon name="gavel" /></q-item-section>
          <q-item-section>Terms</q-item-section>
        </q-item>

        <q-item clickable v-ripple @click="openImpressum">
          <q-item-section avatar><q-icon name="info" /></q-item-section>
          <q-item-section>Legal Notice</q-item-section>
        </q-item>

        <q-separator spaced />

        <q-item clickable v-ripple @click="onLogout">
          <q-item-section avatar><q-icon name="logout" /></q-item-section>
          <q-item-section>Logout</q-item-section>
        </q-item>

      </q-list>
    </q-drawer>

    <!-- Seiteninhalt -->
    <q-page-container>
      <router-view />
    </q-page-container>
    <footer class="q-pa-sm q-px-lg q-mt-lg bg-grey-1 text-grey-9">

      <!-- Desktop/Footer links horizontal -->
      <div v-if="$q.screen.gt.sm" class="column items-center q-mb-md q-mt-md">

        <div class="row q-gutter-md justify-center">
          <q-btn flat dense label="Legal Notice" class="text-capitalize text-caption" @click="openImpressum" />
          <q-btn flat dense label="Privacy" class="text-capitalize text-caption" @click="openPrivacy" />
          <q-btn flat dense label="Disclaimer" class="text-capitalize text-caption" @click="openDisclaimer" />
          <q-btn flat dense label="Contact" class="text-capitalize text-caption" @click="contactDialog = true" />
        </div>

        <!-- Attribution & Copyright in einer Zeile -->
        <div class="row items-start q-px-sm q-mt-xl full-width">
          <!-- Linke Spalte -->
          <div class="col text-caption text-grey-5">
            Images designed by
            <a href="https://www.freepik.com" target="_blank" class="text-secondary">Freepik</a>
          </div>

          <!-- Mittlere Spalte: Back to top -->
          <div class="col-auto">
            <q-btn flat dense class="text-capitalize text-caption" @click="scrollToTop" label="Back to Top"
              icon-right="arrow_upward" size="sm" color="grey" />
          </div>

          <!-- Rechte Spalte -->
          <div class="col text-caption text-grey-5 text-right">
            © 2025
            <a href="https://www.skysail.io" target="_blank" class="text-secondary">Skysail Consulting GmbH</a>.
            All rights reserved.
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

          <div class="cursor-pointer" @click="openImpressum">Legal Notice</div>
          <div class="cursor-pointer" @click="openPrivacy">Privacy</div>
          <div class="cursor-pointer" @click="openDisclaimer">Disclaimer</div>
          <div class="cursor-pointer" @click="contactDialog = true">Contact</div>
          <div class="cursor-pointer">Privacy</div>
        </div>
        <!-- Trennlinie -->
        <q-separator spaced class="q-mt-md" />

        <!-- Attribution + Copyright in einer Zeile -->
        <div class="row justify-between items-start q-px-md q-mt-sm full-width">
          <div class="text-caption text-grey-5">
            Images designed by <a href="https://www.freepik.com" target="_blank" class="text-secondary">Freepik</a>
          </div>
          <div class="text-caption text-grey-5 text-right">
            © 2025 <a href="https://www.skysail.io" target="_blank" class="text-secondary">Skysail Consulting GmbH </a>.
            All rights reserved.
          </div>
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
        <q-btn flat label="Close" color="primary" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>

  <!-- Disclaimer Dialog -->
  <q-dialog v-model="showDisclaimer" persistent full-width transition-show="fade" transition-hide="fade">
    <q-card class="q-pa-md" style="max-width: 800px; width: 90vw; max-height: 90vh;">
      <q-card-section class="row items-center justify-between">
        <div class="text-h6">Disclaimer
        </div>
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>

      <q-separator />

      <q-card-section class="scroll" style="max-height: 70vh;">
        <div>
          <p><strong>Legal disclaimer</strong></p>
          <p>The contents of these pages were prepared with utmost care. Nonetheless, we cannot assume liability for the
            timeless accuracy, correctness and completeness of the information.<br><br>

            Our website contains links to external websites. The content of these websites has not been investigated or
            analyzed by us, and we do not warrant the adequacy, accuracy, reliability or completeness of any information
            on hyperlinked or referenced websites and disclaim any liability for any and all of their content.
            Responsibility for the contents of the linked pages is always held by the provider or operator of the pages.
          </p>

          <p><strong>Copyright</strong></p>
          <p>The content and works created by the page operators and presented on these pages is governed by German
            copyright law. Reproduction, processing, dissemination and any type of use beyond what is permitted under
            copyright requires written authorisation from the respective author and/or the creator. Downloads and copies
            of these pages are only meant for private use and exclude any commercial use. In so far as the contents on
            this page were not created by the operator, third-party copyrights are respected. In particular, the content
            of third parties is identified as such. Should you nevertheless become aware of an infringement of
            copyright, we kindly request that you inform us. As soon as we become aware of breaches of law, we will
            immediately remove such contents.</p>

          <p><strong>Data Protection</strong></p>
          <p>In general, when visiting our website no personal data are saved. However, these data can be given on a
            voluntary basis (e.g. name or email address). No data will be passed on to third parties without your
            consent. We point out that in regard to unsecured data transmission in the internet (e.g. via email),
            security cannot be guaranteed. Such data could possibly be accessed by third parties. <br><br>

            The use of contact data published within the framework of the imprint regulations by third parties for
            sending non-requested advertising material and information is hereby explicitly prohibited. The site
            operators expressly reserve the right to take legal action if unsolicited promotional information such as
            spam emails are sent.</p>
        </div>
      </q-card-section>

      <q-separator />

      <q-card-actions align="right">
        <q-btn flat label="Close" color="secondary" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>

  <!-- Privacy Dialog -->
  <q-dialog v-model="showPrivacy" persistent full-width transition-show="fade" transition-hide="fade">
    <q-card class="q-pa-md" style="max-width: 800px; width: 90vw; max-height: 90vh;">
      <q-card-section class="row items-center justify-between">
        <div class="text-h6">Privacy
        </div>
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>

      <q-separator />

      <q-card-section class="scroll" style="max-height: 70vh;">
        <div>
          <p><strong>Privacy Notice</strong></p>
          <p>We take the protection of your personal data very seriously. In accordance with the European General Data
            Protection Regulation (GDPR), we are committed to ensuring that your personal data is collected, processed,
            and stored in a lawful, transparent, and secure manner.
          </p>

          <p><strong>Data Collection and Usage</strong></p>
          <p>We only collect personal data when it is necessary for the use of our services, such as when you register,
            use our features, or contact us. This data may include your name, email address, usage data, and technical
            information (e.g. browser type, device type, IP address).</p>

          <p><strong>Analytics and Cookies</strong></p>
          <p>We use third-party services such as Google Analytics to understand how visitors use our site and to
            improve your experience. These services may use cookies and similar technologies to collect anonymized usage
            data.<br>

            By using our website, you consent to the use of these tools. You can revoke or manage your consent at any
            time by adjusting your browser settings or using our cookie banner.
          </p>
          <p><strong>Data Security</strong></p>
          <p>Your data is stored securely and will never be shared with third parties without your explicit consent,
            unless required by law.
          </p>
          <p><strong>Your Rights</strong></p>
          <p>In accordance with GDPR, you have the right to:
          <ul>
            <li>Access your stored personal data</li>
            <li>Request correction or deletion</li>
            <li>Restrict or object to data processing</li>
            <li>Lodge a complaint with a data protection authority</li>
          </ul>

          If you have any questions or concerns about your personal data, feel free to contact us at
          <i>info@bibbly.cloud</i>.
          </p>
          <p>Last updated: July 2025</p>
        </div>
      </q-card-section>

      <q-separator />

      <q-card-actions align="right">
        <q-btn flat label="Close" color="secondary" v-close-popup />
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
const showDisclaimer = ref(false);
const showPrivacy = ref(false);
const browserName = ref('Browser');
const supportedBrowsers = ['Chrome', 'Firefox', 'Edge', 'Opera'];

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

function openDisclaimer() {
  showDisclaimer.value = true;
}

function openPrivacy() {
  showPrivacy.value = true;
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
  generateCaptcha();
  browserName.value = detectBrowser();
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

async function scrollToSection(sectionId: string) {
  if (route.name === 'welcome') {
    scrollToAnchor(sectionId)
  } else {
    await router.push({ name: 'welcome', hash: `#${sectionId}` })
  }
}

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function scrollToAnchor(sectionId: string) {
  setTimeout(() => {
    const el = document.getElementById(sectionId)
    if (el) {
      const offset = 80 // Header-Höhe ggf. dynamisch machen
      const top = el.getBoundingClientRect().top + window.pageYOffset - offset
      window.scrollTo({ top, behavior: 'smooth' })
    }
  }, 100)
}

function detectBrowser(): string {
  const ua = navigator.userAgent;

  if (/OPR\//.test(ua)) return 'Opera';
  if (/Edg\//.test(ua)) return 'Edge';
  if (/Chrome\//.test(ua) && !/Edg\//.test(ua) && !/OPR\//.test(ua)) return 'Chrome';
  if (/Firefox\//.test(ua)) return 'Firefox';
  if (/Safari/.test(ua) && !/Chrome\//.test(ua)) return 'Safari';
  return 'Browser';
}

const isSupportedBrowser = computed(() =>
  supportedBrowsers.includes(browserName.value)
);

const extensionLabel = computed(() =>
  isSupportedBrowser.value ? `Add to ${browserName.value}` : 'Get extension'
);

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
