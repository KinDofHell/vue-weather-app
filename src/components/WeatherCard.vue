<template>
  <div class="weather-card">
    <h2>{{ city }}</h2>
    <div>{{ weatherDescription }}</div>
    <div>{{ temperature }}°C</div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: ["city"],
  data() {
    return {
      weatherDescription: "",
      temperature: null,
    };
  },
  mounted() {
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
};
</script>

<style scoped>
.weather-card {
  background-color: #f2f2f2;
  padding: 20px;
  border-radius: 4px;
}
</style>
