<script setup>
import { ref, computed } from 'vue'
import { useAuthStore } from '@/stores/auth.js'
import { useI18n } from 'vue-i18n'

const password = ref('')
const passwordAgain = ref('')
const email = ref('')
const textAlert = ref("")

const store = useAuthStore()

const { t } = useI18n()

const placeholderText = computed(() => t('Password'));
const placeholderText2 = computed(() => t('confirmPassword'));


async function register() {
  if (password.value !== '' && email.value !== '') {
    if (password.value === passwordAgain.value) {
      try {
        const response = await store.register(email.value, password.value);

        if (response.id) {
          store.user.id = response['id']
          store.user.email = email.value
          store.user.role = response['role']
          store.user.isAuthenticated = true
          store.user.access_token = response['access_token']
          store.user.refresh_token = response['refresh_token']

          localStorage.setItem('id', response['id'])
          localStorage.setItem('email', email.value)
          localStorage.setItem('role', response['role'])
          localStorage.setItem('isAuthenticated', "true")
          localStorage.setItem('access_token', response['access_token'])
          localStorage.setItem('refresh_token', response['refresh_token'])

          textAlert.value = '';
          password.value = '';
          passwordAgain.value = '';
          email.value = '';
        } else {
          textAlert.value = t("alert4");
        }
      } catch (error) {
        textAlert.value = t("alert5");
      }
    } else {
      textAlert.value = t("alert6");
    }
  } else {
    textAlert.value = t("alert3");
  }
}
</script>
<template>
  <div class="flex">
    <div
      class="relative flex h-auto max-w-md flex-1 flex-col justify-center px-6 py-12 lg:px-8 bg-white border-2 rounded-xl border-gray-300/30 shadow-md">
      <div class="sm:mx-auto sm:w-full sm:max-w-sm">
        <img class="mx-auto h-15 w-auto" src="@/assets/img/logos/BlueLogo.svg" alt="Your Company" />
      </div>

      <div class="mt-10 sm:mx-auto sm:w-full sm:max-w-sm">
        <form class="space-y-6" @submit.prevent="register">
          <div>
            <div class="mt-2">
              <input v-model="email" id="email" name="email" type="email" autocomplete="email" placeholder="Email"
                class="block w-full rounded-md border-0 py-2 px-2 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:outline-blueFunko-500 focus:ring-blueFunko-300 sm:text-sm sm:leading-6" />
            </div>
          </div>

          <div>
            <div class="mt-2">
              <input v-model="password" id="password" name="password" type="password" autocomplete="current-password"
                :placeholder="placeholderText"
                class="block w-full rounded-md border-0 py-2 px-2 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:outline-blueFunko-500 focus:ring-blueFunko-300 sm:text-sm sm:leading-6" />
            </div>
          </div>

          <div>
            <div class="mt-2">
              <input v-model="passwordAgain" id="confirmPassword" name="confirmPassword" type="password"
                autocomplete="current-password" :placeholder="placeholderText2"
                class="block w-full rounded-md border-0 py-2 px-2 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:outline-blueFunko-500 focus:ring-blueFunko-300 sm:text-sm sm:leading-6" />
            </div>
          </div>

          <div v-if="textAlert != ''"
            class="mt-4 font-regular relative block w-full rounded-lg bg-pink-500 p-4 text-base leading-5 text-white opacity-100"
            data-dismissible="alert">
            <div class="mr-12">{{ textAlert }}</div>
          </div>

          <div>
            <button type="submit"
              class="flex w-full justify-center rounded-md bg-blueFunko-700 px-3 py-2 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-blueFunko-800 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-blueFunko-800 easy-in-out duration-150 hover:scale-105 hover:delay-150">
              {{ t('Sign up') }}
            </button>
          </div>
        </form>

      </div>
      <img src="../assets/img/details/PointShape.svg" alt="" class="w-10 absolute bottom-1 left-1" />
      <img src="../assets/img/details/PointShape.svg" alt="" class="w-10 absolute top-1 right-1" />
    </div>
  </div>
</template>

<style>
body {
  background-color: #f7f8fa;
}
</style>
