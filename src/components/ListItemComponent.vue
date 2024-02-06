<script setup lang="ts">
defineProps<{
  id: number
  done: boolean
}>()
</script>

<template>
  <div class="item">
    <i :class="{ done: done }">
      {{ id }}
    </i>

    <div class="details">
      <h3>
        <slot name="heading"></slot>
      </h3>
      <slot name="note"></slot>
    </div>
  </div>
</template>

<style scoped>
.item {
  margin-top: 2rem;
  display: flex;
  position: relative;
}

.details {
  flex: 1;
  margin-left: 1rem;
}

i {
  display: flex;
  place-items: center;
  place-content: center;
  width: 32px;
  height: 32px;

  color: #111;
}

i.done {
  border: 5px solid var(--color-green);
}

h3 {
  font-size: 1.2rem;
  font-weight: 500;
  margin-bottom: 0.4rem;
  color: var(--color-heading);
}

@media (min-width: 1024px) {
  .item {
    margin-top: 0;
    padding: 0.4rem 0 1rem calc(var(--section-gap) / 2);
  }

  i {
    top: calc(50% - 25px);
    left: -26px;
    position: absolute;
    border: 1px solid var(--color-border);
    background: var(--color-red);
    border-radius: 8px;
    width: 50px;
    height: 50px;
  }

  i.done {
    background: var(--color-green);
  }
}
</style>
