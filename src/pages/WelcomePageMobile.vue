<template>
  <q-page class="welcome-bg column">
    <q-carousel animated v-model="slide" navigation infinite :autoplay="autoplay" arrows transition-prev="slide-right"
      transition-next="slide-left" swipeable control-color="teal" height="500px" class="welcome-carousel">
      <q-carousel-slide v-for="(item, index) in slides" :key="index" :name="item.name" :img-src="item.img"
        class="column no-wrap flex-center" />
    </q-carousel>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue'

// ❗️Importiere Bilder korrekt
import Pic30 from 'src/assets/images/pic30.jpg'
import Pic20 from 'src/assets/images/pic20.jpg'
import Pic10 from 'src/assets/images/pic10.jpg'
import Pic7 from 'src/assets/images/pic7.png'

const slide = ref('style')
const autoplay = ref(6000)

const slides = [
  { name: 'style', img: Pic30 },
  { name: 'tv', img: Pic20 },
  { name: 'layers', img: Pic10 },
  { name: 'map', img: Pic7 },
]
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
</style>
