<template>
  <AppHeader />
  <main>
    <router-view></router-view>
  </main>
</template>

<script>
import "./assets/global.css";

import AppHeader from "@/layouts/AppHeader.vue";

export default {
  components: {
    AppHeader,
  },
};
</script>

<style>
h1 {
  text-align: center;
}
</style>
