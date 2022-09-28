<template>
  <button
    type="button"
    @click="onClick(msg)"
  >
    TS Click Me Child
  </button>
</template>

<script setup lang="ts">
interface props {
  msg?: string;
}
const props = withDefaults(defineProps<props>(), {
  msg: "Default message text",
});
const emits = defineEmits(["writeInConsole"]);

function onClick(msg) {
  emits("writeInConsole", msg);
}
</script>

<style scoped>
button {
  display: block;
  width: auto;
  padding: 0.5rem;
  background: #c4c;
}
</style>
