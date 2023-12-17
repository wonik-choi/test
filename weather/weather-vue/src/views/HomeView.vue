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
        <p v-if="searchError">Sorry, something went wrong, please try again</p>
        <p v-if="!searchError && mapboxSearchResults.length === 0">찾는 결과가 없습니다!</p>
        <template v-else>
          <li
            v-for="place in mapboxSearchResults"
            :key="place.id"
            @click="previewCity(place)"
            class="py-2 cursor-pointer"
          >
            {{ place.place_name }}
          </li>
        </template>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

const mapboxAPIKey = `pk.eyJ1IjoieWVsaWhpIiwiYSI6ImNscTkyeTlpbTFpd2cyaWxrbmNmaXR0cngifQ.gr-Weu_evN5Ic1C5lgLVHQ`;
const searchQuery = ref('');
const searchError = ref(null);
const queryTimeout = ref(null);
const mapboxSearchResults = ref(null);
const router = useRouter();

// debounce 를 setTimeout 으로 한번에 구현
const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== '') {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`
        );
        mapboxSearchResults.value = result.data.features;
        return;
      } catch (error) {
        console.error(error);
        searchError.value = error;
      }
    }
    mapboxSearchResults.value = null;
  }, 300);
};

const previewCity = (searchResult) => {
  const [city, state] = searchResult.place_name.split(',');
  router.push({
    name: 'city',
    params: {
      state: state.replaceAll(' ', ''),
      city: city.replaceAll(' ', ''),
    },
    query: {
      lat: searchResult.geometry.coordinates[1],
      lng: searchResult.geometry.coordinates[0],
      preview: true,
    },
  });
  console.log(city);
  console.log(state);
};
</script>
