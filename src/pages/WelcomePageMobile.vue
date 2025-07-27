<template>
  <q-page class="welcome-bg column">

    <!-- Intro Section -->
    <section id="intro" class="q-pa-xl bg-white">
      <div class="text-center q-mb-md q-mt-xl">
        <div class="app-desc-welcome">
          Bookmarks, But <span class="gradient-text">Better</span>
        </div>
        <div class="app-desc-text-welcome q-mt-lg">
          The links you save are more than just URLs – they reflect your curiosity, your taste, and your intent.<br>
          Collected together, they form a curated mosaic of meaning – uniquely yours.<br>
          And when you share them with friends or family, they become a trusted shortcut to what’s worth seeing.
        </div>
        <div class="row justify-center q-gutter-sm q-mt-xl">
          <q-btn color="secondary" unelevated class="text-capitalize q-mr-sm" @click="goToRegister">
            <span style="font-size: 1rem;">Start free trial</span>
          </q-btn>

          <q-btn style="background: #191919; color: white" class="text-capitalize q-ml-sm">
            <span style="font-size: 1rem;">Learn more</span>
          </q-btn>
        </div>
      </div>
    </section>

    <!-- Product Section -->
    <section id="product" class="q-pa-xl text-center">
      <div class="app-ext-welcome q-mb-sm">
        How Bibbly works
      </div>

      <div class="text-subtitle2 text-grey-7 q-mb-md">
        Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore pariatur. Excepteur sint occaecat
        cupidatat non proident, sunt in culpa.
      </div>
    </section>

    <!-- Cards Section (instead Carousel) with Collections -->
    <section id="collections" class="q-pa-sm q-pa-md-md flex flex-center">
      <div class="text-center q-mb-lg">
        <div class="app-ext-welcome">
          Explore Your Collections
        </div>
        <div class="text-subtitle2 text-grey-7 q-mt-xs">
          Choose from a variety of smart link collections tailored to your interests - and try them instantly.
        </div>
      </div>

      <q-carousel v-model="activeSlide" animated swipeable navigation padding control-color="secondary" height="auto"
        arrows>
        <q-carousel-slide v-for="(item, index) in slides" :key="index" :name="index" class="q-pa-sm">
          <q-card flat bordered class="q-pa-md">
            <q-img :src="item.img" />
            <div class="column items-center">
              <div class="app-card-title-welcome q-mt-md">{{ item.title }}</div>
              <div class="text-subtitle2 text-grey-7 text-center q-mt-sm">
                {{ item.subtitle }}
              </div>
              <q-form @submit.prevent="submitUrl" class="full-width q-mt-md">
                <q-input v-model="demoUrl" :label="item.inputLabel" dense filled bg-color="grey-2" type="url"
                  :rules="[isValidUrl]">
                  <template #append>
                    <q-btn flat icon="send" color="secondary" @click="submitUrl" />
                  </template>
                </q-input>
              </q-form>
            </div>
          </q-card>
        </q-carousel-slide>
      </q-carousel>
    </section>


    <!-- Download Extension Section -->
    <section id="extension" class="q-pa-xl bg-grey-1 text-center">
      <div class="app-ext-welcome q-mb-sm">
        Save Links with the bibbly Extension
      </div>

      <div class="text-subtitle2 text-grey-7 q-mb-md">
        Install the bibbly browser extension to add links in just one click – from any website, at any time.<br />
        Everything you save is instantly added to your collection – no copy-paste, no hassle.
      </div>

      <q-btn label="Get the Extension" icon="extension" color="secondary" unelevated size="md"
        class="q-mt-sm text-capitalize" @click="goToExtension" />
    </section>

    <!-- About Section -->
    <section id="about" class="q-pa-xl text-center">
      <div class="app-ext-welcome q-mb-sm">
        About
      </div>

      <div class="text-subtitle2 text-grey-7 q-mb-md">
        We believe that small, clever tools can simplify your life and give you the time to focus on what truly
        matters.<br>
        As IT professionals, we spend a lot of time at our computers, often with
        numerous tabs open,
        gathering all kinds of information. <br>
        Our mission is to keep this information organized and easily
        retrievable.<br>
        That's where Bibbly comes in –
        designed to help us, and hopefully, to help you too.
      </div>
    </section>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { Notify } from 'quasar'
import Pic30 from 'src/assets/images/pic30.jpg'
import Pic20 from 'src/assets/images/pic20.jpg'
import Pic10 from 'src/assets/images/pic10.jpg'
import Pic7 from 'src/assets/images/pic7.png'

const router = useRouter()
const demoUrl = ref('')
const activeSlide = ref(0) // für v-model im q-carousel

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

function isValidUrl(val: string) {
  const pattern = new RegExp(
    '^(https?:\\/\\/)?' + // optional http(s)
    '((([a-z\\d]([a-z\\d-]*[a-z\\d])*).)+[a-z]{2,}|' + // domain
    '((\\d{1,3}\\.){3}\\d{1,3}))' + // OR IPv4
    '(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*' + // port/path
    '(\\?[;&a-z\\d%_.~+=-]*)?' + // query string
    '(\\#[-a-z\\d_]*)?$', 'i'
  )
  return pattern.test(val) || 'Bitte eine gültige URL eingeben'
}

function submitUrl() {
  if (!isValidUrl(demoUrl.value)) {
    Notify.create({
      type: 'negative',
      message: 'Bitte gib eine gültige URL ein',
    })
    return
  }
  void router.push({ name: 'preview', query: { url: demoUrl.value } })
}

async function goToRegister() {
  await router.push('/register')
}

function goToExtension() {
  window.open('https://example.com/extension', '_blank')
}
</script>

<style scoped>
.welcome-bg {
  background-color: white;
}
</style>
