<template>
  <main class="home flex-items">
    <section class="instruments">
      <autocomplete-input @city-selected="addCity"></autocomplete-input>
    </section>
    <section class="cards flex-items-column">
      <section class="flex-items-column weather-block" v-if="this.city">
        <button class="delete-button" @click="deleteCity()">Delete</button>
        <weather-card
          :city="this.city.name"
          @toggle-favorite="updateFavoriteCities"
        ></weather-card>
      </section>
      <add-city></add-city>
    </section>
    <article class="fav flex-items-column">
      <h2>Favorites</h2>
      <ul>
        <li>sgdfg</li>
        <li>sdfgsdfg</li>
        <li>dsfgsdfg</li>
        <li>sdfgsdfg</li>
      </ul>
    </article>
  </main>
</template>

<script>
import AutocompleteInput from "@/components/AutocompleteInput.vue";
import WeatherCard from "@/components/WeatherCard.vue";
import AddCity from "@/components/AddCity.vue";

export default {
  name: "HomeView",
  components: {
    AddCity,
    WeatherCard,
    AutocompleteInput,
  },
  data() {
    return {
      city: JSON.parse(localStorage.getItem("searchedCity")) || null,
      favoriteCities: JSON.parse(localStorage.getItem("favorites")),
    };
  },
  methods: {
    addCity(city) {
      this.city = city;
      localStorage.setItem("searchedCity", JSON.stringify(this.city));
    },
    deleteCity() {
      this.city = null;
      localStorage.removeItem("searchedCity");
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

  margin-top: 100px;
}
.weather-block {
  border: 1px solid black;
  border-radius: 10px;

  background-color: lightblue;
}
.delete-button {
  border: none;
  border-radius: 10px;

  background-color: red;
  color: white;

  width: 100%;

  font-size: 30px;

  cursor: pointer;

  padding: 0 10px;

  transition: all 0.3s;

  &:hover {
    background-color: darkred;
    scale: 0.8;
  }
}
.fav {
  width: 300px;
}
</style>
