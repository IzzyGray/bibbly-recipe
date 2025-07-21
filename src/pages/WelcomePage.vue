<template>
  <q-page class="welcome-bg column">
    <q-carousel v-model="slide" transition-prev="jump-right" transition-next="jump-left" swipeable animated
      control-color="white" prev-icon="arrow_left" next-icon="arrow_right" navigation-icon="radio_button_unchecked"
      navigation padding arrows height="500px" class="welcome-carousel">
      <q-carousel-slide name="style" class="column no-wrap flex-center">
        <q-icon name="style" size="56px" />
        <div class="q-mt-md text-center">
          text 1
        </div>
      </q-carousel-slide>
      <q-carousel-slide name="tv" class="column no-wrap flex-center">
        <q-icon name="live_tv" size="56px" />
        <div class="q-mt-md text-center">
          text 2
        </div>
      </q-carousel-slide>
      <q-carousel-slide name="layers" class="column no-wrap flex-center">
        <q-icon name="layers" size="56px" />
        <div class="q-mt-md text-center">
          text 3
        </div>
      </q-carousel-slide>
      <q-carousel-slide name="map" class="column no-wrap flex-center" img-src="../assets/images/pic5.jpg">
        <q-icon name="restaurant_menu" size="64px" color="teal" />
        <div class="q-mt-md text-center">
          <div class="text-h4 q-mt-md app-card-title-welcome">Welcome to bibbly's recipe collection</div>
        </div>
        <div class="text-subtitle1 text-grey-7 q-mt-sm text-center">
          Bibbly extracts and organizes your recipe links. <br> It makes them searchable
          and lets you add your own flavor to them!
        </div>
        <div class="text-body1 q-mt-md">Try it now</div>
        <q-form @submit.prevent="submitUrl" class="q-mt-sm">
          <q-input v-model="demoUrl" label="Enter recipe URL" dense filled bg-color="white" type="url"
            :rules="[isValidUrl]" class="q-mt-sm">
            <template #append>
              <q-btn flat icon="send" color="teal" @click="submitUrl" />
            </template>
          </q-input>
        </q-form>
      </q-carousel-slide>
    </q-carousel>

    <!-- Description Section -->
    <div class="q-pa-xl bg-grey-1">
      <div class="text-center q-mb-md">
        <div class="app-desc-welcome">Organize your bookmarks</div>
        <div class="text-subtitle1 text-grey-7 q-mt-sm">
          Just add a recipe link to your collection. Bibbly extracts
          all the details, presents them in a clean, unified format. <br>Whether you’re searching by ingredients,
          category,
          or
          cooking time, your personal recipe collection is just a tap away.<br> And best of all: share your Bibbly
          collection with friends and family – cooking inspiration has never been this easy!
        </div>
      </div>
    </div>

    <footer class="q-pa-sm q-px-lg q-mt-lg bg-grey-1 text-grey-9">
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
          © Skysail Consulting. All rights reserved.
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
        <q-btn flat label="Schließen" color="primary" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>

</template>


<script setup lang="ts">

import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { Notify } from 'quasar';



const router = useRouter();
const demoUrl = ref('');
const newsletterEmail = ref('');
const showImpressum = ref(false);




const products = ['Recipe Editor', 'Shopping List', 'Meal Planner']
const resources = ['Help Center', 'Blog', 'Video Tutorials', 'Community']
const company = ['About Us', 'Legal Notice', 'Contact', 'Press']

const slide = ref('style') // oder 'tv', 'layers', 'map'


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

function openImpressum() {
  showImpressum.value = true;
}


</script>

<style scoped lang="scss">
.welcome-page {
  background-color: #91B494;
  background-image: url('/src/assets/images/green-grid.svg');
  background-repeat: repeat;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 70vh;
  padding: 1rem;
}

.welcome-carousel {
  background-color: #91B494;
  background-image: url('/src/assets/images/green-grid.svg');
  background-repeat: repeat;

}

.background-wrapper {
  position: relative;
  width: 100%;
  max-width: 1200px;
  height: 500px;
  overflow: hidden;

}

.background-image {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
  top: 0;
  left: 0;
  z-index: 1;
}

.content-card {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: rgba(255, 255, 255, 0);
  padding: 2rem 3rem;
  border-radius: 0;
  text-align: center;
  z-index: 2;
  width: 80%;
  max-width: 600px;
  box-shadow: none;
}

.title {
  font-size: 2.5rem;
  font-weight: 800;
  margin: 0 0 1rem;
}

.subtitle {
  font-size: 1rem;
  color: #333;
  margin-bottom: 1.5rem;
}
</style>
