<template>
  <main class="home">
    <section class="instruments">
      <autocomplete-input @city-selected="addCity"></autocomplete-input>
    </section>
    <section class="flex-items" v-for="(city, index) in cities" :key="index">
      <button @click="deleteCity(index)">Delete</button>
      <weather-card :city="city"></weather-card>
      <temperature-chart :city="city"></temperature-chart>
    </section>
    <add-city></add-city>
  </main>
</template>

<script>
import AutocompleteInput from "@/components/AutocompleteInput.vue";
import TemperatureChart from "@/components/TemperatureChart.vue";
import WeatherCard from "@/components/WeatherCard.vue";
import AddCity from "@/components/AddCity.vue";

export default {
  name: "HomeView",
  components: {
    AddCity,
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

<style>
.home {
  padding: 20px;
}
</style>
