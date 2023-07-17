<template>
  <header class="flex-items">
    <h1>WeatherApp</h1>
    <nav class="flex-items">
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>
      <router-link to="/contacts">Contacts</router-link>
    </nav>
  </header>
</template>

<script>
export default {
  name: "AppHeader",
};
</script>

<style>
header {
  padding: 30px 20px;

  background-color: #338633;
}
nav {
  width: 350px;
}
header > h1 {
  color: darkblue;
  background-color: #c0c040;

  padding: 10px;

  border-radius: 10px;
}

nav > a {
  text-decoration: none;
  text-transform: uppercase;

  font-size: 20px;

  color: white;

  transition: color 0.4s ease-in, scale 0.2s ease-in-out;
  &:hover {
    scale: 1.5;
    color: black;
  }
}
</style>
