<script setup lang="ts">
import { ref, type Ref } from 'vue'
import ListItemComponent from './ListItemComponent.vue'

type Todo = { title: string; id: number; body: string; done: boolean }

const list: Ref<Todo[]> = ref([])
const status: Ref<'done' | 'todo' | 'all'> = ref('all')
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

console.error(error)
getPosts()

function filterDone() {
  status.value = 'done'
}
function removeFilter() {
  status.value = 'all'
}
function filterTodo() {
  status.value = 'todo'
}
</script>

<template>
  <section class="btn-group">
    <button @click="removeFilter">All</button>
    <button @click="filterDone">Done</button>
    <button @click="filterTodo">To Do</button>
  </section>
  <div
    v-for="item in list.filter((i) =>
      status === 'all' ? i : status === 'done' ? i.done : !i.done
    )"
    :key="item.id"
  >
    <ListItemComponent :id="item.id" :done="item.done" @update-done="updateDone">
      <template #heading>{{ item.title }}</template>
      <template v-if="!item.done" #note>{{ item.body }}</template>
    </ListItemComponent>
  </div>
</template>

<style>
.btn-group button {
  background-color: hsla(160, 100%, 37%, 1);
  border: 1px solid hsla(160, 100%, 37%, 1);
  color: white;
  padding: 10px 24px;
  cursor: pointer;
}

.btn-group button:not(:last-child) {
  border-right: none;
}

/* Add a background color on hover */
.btn-group button:hover {
  background-color: hsla(160, 100%, 37%, 0.2);
}
</style>
