<script setup lang="ts">
import TheWelcome from './components/TheWelcome.vue'
import TitleComponent from './components/TitleComponent.vue'
</script>

<template>
  <header>
    <TitleComponent msg="Todo App" />
  </header>

  <main>
    <TheWelcome />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}
</style>
