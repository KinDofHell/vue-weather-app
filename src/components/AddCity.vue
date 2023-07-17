<template>
  <div>
    <div
      v-for="(city, index) in favoriteCities"
      :key="index"
      class="weather-block"
    >
      <button class="delete-button" @click="confirmDeleteCity(index)">
        &#10006;
      </button>
      <weather-card
        :city="city.name"
        @toggle-favorite="toggleFavorite"
      ></weather-card>
      <temperature-chart :city="city.name"></temperature-chart>
    </div>
    <button
      class="add-button"
      v-if="favoriteCities.length < 5"
      @click="showAddCityModal"
    >
      Add City
    </button>
    <add-city-modal
      v-if="showModal"
      @city-added="addCity"
      @modal-closed="closeModal"
    ></add-city-modal>
    <confirmation-modal
      v-if="showConfirmationModal"
      @confirm="deleteCity"
      @cancel="cancelDelete"
    ></confirmation-modal>
  </div>
</template>

<script>
import WeatherCard from "./WeatherCard.vue";
import TemperatureChart from "./TemperatureChart.vue";
import AddCityModal from "./AddCityModal.vue";
import ConfirmationModal from "./ConfirmationModal.vue";

export default {
  components: {
    WeatherCard,
    TemperatureChart,
    AddCityModal,
    ConfirmationModal,
  },
  data() {
    return {
      favoriteCities: [],
      showModal: false,
      showConfirmationModal: false,
      cityToDelete: null,
    };
  },
  mounted() {
    this.favoriteCities =
      JSON.parse(localStorage.getItem("favoriteCities")) || [];
  },
  methods: {
    showAddCityModal() {
      this.showModal = true;
    },
    addCity(city) {
      if (this.favoriteCities.length >= 5) {
        // Показати модальне вікно, якщо досягнуто максимальну кількість обраних міст
        alert(
          "You have reached the maximum limit of favorite cities. Please remove a city to add a new one."
        );
        return;
      }

      // Перевірка, чи місто вже є в списку обраних
      if (
        !this.favoriteCities.some((favoriteCity) => favoriteCity.name === city)
      ) {
        this.favoriteCities.push({ name: city, isFavorite: true });
        localStorage.setItem(
          "favoriteCities",
          JSON.stringify(this.favoriteCities)
        );
      }
      this.showModal = false;
    },
    closeModal() {
      this.showModal = false;
    },
    confirmDeleteCity(index) {
      this.cityToDelete = index;
      this.showConfirmationModal = true;
    },
    deleteCity() {
      this.favoriteCities.splice(this.cityToDelete, 1);
      localStorage.setItem(
        "favoriteCities",
        JSON.stringify(this.favoriteCities)
      );
      this.showConfirmationModal = false;
    },
    cancelDelete() {
      this.showConfirmationModal = false;
    },
    toggleFavorite(city, isFavorite) {
      const index = this.favoriteCities.findIndex(
        (favoriteCity) => favoriteCity.name === city
      );
      if (index !== -1) {
        this.favoriteCities[index].isFavorite = isFavorite;
        localStorage.setItem(
          "favoriteCities",
          JSON.stringify(this.favoriteCities)
        );
      }
    },
  },
};
</script>

<style scoped>
.weather-block {
  margin-bottom: 20px;
}

.add-button {
  position: absolute;
  top: 140px;
  right: 320px;

  border: none;
  border-radius: 10px;

  background-color: #1a67d9;
  color: white;

  padding: 10px;
}

.delete-button {
  border: none;
  background: none;
  font-size: 14px;
  color: red;
  cursor: pointer;
}
</style>
