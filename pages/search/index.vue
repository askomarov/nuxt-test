<script setup>
import { ref } from "vue";

const shows = ref([]);
const search = ref("");
const URL = "https://api.tvmaze.com/search/shows?q=";

async function searchShow(search) {
  const data = await fetch(URL + search.value);
  const dataJson = await data.json();
  shows.value = dataJson;
}

async function submit() {
  if (!search.value) return;
  searchShow(search);
}
</script>

<template>
  <div>
    <div class="container">
      <h2>Find TV Shows</h2>
      <form
        @submit.prevent="submit"
        class="relative inline-grid grid-cols-[1fr,auto] rounded-xl p-2 border border-dark-300 gap-1 mb-5"
      >
        <input
          class="border border-dark-300 rounded-xl p-2 width-full height-full"
          type="text"
          name="search show value"
          id="search show value"
          v-model="search"
        />
        <button
          type="submit"
          class="btn rounded-xl"
        >
          Seacrh
        </button>
      </form>
      <ul class="shows">
        <li
          class="show"
          v-for="show in shows"
          :key="show.id"
        >
          <NuxtLink
            :to="`/search/${show.show.id}`"
            class="show-item-link"
          >
            <span>
              {{ show.show.name }}
            </span>
            <div class="relative overflow-hidden w-full aspect-[9/15] mt-2">
              <img
                :src="show.show.image?.medium"
                width="120"
                height="120"
                :alt="show.show.name"
              />
            </div>
          </NuxtLink>
        </li>
      </ul>
    </div>
  </div>
</template>
<style lang="scss">
.btn {
  @apply text-red-600 bg-cyan-400 p-2;
}
.shows {
  @apply grid grid-cols-[repeat(auto-fill,minmax(150px,200px))] justify-center gap-4;
  align-items: start;
}

.show-item-link {
  padding: 1rem;
  display: inline-flex;
  flex-direction: column;
  text-align: center;
  width: 100%;
  transition: box-shadow 0.3s ease 0s;
  box-shadow: 0 0 3px 0 #c4c4;
  border-radius: 10px;
  &:hover {
    box-shadow: 0 0 6px 1px rgba(155, 42, 155, 0.267);
    img {
      scale: 1.1;
    }
  }
  img {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    object-fit: cover;
    transition: scale 0.3s ease 0s;
  }
}
</style>
