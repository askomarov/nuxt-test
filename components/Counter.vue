<template>
  <div class="wrap">
    <p>My Counter component</p>
    <span>Counter value: {{ counter }}</span>
  </div>
</template>

<script setup>
const props = defineProps({
  counter: {
    type: Number,
    default: 0,
  },
});
</script>

<style scoped>
.wrap {
  border: 1px solid #000;
  padding: 1rem;
  display: inline-grid;
  justify-content: center;
  text-align: center;
  gap: 1rem;
}
</style>
