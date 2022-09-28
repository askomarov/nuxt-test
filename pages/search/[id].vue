<script setup>
import { ref, onMounted } from "vue";
const route = useRoute();
const episodes = ref([]);
const URL = ` https://api.tvmaze.com/shows/${route.params.id}?embed=episodes`;

onMounted(async () => {
  const episodeData = await fetch(URL);
  const dataJson = await episodeData.json();
  episodes.value = dataJson._embedded.episodes;
});
</script>
<template>
  <div class="container">
    <NuxtLink to="/search">Back to Search</NuxtLink>
    <h2>This search ID page: {{ $route.params.id }}</h2>
    <h2>This search ID page second way: {{ route.params.id }}</h2>
    <h3 class="text-center mb-5 font-bold">List of episods</h3>
    <ul
      class="shows"
      v-if="episodes.length > 0"
    >
      <li
        class="show"
        v-for="ep in episodes"
        :key="ep.id"
      >
        <a
          href=""
          class="show-item-link"
        >
          <h4>
            {{ ep.name }}
          </h4>
          <p>Season: {{ ep.season }}</p>
          <p>Episode: {{ ep.number }}</p>
          <div class="relative overflow-hidden w-full aspect-[9/15] mt-2">
            <img
              :src="ep.image?.medium"
              width="120"
              height="120"
              :alt="ep.name"
            />
          </div>
          <span :date-time="ep.airdate">Date: {{ ep.airdate }}</span>
          <details class="text-sm">
            <summary>Show Episode description</summary>
            <div
              v-html="ep.summary"
              class="epsiode-descr-text text-justify pt-2"
            ></div>
          </details>
        </a>
      </li>
    </ul>
    <h3 v-else>No Episodes</h3>
  </div>
</template>

<style lang="scss" scoped>
details[open] {
  position: relative;
  .epsiode-descr-text {
    padding: 1rem;
    position: absolute;
    z-index: 2;
    min-width: 270px;
    min-height: 60px;
    background: #c4c4c4;
  }
}
</style>
