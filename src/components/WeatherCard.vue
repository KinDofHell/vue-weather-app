<template>
  <article
    class="weather-card flex-items-column"
    :class="{ favorite: isFavorite }"
  >
    <section class="title flex-items">
      <h2>{{ city }}</h2>
      <div class="icon">
        <img :src="weatherIconUrl" alt="Weather Icon" v-if="weatherIconUrl" />
      </div>
    </section>
    <p class="temp">{{ temperature }}°C</p>
    <button @click="toggleFavorite" v-if="!isFavorite">Add to Favorites</button>
    <button @click="toggleFavorite" v-else>Remove from Favorites</button>
  </article>
  <temperature-chart :city="city"></temperature-chart>
</template>

<script>
import axios from "axios";
import TemperatureChart from "@/components/TemperatureChart.vue";

export default {
  components: { TemperatureChart },
  props: ["city"],
  data() {
    return {
      temperature: null,
      isFavorite: false,
      weatherIcon: null,
    };
  },
  computed: {
    weatherIconUrl() {
      if (this.weatherIcon) {
        return `https://openweathermap.org/img/wn/${this.weatherIcon}.png`;
      }
      return null;
    },
  },
  watch: {
    city(newCity) {
      this.getWeatherData(newCity);
    },
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
          this.temperature = response.data.main.temp;
          this.weatherIcon = response.data.weather[0].icon;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    toggleFavorite() {
      if (!this.isFavorite)
        if (JSON.parse(localStorage.getItem("favorites")).length >= 5) {
          alert("You have reached the maximum limit of favorite cities!");
          return;
        }
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
  padding: 10px;
}
.title > h2 {
  background-color: #c0c040;
  padding: 5px 10px;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;

  width: max-content;
}
.icon {
  background-color: goldenrod;
  border-radius: 5px;
}

.favorite {
  border: 2px solid blue;
}

button {
  margin-top: 10px;
  padding: 5px;
  border-radius: 5px;
}
</style>
