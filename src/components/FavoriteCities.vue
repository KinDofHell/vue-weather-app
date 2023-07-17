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
      <weather-card :city="city"></weather-card>
      <temperature-chart :city="city"></temperature-chart>
    </div>
    <button v-if="favoriteCities.length < 5" @click="showAddCityModal">
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
      this.favoriteCities.push(city);
      localStorage.setItem(
        "favoriteCities",
        JSON.stringify(this.favoriteCities)
      );
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
  },
};
</script>

<style scoped>
.weather-block {
  margin-bottom: 20px;
}

.delete-button {
  position: absolute;
  top: 10px;
  right: 10px;
  border: none;
  background: none;
  font-size: 14px;
  color: red;
  cursor: pointer;
}
</style>
