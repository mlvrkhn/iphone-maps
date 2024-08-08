<template>
  <div id="app">
    <autocomplete
      :search="getCoordinates"
      placeholder="Enter city name"
      aria-label="Enter city name"
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
  console.log("🚀   getCoordinates  input:", input);
  try {
    if (input.length < 1) {
      return [];
    }
    const url = `https://api.mapbox.com/search/searchbox/v1/suggest?q=${input},+po&language=en&session_token=0f206044-3a78-42e1-8913-383ccd20b30b&access_token=pk.eyJ1IjoibWx2cmtobiIsImEiOiJjbHpsbzkzanMwM2kyMnJwbmo1dnhkZTlkIn0.eYXg3LX3YBuBLcY0Q6gMIQ`;
    const response = await fetch(url);
    const data = await response.json().suggestions[0];
    console.log("🚀   getCoordinates  data:", data);
    coordinates.value = {
      lat: data.coord.lat,
      lng: data.coord.lon,
    };
  } catch (err) {
    error.value = "City not found";
  }
};

const getResultValue = (result) => {
  console.log(result);
};
</script>

<style scoped></style>
