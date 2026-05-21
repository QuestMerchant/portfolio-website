<script setup>
import Card from './Card.vue'
const card1 = 'https://images.unsplash.com/photo-1479660656269-197ebb83b540?dpr=2&auto=compress,format&fit=crop&w=1199&h=798&q=80&cs=tinysrgb&crop='
const card2 = 'https://plus.unsplash.com/premium_photo-1675433344518-21eb72dfc7a5?w=500&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8NXx8bmF0dXJlfGVufDB8fDB8fHww'
const card3 = 'https://images.unsplash.com/photo-1435224654926-ecc9f7fa028c?q=80&w=1974&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
</script>
<template>
  <div class="flex justify-center bg-linear-to-b from-gray-50 to-gray-100 shadow-sm dark:from-gray-900 dark:to-gray-800 py-4 flex-wrap">
    <Card :img="`url(${card1})`">
      <template #header>
        Performance & Optimization
      </template>
      <template #content>
        Experienced in refactoring rendering logic, caching data, SQL optimization top reduce costs and loading times.
      </template>
    </Card>
    <Card :img="`url(${card2})`">
      <template #header>
        API architecture & Integration
      </template>
      <template #content>
        Experienced in designing secure, scalable RESTful API endpoints and handling webhooks. Track record of integrating third-party APIs such as Shopify, Paystack, Firebase and more.
      </template>
    </Card>
    <Card :img="`url(${card3})`">
      <template #header>
        Data Management & Modeling
      </template>
      <template #content>
        Proficient in designing relational database schemas and managing data flow using PostgreSQL. Writing clean server-side logic to read, process, and synchronize data across multiple external data sources.
      </template>
    </Card>
    <Card :img="`url(${card3})`">
      <template #header>
        Creativity
      </template>
      <template #content>
        Hopefully this is a good enough example.
      </template>
    </Card>
  </div>
</template>
