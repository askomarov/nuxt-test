<script setup>
const props = defineProps({
  isPlaying: Boolean,
  size: Number,
});
</script>

<template>
  <button type="button">
    <SvgPause
      :size="props.size"
      v-if="props.isPlaying"
    >
    </SvgPause>
    <SvgPlay
      :size="props.size"
      v-else
    />
  </button>
</template>
