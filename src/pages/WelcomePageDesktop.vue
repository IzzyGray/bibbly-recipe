<template>
  <q-page class="welcome-bg column">
    <div class="q-pa-xl bg-white">
      <div class="text-center q-mb-md q-mt-xl">
        <div class="app-desc-welcome">Bookmarks, But <span class="gradient-text">Better</span></div>
        <div class="app-desc-text-welcome q-mt-lg">
          The links you save are more than just URLs — they reflect your curiosity, your taste, and your intent.<br>
          Collected together, they form a curated mosaic of meaning — uniquely yours.<br>
          And when you share them with friends or family, they become a trusted shortcut to what’s worth seeing.
        </div>
        <div class="row justify-center q-gutter-sm q-mt-xl">
          <q-btn style="background: #698f3f; color: white" unelevated class="text-capitalize q-mr-sm"
            @click="goToRegister">
            <span style="font-size: 1rem;">
              Start free trial
            </span>
          </q-btn>

          <q-btn style="background: #191919; color: white" class="text-capitalize q-ml-sm">
            <span style="font-size: 1rem;">
              Learn more
            </span>
          </q-btn>
        </div>
      </div>
    </div>

    <!-- Carousel Section -->
    <div class="q-pa-md flex flex-center">
      <div style="max-width: 900px; width: 100%;">
        <q-carousel animated v-model="slide" navigation infinite :autoplay="autoplay" arrows
          transition-prev="slide-right" transition-next="slide-left" swipeable control-color="teal" height="500px"
          class="welcome-carousel">
          <q-carousel-slide v-for="(item, index) in slides" :key="index" :name="item.name" :img-src="item.img"
            class="column no-wrap flex-center">
            <div class="slide-overlay" />
            <div class="carousel-overlay q-pa-md">
              <div class="q-mb-md icon-wrapper">
                <q-icon :name="item.icon" size="64px" color="teal" />
              </div>
              <div class="q-mt-md text-center">
                <div class="text-h4 q-mt-md app-card-title-welcome">{{ item.title }}</div>
              </div>
              <div class="text-subtitle1 text-grey-7 q-mt-sm text-center">
                {{ item.subtitle }}
              </div>
              <div class="text-body1 q-mt-md text-center">Try it now</div>
              <q-form @submit.prevent="submitUrl" class="q-mt-sm">
                <q-input v-model="demoUrl" :label="item.inputLabel" dense filled bg-color="white" type="url"
                  :rules="[isValidUrl]" class="q-mt-sm">
                  <template #append>
                    <q-btn flat icon="send" color="teal" @click="submitUrl" />
                  </template>
                </q-input>
              </q-form>
            </div>
          </q-carousel-slide>
        </q-carousel>
      </div>
    </div>


    <!-- Download Extension Section -->
    <div class="q-pa-xl bg-grey-1 text-center">
      <div class="app-ext-welcome q-mb-sm">
        Save Links with the bibbly Extension
      </div>

      <div class="app-ext-text-welcome q-mb-md">
        Install the bibbly browser extension to add links in just one click - from any website, at any time.<br />
        Everything you save is instantly added to your collection - no copy-paste, no hassle.
      </div>

      <q-btn label="Get the Extension" icon="extension" style="background: #698f3f; color: white" unelevated size="md"
        class="q-mt-sm text-capitalize" @click="goToExtension" />
    </div>


    <!-- Footer -->

    <footer class="q-pa-sm q-px-lg q-mt-lg bg-grey-1 text-grey-9">
      <div class="footer-content q-mb-md q-gutter-md" v-if="$q.screen.gt.md">
        <div class="row q-col-gutter-sm q-mb-md items-start justify-start flex-wrap">
          <!-- Logo & Links -->
          <div class="col-12 col-sm-6 col-md-3 q-mb-sm">

          </div>

          <!-- Products -->
          <div class="col-12 col-sm-6 col-md-2 q-mb-sm">
            <div class="text-subtitle2 midnight-green q-mb-xs">Products</div>
            <div class="column no-wrap">
              <div v-for="item in products" :key="item" class="text-body2 q-mb-xs">
                {{ item }}
              </div>
            </div>
          </div>

          <!-- Resources -->
          <div class="col-12 col-sm-6 col-md-2 q-mb-sm">
            <div class="text-subtitle2 midnight-green q-mb-xs">Resources</div>
            <div class="column no-wrap">
              <div v-for="item in resources" :key="item" class="text-body2 q-mb-xs">
                {{ item }}
              </div>
            </div>
          </div>

          <!-- Company -->
          <div class="col-12 col-sm-6 col-md-2 q-mb-sm">
            <div class="text-subtitle2 midnight-green q-mb-xs">Company</div>
            <div class="column no-wrap">
              <div v-for="item in company" :key="item" class="text-body2 q-mb-xs cursor-pointer"
                @click="item === 'Legal Notice' ? openImpressum() : null">
                {{ item }}
              </div>
            </div>
          </div>

          <!-- Newsletter -->
          <div class="col-12 col-md-3 q-mb-sm">
            <div class="text-subtitle2 midnight-green q-mb-xs">Subscribe</div>
            <div class="text-caption q-mb-xs">
              Get the latest news and articles to your inbox.
            </div>
            <q-input v-model="demoUrl" label="Deine E-Mail Adresse" dense filled type="url" :rules="[isValidEmail]"
              class="q-mt-sm">
              <template #append>
                <q-btn flat icon="arrow_forward" color="teal" @click="submitEmail" />
              </template>
            </q-input>
          </div>
        </div>

        <q-separator spaced />

        <!-- Footer Bottom -->
        <div class="row items-center justify-between q-mt-sm q-gutter-sm">
          <div class="col-12 col-sm-auto flex justify-center justify-sm-end">
            <div class="q-gutter-xs">
              <q-btn flat round icon="language" size="xs" />
              <q-btn flat round icon="code" size="xs" />
              <q-btn flat round icon="facebook" size="xs" />
            </div>
          </div>
          <div class="col-12 col-sm-auto text-caption text-center text-sm-left">
            © 2025 Skysail Consulting GmbH. All rights reserved.
          </div>

        </div>
      </div>



      <!-- Mobile/Tablet Footer Menu -->
      <div class="q-mb-md" v-else>
        <!-- Linkzeile -->
        <div class="row items-center justify-center q-gutter-md text-caption q-mt-sm">
          <div class="cursor-pointer">Contact</div>
          <div class="cursor-pointer" @click="openImpressum()">Legal Notice</div>
          <div class="cursor-pointer">Disclaimer</div>
          <div class="cursor-pointer">Privacy</div>

        </div>

        <!-- Trennlinie -->
        <q-separator spaced />

        <!-- Copyright -->
        <div class="text-caption text-center q-mt-xs">
          © Skysail Consulting GmbH. All rights reserved.
        </div>
      </div>


    </footer>

  </q-page>

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
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { Notify } from 'quasar';
import { useRouter } from 'vue-router';
import Pic30 from 'src/assets/images/pic30.jpg'
import Pic20 from 'src/assets/images/pic20.jpg'
import Pic10 from 'src/assets/images/pic10.jpg'
import Pic7 from 'src/assets/images/pic7.png'

const router = useRouter();
const demoUrl = ref('');
const newsletterEmail = ref('');
const showImpressum = ref(false);

const products = ['Trip Planner', 'News Feed', 'Recipe Book', 'Dining Guide']
const resources = ['Help Center', 'Video Tutorials']
const company = ['About Us', 'Legal Notice', 'Privacy', 'Contact']

const slide = ref('style')
const autoplay = ref(8000)

const slides = [
  {
    name: 'style',
    img: Pic30,
    icon: 'restaurant_menu',
    title: 'Welcome to your Dining Guide',
    subtitle: 'Turn links into smart, searchable, and personalized collections – plated to your cravings.',
    inputLabel: 'Enter a restaurant URL'
  },
  {
    name: 'tv',
    img: Pic20,
    icon: 'beach_access',
    title: 'Welcome to your Trip Planner',
    subtitle: 'Turn links into smart, searchable, and personalized collections – mapped to your next adventure.',
    inputLabel: 'Enter a travel destination URL'
  },
  {
    name: 'layers',
    img: Pic10,
    icon: 'radio',
    title: 'Welcome to your News Feed',
    subtitle: 'Turn links into smart, searchable, and personalized collections – filtered to your interests.',
    inputLabel: 'Enter a news site URL'
  },
  {
    name: 'map',
    img: Pic7,
    icon: 'outdoor_grill',
    title: 'Welcome to your Recipe Book',
    subtitle: 'Turn links into smart, searchable, and personalized collections – seasoned to your taste.',
    inputLabel: 'Enter a recipe URL'
  }
]

async function goToRegister() {
  await router.push('/register')
}

function openImpressum() {
  showImpressum.value = true;
}

function isValidUrl(val: string) {
  const pattern = new RegExp(
    '^(https?:\\/\\/)?' + // optional http(s)
    '((([a-z\\d]([a-z\\d-]*[a-z\\d])*).)+[a-z]{2,}|' + // domain
    '((\\d{1,3}\\.){3}\\d{1,3}))' + // OR IPv4
    '(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*' + // port/path
    '(\\?[;&a-z\\d%_.~+=-]*)?' + // query string
    '(\\#[-a-z\\d_]*)?$', 'i'
  );
  return pattern.test(val) || 'Bitte eine gültige URL eingeben';
}

function submitUrl() {
  if (!isValidUrl(demoUrl.value)) {
    Notify.create({
      type: 'negative',
      message: 'Bitte gib eine gültige URL ein',
    });
    return;
  }
  // Beispiel: leite weiter zur Vorschau mit Query-Param
  void router.push({ name: 'preview', query: { url: demoUrl.value } });
}

function isValidEmail(email: string) {

  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email)
}

function submitEmail() {
  if (!isValidEmail(newsletterEmail.value)) {
    Notify.create({
      type: 'negative',
      message: 'Bitte gib eine gültige E-Mail Adresse ein',
    });
    return;
  }

  // Beispiel: leite weiter zur Bestätigung für die Newsletter-Anmeldung
  void router.push({ name: 'preview', query: { url: demoUrl.value } });
}

function goToExtension() {
  window.open('https://example.com/extension', '_blank') // ← hier den echten Link einfügen
}

</script>

<style scoped lang="scss">
.welcome-carousel {
  background-color: #91B494;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
}

.welcome-bg {
  background-color: white;
}

.welcome-carousel {
  background-color: #91B494;
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;


}

.carousel-overlay {
  background-color: rgba(255, 255, 255, 0.9);
  /* halbtransparent weiß */
  border-radius: 5px;
  padding: 2rem;
  max-width: 600px;
  margin: 0 auto;

}

.icon-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

.slide-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0, 100, 80, 0.2); // Farbe + Transparenz
  mix-blend-mode: multiply;
  z-index: 1;
  pointer-events: none;
}

.carousel-overlay {
  position: relative; // stellt sicher, dass es über dem Overlay liegt
  z-index: 2;
}
</style>
