<template>
  <q-page class="flex flex-center bg-grey-1">
    <q-card class="q-pa-lg shadow-2" style="width: 400px; max-width: 90vw;">
      <q-card-section class="text-center">
        <div class="text-h6 text-primary">Sign up</div>
        <div class="text-subtitle2 text-grey-7">Create a new account</div>
      </q-card-section>

      <q-form @submit="onRegister" class="q-gutter-md q-mt-md">
        <q-input v-model="name" label="Name" outlined dense :rules="[val => !!val || 'Name is mandatory']" />
        <q-input v-model="email" label="Email" type="email" outlined dense
          :rules="[val => !!val || 'Email is mandatory']" />
        <q-input v-model="password" label="Password" type="password" outlined dense
          :rules="[val => val.length >= 4 || 'At least 4 characters']" />
        <q-input v-model="confirmPassword" label="Confirm password" type="password" outlined dense
          :rules="[val => val === password || 'Passwords do not match']" />

        <q-btn label="Sign Up" type="submit" color="primary" class="full-width" icon="person_add" />
      </q-form>

      <q-card-actions align="center" class="q-mt-md">
        <q-btn flat label="Already have an account?" to="/login" color="secondary" />
      </q-card-actions>
    </q-card>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useQuasar } from 'quasar';
import { useRouter } from 'vue-router';

const name = ref('');
const email = ref('');
const password = ref('');
const confirmPassword = ref('');
const $q = useQuasar();
const router = useRouter();

function onRegister() {
  if (!name.value || !email.value || !password.value || password.value !== confirmPassword.value) {
    $q.notify({ type: 'negative', message: 'Please check your input.' });
    return;
  }

  // Dummy-Registrierung – später durch Backend ersetzen
  $q.notify({ type: 'positive', message: 'Sign-up successful' });
  void router.push('/login');
}
</script>
