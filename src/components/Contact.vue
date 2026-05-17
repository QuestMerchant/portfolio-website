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
   <div class="bg-linear-to-b from-gray-50 to-gray-100 shadow-sm dark:from-gray-900 dark:to-gray-800 dark:text-gray-200 py-4 sm:py-5">
     <div class="container px-4">
       <!-- Title -->
       <div class="text-center mb-4">
         <p class="letter-spacing--2 mb-3">Feedback</p>
         <h2 class="fs-2">Send me a message</h2>
       </div>
       <form @submit.prevent="submitForm">
         <div class="flex flex-wrap mx-4 mb-2 ">
           <!-- Details -->
           <div class="w-full sm:w-1/2 px-4 mb-2 md:mb-4">
             <div class="mb-2">
               <input type="text" v-model="name" class="w-full h-14 text-sm font-light border border-gray-300 bg-white text-gray-900 placeholder-gray-500 tracking-wide px-5 py-2.5 rounded-full transition-all duration-300 ease-in-out focus:outline-none focus:border-blue-500 focus:ring-4 focus:ring-blue-100 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-200 dark:placeholder-gray-400" placeholder="* Name" required>
             </div>
             <div class="mb-2">
               <input type="email" v-model="email" class="w-full h-14 text-sm font-light border border-gray-300 bg-white text-gray-900 placeholder-gray-500 tracking-wide px-5 py-2.5 rounded-full transition-all duration-300 ease-in-out focus:outline-none focus:border-blue-500 focus:ring-4 focus:ring-blue-100 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-200 dark:placeholder-gray-400" placeholder="* Email" required>
             </div>
             <input type="text" v-model="phone" class="w-full h-14 text-sm font-light border border-gray-300 bg-white text-gray-900 placeholder-gray-500 tracking-wide px-5 py-2.5 rounded-full transition-all duration-300 ease-in-out focus:outline-none focus:border-blue-500 focus:ring-4 focus:ring-blue-100 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-200 dark:placeholder-gray-400" placeholder="* Phone">
           </div>
           <!-- Message Box -->
           <div class="w-full sm:w-1/2 px-4">
             <textarea class="w-full text-sm font-light border border-gray-300 bg-white text-gray-900 placeholder-gray-500 tracking-wide px-5 py-4 rounded-lg transition-all duration-300 ease-in-out focus:outline-none focus:border-blue-500 focus:ring-4 focus:ring-blue-100 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-200 dark:placeholder-gray-400" v-model="message" rows="8" placeholder="* Your Message" required></textarea>
           </div>
         </div>
         <div class="text-center">
           <button type="submit" :class="{ 'opacity-50': isDisabled }" class="inline-flex items-center justify-center bg-blue-500 hover:bg-blue-600 text-white text-sm font-medium py-2 px-5 rounded-full uppercase transition-colors duration-200 mb-3 disabled:opacity-50 disabled:cursor-not-allowed">{{ button }}</button>
         </div>
       </form>
     </div>
   </div>
</template>


