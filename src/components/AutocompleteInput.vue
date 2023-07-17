<template>
  <div>
    <input
      type="text"
      v-model="searchText"
      @input="handleInput"
      placeholder="Search city..."
    />
    <ul v-if="showAutocomplete">
      <li
        v-for="city in autocompleteCities"
        :key="city.id"
        @click="selectCity(city)"
      >
        {{ city.name }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      searchText: "",
      autocompleteCities: [],
      showAutocomplete: false,
    };
  },
  methods: {
    handleInput() {
      if (this.searchText.length > 2) {
        axios
          .get(
            `https://api.openweathermap.org/data/2.5/find?q=${this.searchText}&appid=928403f3d69e0d00da8ce295529e45fb`
          )
          .then((response) => {
            this.autocompleteCities = response.data.list;
            this.showAutocomplete = true;
          })
          .catch((error) => {
            console.log(error);
          });
      } else {
        this.autocompleteCities = [];
        this.showAutocomplete = false;
      }
    },
    selectCity(city) {
      this.searchText = city.name;
      this.showAutocomplete = false;
      this.$emit("city-selected", city);
    },
  },
};
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

li {
  cursor: pointer;
}
</style>
