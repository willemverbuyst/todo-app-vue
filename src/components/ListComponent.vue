<script setup lang="ts">
import { ref, type Ref } from 'vue'
import ListItemComponent from './ListItemComponent.vue'

const list: Ref<{ title: string; id: string; body: string }[]> = ref([])
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
  <div v-for="item in list" :key="item.id">
    <ListItemComponent>
      <template #icon>
        {{ item.id }}
      </template>
      <template #heading>{{ item.title }}</template>
      {{ item.body }}
    </ListItemComponent>
  </div>
</template>
