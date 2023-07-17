<template>
  <div class="modal-overlay">
    <div class="modal">
      <h2>Add City</h2>
      <input
        type="text"
        v-model="searchText"
        placeholder="Enter city name..."
      />
      <button @click="searchCity">Search</button>
      <ul v-if="searchResults.length > 0">
        <li
          v-for="result in searchResults"
          :key="result.id"
          @click="addCity(result)"
        >
          {{ result.name }}
        </li>
      </ul>
      <p v-else>No results found.</p>
      <button @click="closeModal">Close</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      searchText: "",
      searchResults: [],
    };
  },
  methods: {
    searchCity() {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/find?q=${this.searchText}&appid=928403f3d69e0d00da8ce295529e45fb`
        )
        .then((response) => {
          this.searchResults = response.data.list;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    addCity(city) {
      this.$emit("city-added", city.name);
      this.closeModal();
    },
    closeModal() {
      this.$emit("modal-closed");
    },
  },
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background-color: #fff;
  padding: 20px;
  border-radius: 4px;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

li {
  cursor: pointer;
}

button {
  margin-top: 10px;
}
</style>
