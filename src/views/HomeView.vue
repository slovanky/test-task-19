<script setup>
import { ref } from "vue"

import passwordMeter from "vue-simple-password-meter";

const userEmail = ref('kminchelle')
const userPassword = ref('0lelplR')

const loginResponse = ref('')

const loginIsLoading = ref(false)

const onSubmitLogin = () => {
  fetch('https://dummyjson.com/auth/login', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      username: userEmail.value,
      password: userPassword.value,
    })
  })
    .then(res => res.json())
    .then((res) => {
      console.log(res);

      loginResponse.value = res
    }).catch(err => console.log(err));
}

</script>

<template>
  <main>
    <section class="h-screen flex items-center justify-center py-12 bg-slate-100">
      <div class="w-full max-w-md bg-white rounded-lg shadow">
        <div class="p-6 space-y-4">
          <div class="form-group">
            <label for="email">Email</label>
            <input v-model="userEmail" type="text" class="form-control bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg w-full p-2.5" id="email">
          </div>

          <div class="form-group">
            <label for="password">Password</label>
            <input v-model="userPassword" type="password" class="form-control bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg w-full p-2.5" id="password">
          </div>

          <passwordMeter :password="userPassword" />

          <button @click="onSubmitLogin" type="button" id="login"
            class="w-full text-white bg-blue-600 hover:bg-blue-700 focus:ring-0 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center">Login</button>
          <div v-if="loginResponse" id="response">
            <p v-for="(value, key) in loginResponse" :key="key" class=" overflow-hidden">
              {{ key }} : <span class="font-bold">{{ value }}</span>
            </p>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<style>
.po-password-strength-bar {
  border-radius: 2px;
  transition: all 0.2s linear;
  height: 5px;
  margin-top: 8px;
}

.po-password-strength-bar.risky {
  background-color: #f95e68;
}

.po-password-strength-bar.guessable {
  background-color: #fb964d;
}

.po-password-strength-bar.weak {
  background-color: #fdd244;
}

.po-password-strength-bar.safe {
  background-color: #b0dc53;
}

.po-password-strength-bar.secure {
  background-color: #35cc62;
}
</style>
