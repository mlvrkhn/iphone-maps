<template>
  <div class="container" id="app">
    <autocomplete
      :search="getCoordinates"
      placeholder="Enter city name"
      aria-label="Enter city name"
      :get-result-value="getResultValue"
    ></autocomplete>
    <div v-if="coordinates">
      <h2>Coordinates for {{ city }}:</h2>
      <p>Latitude: {{ coordinates.lat }}</p>
      <p>Longitude: {{ coordinates.lng }}</p>
    </div>

    <div v-if="error">
      <p style="color: red">{{ error }}</p>
    </div>
    <button @click="downloadMap">Download</button>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Autocomplete from "@trevoreyre/autocomplete-vue";

const city = ref("");
const coordinates = ref(null);
const error = ref(null);

const getCoordinates = async (input) => {
  try {
    const url = `https://api.mapbox.com/search/searchbox/v1/suggest?q=${encodeURI(
      input
    )},+po&language=en&session_token=0f206044-3a78-42e1-8913-383ccd20b30b&access_token=pk.eyJ1IjoibWx2cmtobiIsImEiOiJjbHpsbzkzanMwM2kyMnJwbmo1dnhkZTlkIn0.eYXg3LX3YBuBLcY0Q6gMIQ`;
    return new Promise((resolve) => {
      if (input.length < 3) {
        return resolve([]);
      }

      fetch(url)
        .then((response) => response.json())
        .then((data) => {
          resolve(data.suggestions);
        });
    });
  } catch (err) {
    error.value = "City not found";
  }
};

const getResultValue = (result) => {
  return result.name;
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: row;
  /* align-items: center;
  justify-content: center; */
}
</style>
