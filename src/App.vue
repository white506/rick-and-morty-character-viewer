<template>
  <div class="container">
    <h1 class="container__title">Welcome to the Rick and Morty Universe</h1>
    <CharacterList />
  </div>
</template>

<script setup>
import CharacterList from "./components/CharacterList.vue";
</script>

<style scoped>
.container {
  max-width: 1350px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

.container__title {
  font-size: 2.5em;
  margin-bottom: 20px;
  color: #ffffffde;
}
</style>
