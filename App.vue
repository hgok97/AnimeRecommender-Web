<template>
  <div class="app">
    <AnimeRecommender />
  </div>
</template>

<script lang="ts">
import AnimeRecommender from './AnimeRecommender.vue'

export default {
  name: 'App',
  components: {
    AnimeRecommender,
  },
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
  background-color: #10002b;
  color: #a0c4ff;
  font-family: 'Arial', sans-serif;
}

.app {
  min-height: 100vh;
  padding: 20px;
}
</style>
