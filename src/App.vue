<template>
  <div class="main-app">
    <h1>Todo App</h1>
    <TodoComp @sendNameToParent="handleNameUpdate" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import TodoComp from "./components/TodoComp.vue";
</script>

<style scoped>
.main-app {
  display: flex;
  flex-direction: column;
  gap: 4rem;
}
</style>
