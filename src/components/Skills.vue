<script setup>
import Card from './Card.vue'
const card1 = 'https://images.unsplash.com/photo-1479660656269-197ebb83b540?dpr=2&auto=compress,format&fit=crop&w=1199&h=798&q=80&cs=tinysrgb&crop='
const card2 = 'https://plus.unsplash.com/premium_photo-1675433344518-21eb72dfc7a5?w=500&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8NXx8bmF0dXJlfGVufDB8fDB8fHww'
const card3 = 'https://images.unsplash.com/photo-1435224654926-ecc9f7fa028c?q=80&w=1974&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
</script>
<template>
  <div class=container>
    <Card :img="`url(${card1})`">
      <template #header>
        Problem-solving
      </template>
      <template #content>
        Whether it's fixing a frontend bug or researching and creating algorithms for the backend.
      </template>
    </Card>
    <Card :img="`url(${card2})`">
      <template #header>
        API integration
      </template>
      <template #content>
        Experience with Auth0, Google Firebase and a few other smaller ones.
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
<style scoped lang="scss">
.container {
  display: flex;
  margin-top: 1rem;
  margin-bottom: 1rem;
}
</style>