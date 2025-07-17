<template>
  <div class="row fullscreen">
    <!-- Bild / Branding links -->
    <div class="col-12 col-md-6 bg-image flex flex-center text-white column q-pa-xl">
      <div class="text-h4 q-mb-md">Willkommen bei</div>
      <div class="text-h2 text-weight-bold">Meine Rezept-App</div>
    </div>

    <!-- Loginformular rechts -->
    <div class="col-12 col-md-6 flex flex-center bg-white q-pa-xl">
      <q-card class="q-pa-xl" style="width: 100%; max-width: 400px;">
        <q-card-section class="text-center">
          <div class="text-h5 text-weight-medium">Login</div>
          <div class="text-subtitle2 text-grey-7 q-mt-xs">Melde dich an, um fortzufahren</div>
        </q-card-section>

        <q-card-section>
          <q-input v-model="email" label="E-Mail" type="email" filled class="q-mb-md" />
          <q-input v-model="password" label="Passwort" type="password" filled class="q-mb-md" />
          <q-btn label="Einloggen" color="primary" class="full-width q-mt-md" @click="login" />
        </q-card-section>

        <q-card-actions vertical align="center">
          <q-btn flat label="Noch keinen Account? Registrieren" color="primary" @click="goToRegister" />
        </q-card-actions>
      </q-card>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const email = ref('')
const password = ref('')

function login() {
  // Fake Login für Demo
  if (email.value && password.value) {
    void router.push('/home')
  }
}

function goToRegister() {
  void router.push('/register')
}
</script>

<style scoped lang="scss">
.bg-image {
  background-image: url('https://images.unsplash.com/photo-1504674900247-0877df9cc836?auto=format&fit=crop&w=1050&q=80');
  background-size: cover;
  background-position: center;
}
</style>
