<template>
  <section class="input-search">
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
  </section>
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
      this.searchText = "";
      this.showAutocomplete = false;
      this.$emit("city-selected", city);
    },
  },
};
</script>

<style scoped>
.input-search {
  position: absolute;
  right: 50px;
  top: 135px;
}
input {
  border: none;
  border-radius: 10px;

  font-size: 18px;
  font-weight: bold;
  text-align: center;

  background-color: lightgray;

  padding: 10px 5px;
}
.input-search > ul {
  list-style-type: none;

  background-color: darkgray;

  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
}

li {
  cursor: pointer;

  text-align: center;
  padding: 10px 0;

  font-size: 18px;

  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;

  &:not(:last-child) {
    border-bottom: 1px solid black;
  }
}
</style>
