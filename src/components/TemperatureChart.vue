<template>
  <div class="temperature-chart">
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<script>
import { Chart } from "chart.js";
import axios from "axios";

export default {
  props: ["city"],
  mounted() {
    axios
      .get(
        `https://api.openweathermap.org/data/2.5/forecast?q=${this.city}&appid=928403f3d69e0d00da8ce295529e45fb&units=metric`
      )
      .then((response) => {
        const labels = response.data.list.map((data) => data.dt_txt);
        const temperatures = response.data.list.map((data) => data.main.temp);

        new Chart(this.$refs.chartCanvas.getContext("2d"), {
          type: "line",
          data: {
            labels: labels,
            datasets: [
              {
                label: "Temperature (°C)",
                data: temperatures,
                borderColor: "blue",
                fill: false,
              },
            ],
          },
          options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
              x: {
                ticks: {
                  display: false,
                },
              },
              y: {
                ticks: {
                  beginAtZero: true,
                },
              },
            },
          },
        });
      })
      .catch((error) => {
        console.log(error);
      });
  },
};
</script>

<style scoped>
.temperature-chart {
  max-width: 100%;
}
</style>
