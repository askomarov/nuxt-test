<template>
  <button
    v-bind="$attrs"
    type="button"
    @click="onClick(msg)"
  >
    Click Me Child
  </button>
</template>

<script setup>
const props = defineProps({
  msg: {
    type: String,
    default: "Default text Artem Hello!",
  },
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
}
.green {
  background-color: green;
}
</style>
