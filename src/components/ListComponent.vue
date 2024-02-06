<script setup lang="ts">
import { ref, type Ref } from 'vue'
import ListItemComponent from './ListItemComponent.vue'

const list: Ref<{ title: string; id: number; body: string }[]> = ref([])
const error = ref(null)

async function getPosts() {
  return await fetch('https://jsonplaceholder.typicode.com/posts')
    .then((response) => response.json())
    .then((json) => (list.value = json))
    .catch((err) => (error.value = err))
}

console.error(error)
getPosts()
</script>

<template>
  <div v-for="item in list.map((i) => ({ ...i, done: Math.random() < 0.5 }))" :key="item.id">
    <ListItemComponent :id="item.id" :done="item.done">
      <template #heading>{{ item.title }}</template>
      <template #note>{{ item.body }}</template>
    </ListItemComponent>
  </div>
</template>
