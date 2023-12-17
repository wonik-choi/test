<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-md"
      />
      <ul
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
        v-if="mapboxSearchResults"
      >
        <li v-for="place in mapboxSearchResults" :key="place.id" class="py-2 cursor-pointer">
          {{ place.properties.name }}
        </li>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const mapboxAPIKey = `pk.eyJ1IjoieWVsaWhpIiwiYSI6ImNscTkyeTlpbTFpd2cyaWxrbmNmaXR0cngifQ.gr-Weu_evN5Ic1C5lgLVHQ`;
const searchQuery = ref('');
const queryTimeout = ref(null);
const mapboxSearchResults = ref(null);

// debounce 를 setTimeout 으로 한번에 구현
const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== '') {
      const result = await axios.get(
        `https://api.mapbox.com/search/searchbox/v1/forward?q=${searchQuery.value}&access_token=${mapboxAPIKey}`
      );
      mapboxSearchResults.value = result.data.features;
      console.log(mapboxSearchResults.value);
      return;
    }
    mapboxSearchResults.value = null;
  }, 300);
};
</script>
