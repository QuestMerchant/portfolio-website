<script setup>
import { ref } from 'vue'

const web3formsAccessKey = "322a23fa-ce9a-4fe1-a929-914ece085e1b"
const name = ref("")
const email = ref("")
const phone = ref("")
const message = ref("")
const button = ref("submit")
const isDisabled = ref(false)

const submitForm = async () => {
  const resp = await fetch("https://api.web3forms.com/submit", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Accept: "application/json",
    },
    body: JSON.stringify({
      access_key: web3formsAccessKey,
      name: name.value,
      email: email.value,
      phone: phone.value,
      message: message.value,
    }),
  })
  const result = await resp.json()
  if (result.success) {
    button.value = "sent!"
    isDisabled.value = true
  }
}
</script>

<template>
  <div class="bg-primary-subtle">
    <div class="container py-4 py-sm-5">
      <!-- Title -->
      <div class="text-xs-center mb-4">
        <p class="letter-spacing--2 mb-3">Feedback</p>
        <h2 class="fs-2">Send me a message</h2>
      </div>
      <form @submit.prevent="submitForm">
        <div class="row mb-2">
          <!-- Details -->
          <div class="col-sm-6 mb-2 mb-md-4">
            <div class="mb-2">
              <input type="text" v-model="name" class="form-control form-input radius--50" placeholder="* Name" required>
            </div>
            <div class="mb-2">
            <input type="email" v-model="email" class="form-control form-input radius--50" placeholder="* Email" required>
            </div>
            <input type="text" v-model="phone" class="form-control form-input radius--50" placeholder="* Phone">
          </div>
          <!-- Message Box -->
          <div class="col-sm-6">
            <textarea class="form-control form-input radius--10 py-2" v-model="message" rows="8" placeholder="* Your Message" required></textarea>
          </div>
        </div>
        <div class="text-xs-center">
          <button type="submit" :class="{ disabled: isDisabled }" class="text-uppercase btn btn-primary btn-sm px-5 py-2 radius--50 mb-3">{{ button }}</button>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped lang="scss">
.form-input {
  min-height: 3.5rem;
  font-size: 0.875rem;
  font-weight: 300;
  border: none;
  box-shadow: none;
  letter-spacing: .1rem;
  padding: .625rem 1.25rem;
  transition-duration: 300ms;
  transition-property: all;
  transition-timing-function: cubic-bezier(0.7, 1, 0.7, 1);

  &:focus {
    font-weight: 400;
    box-shadow: none;
    transition-duration: 300ms;
    transition-property: all;
    transition-timing-function: cubic-bezier(0.7, 1, 0.7, 1);
  }
}
</style>