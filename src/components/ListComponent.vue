<script setup lang="ts">
import { ref, type Ref } from 'vue'
import ListItemComponent from './ListItemComponent.vue'

type Todo = { title: string; id: number; body: string; done: boolean }

const list: Ref<Todo[]> = ref([])
const error = ref(null)

async function getPosts() {
  return await fetch('https://jsonplaceholder.typicode.com/posts')
    .then((response) => response.json())
    .then(
      (json) =>
        (list.value = json.map((i: Omit<Todo, 'done'>) => ({ ...i, done: Math.random() < 0.5 })))
    )
    .catch((err) => (error.value = err))
}

function updateDone(id: number) {
  const todo = list.value.find((i) => i.id === id)

  if (!todo) throw new Error('No todo found')

  const wasDone = todo.done

  todo.done = !wasDone
}

console.log('running')

console.error(error)
getPosts()
</script>

<template>
  <div v-for="item in list" :key="item.id">
    <ListItemComponent :id="item.id" :done="item.done" @update-done="updateDone">
      <template #heading>{{ item.title }}</template>
      <template #note>{{ item.body }}</template>
    </ListItemComponent>
  </div>
</template>
