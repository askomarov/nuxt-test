<script setup>
const props = defineProps({
  isPlaying: Boolean,
});
</script>

<template>
  <button type="button">
    <SvgPause v-if="props.isPlaying"> </SvgPause>
    <SvgPlay v-else />
  </button>
</template>
