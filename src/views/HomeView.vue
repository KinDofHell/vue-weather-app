<template>
  <main class="home">
    <section class="instruments">
      <section class="fav flex-items">
        <h2 class="fav-title">Favorite:</h2>
        <h3 v-for="(city, index) in favoriteCities" :key="index">
          {{ city }}
        </h3>
      </section>
      <autocomplete-input @city-selected="addCity"></autocomplete-input>
    </section>
    <section
      class="flex-items weather-block"
      v-for="(city, index) in cities"
      :key="index"
    >
      <button class="delete-button" @click="deleteCity(index)">&#10006;</button>
      <weather-card
        :city="city.name"
        @toggle-favorite="updateFavoriteCities"
      ></weather-card>
      <temperature-chart :city="city.name"></temperature-chart>
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
      favoriteCities: JSON.parse(localStorage.getItem("favorites")),
    };
  },
  methods: {
    addCity(city) {
      this.cities.push(city);
    },
    deleteCity(index) {
      this.cities.splice(index, 1);
    },
    updateFavoriteCities(city, isFavorite) {
      if (isFavorite) {
        if (!this.favoriteCities.includes(city)) {
          if (this.favoriteCities.length >= 5) {
            alert("You have reached the maximum limit of favorite cities!");
            return;
          }
          this.favoriteCities.push(city);
        }
      } else {
        this.favoriteCities = this.favoriteCities.filter(
          (favoriteCity) => favoriteCity !== city
        );
      }
      localStorage.setItem("favorites", JSON.stringify(this.favoriteCities));
    },
  },
};
</script>

<style>
.home {
  padding: 20px;
}
.weather-block {
  margin-top: 50px;
  margin-bottom: 20px;
  padding: 10px;
  height: auto;

  background-color: lightblue;

  border-radius: 10px;
}
.delete-button {
  border: none;
  background: none;
  font-size: 20px;
  color: red;
  cursor: pointer;

  margin-right: 20px;
}
.fav {
  min-width: 30%;

  max-width: 60%;
}
.fav > h3 {
  background-color: green;
  color: white;
  padding: 2px;
  border-radius: 5px;
}
.fav-title {
  background-color: gray;
  padding: 2px;
  border-radius: 5px;
}
</style>
