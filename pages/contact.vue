<script setup>
import { useMouse } from "@vueuse/core";
const { x, y } = useMouse();
const pos = ref("");
const isAlien = ref(false);

const initCursor = () => {
  const render = () => {
    pos.value = `translate(${x.value}px, ${y.value}px)`;
    requestAnimationFrame(render);
  };
  requestAnimationFrame(render);
};

onMounted(() => {
  initCursor();
});
</script>

<template>
  <div class="container bg-dark-200">
    <div
      class="cursor-el"
      :class="{ isAlien: isAlien }"
      :style="{
        transform: pos,
        transition: 'transform 0.1s ease',
      }"
    ></div>
    <h1>hello</h1>
    <label for="alien">
      <b>Show Alien</b>
      <input type="checkbox" name="alien" id="alien" v-model="isAlien" />
    </label>
    <div>pos: {{ x }}, {{ y }}</div>
  </div>
</template>

<style lang="scss" scoped>
:deep(body),
:deep(body *) {
  cursor: none;
}
:deep(body) {
  --color-text: #fff;
  --color-bg: #171717;
  --color-link: #ff0000;
  background-color: var(--color-bg);
  color: var(--color-text);
}
:deep(body a) {
  color: var(--color-text);
}
.cursor-el {
  position: fixed;
  left: 0;
  top: 0;
  pointer-events: none;
  width: 5px;
  height: 5px;
  border-radius: 50%;
  z-index: 11000;
  background: red;
  &::after {
    content: "";
    position: absolute;
    width: 60px;
    height: 60px;
    border-radius: 50%;
    background: transparent;
    border: 1px solid var(--color-text);
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 70px 0 #ffffffd1;
  }
  &.isAlien::before {
    content: "";
    position: absolute;
    width: 60px;
    height: 60px;
    top: 20px;
    left: 20px;
    background: url("@/assets/alien-space-ship.png") no-repeat;
    background-size: contain;
  }
}
</style>
