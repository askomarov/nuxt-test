<script setup lang="ts">
const msg = "Hello World";
const counter = ref(0);
const URLp = "https://pokeapi.co/api/v2/pokemon/pikachu";
const pokemon = ref(null);

async function writeInConsole(text: string) {
  counter.value++;
  const pokeInfo = await fetch(URLp);
  const pokeInfoJson = await pokeInfo.json();
  pokemon.value = pokeInfoJson;
}
</script>
<template>
  <div class="container">
    <h2>about</h2>
    <Counter :counter="counter" />
    <VButton @writeInConsole="writeInConsole" class="green" />
    <VButtonTS @writeInConsole="writeInConsole" :msg="msg" />

    <div v-if="pokemon">
      <img
        :src="pokemon.sprites.back_default"
        alt=""
        width="120"
        height="120"
      />
    </div>
  </div>
</template>

<style lang="scss">
</style>
