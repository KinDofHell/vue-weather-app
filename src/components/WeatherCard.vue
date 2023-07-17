<template>
  <article
    class="weather-card flex-items-column"
    :class="{ favorite: isFavorite }"
  >
    <h2>{{ city }}</h2>
    <p>{{ weatherDescription }}</p>
    <p>{{ temperature }}°C</p>
    <button @click="toggleFavorite" v-if="!isFavorite">Add to Favorites</button>
    <button @click="toggleFavorite" v-else>Remove from Favorites</button>
  </article>
</template>

<script>
import axios from "axios";

export default {
  props: ["city"],
  data() {
    return {
      weatherDescription: "",
      temperature: null,
      isFavorite: false,
    };
  },
  mounted() {
    this.getWeatherData();
    this.loadFavoritesFromLocalStorage();
  },
  methods: {
    getWeatherData() {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=928403f3d69e0d00da8ce295529e45fb&units=metric`
        )
        .then((response) => {
          this.weatherDescription = response.data.weather[0].description;
          this.temperature = response.data.main.temp;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    toggleFavorite() {
      this.isFavorite = !this.isFavorite;
      this.saveFavoritesToLocalStorage();
      this.$emit("toggle-favorite", this.city, this.isFavorite);
    },
    loadFavoritesFromLocalStorage() {
      const favorites = localStorage.getItem("favorites");
      if (favorites) {
        const parsedFavorites = JSON.parse(favorites);
        this.isFavorite = parsedFavorites.includes(this.city);
      }
    },
    saveFavoritesToLocalStorage() {
      let favorites = localStorage.getItem("favorites");
      if (!favorites) {
        favorites = [];
      } else {
        favorites = JSON.parse(favorites);
      }

      if (this.isFavorite) {
        favorites.push(this.city);
      } else {
        favorites = favorites.filter((favorite) => favorite !== this.city);
      }

      localStorage.setItem("favorites", JSON.stringify(favorites));
    },
  },
};
</script>

<style scoped>
.weather-card {
  background-color: #f2f2f2;
  padding: 20px;
  border-radius: 10px;
  width: fit-content;
}

.favorite {
  border: 2px solid blue;
}

button {
  margin-top: 10px;
}
</style>
