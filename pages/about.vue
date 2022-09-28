<script setup lang="ts">
import { ref } from "vue";
const msg = "Hello World";
const counter = ref(0);
const URL = " https://pokeapi.co/api/v2/pokemon/pikachu";
const pokemon = ref(null);

async function writeInConsole(text: string) {
  counter.value++;
  console.log(text);
  const pokeInfo = await fetch(URL);
  const pokeInfoJson = await pokeInfo.json();
  pokemon.value = pokeInfoJson;
  console.log(pokemon.value);
}
</script>
<template>
  <div>
    <h2>about</h2>
    <Counter :counter="counter" />
    <VButton @writeInConsole="writeInConsole" class="green"/>
    <VButtonTS
      @writeInConsole="writeInConsole"
      :msg="msg"
    />

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
<style></style>
