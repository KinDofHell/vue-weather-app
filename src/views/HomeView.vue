<template>
  <main class="home">
    <autocomplete-input @city-selected="addCity"></autocomplete-input>
    <div v-for="(city, index) in cities" :key="index">
      <button @click="deleteCity(index)">Delete</button>
      <weather-card :city="city"></weather-card>
      <temperature-chart :city="city"></temperature-chart>
    </div>
    <favorite-cities></favorite-cities>
  </main>
</template>

<script>
import AutocompleteInput from "@/components/AutocompleteInput.vue";
import TemperatureChart from "@/components/TemperatureChart.vue";
import WeatherCard from "@/components/WeatherCard.vue";
import FavoriteCities from "@/components/FavoriteCities.vue";

export default {
  name: "HomeView",
  components: {
    FavoriteCities,
    WeatherCard,
    TemperatureChart,
    AutocompleteInput,
  },
  data() {
    return {
      cities: [],
    };
  },
  methods: {
    addCity(city) {
      this.cities.push(city);
    },
    deleteCity(index) {
      this.cities.splice(index, 1);
    },
  },
};
</script>
