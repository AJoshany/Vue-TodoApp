<template>
  <div class="todo-main">
    <h3>Todo-list</h3>
    <div class="input-group">
      <input type="text" placeholder="Enter Your Task" v-model="newTodo" />
      <div>
        <input type="number" placeholder="%" min="0" v-model="progress" />
        <button @click="addTodo">Add</button>
      </div>
    </div>

    <ul class="todo-categories">
      <li v-for="(category, index) in categories" :key="index">
        <a>
          {{ category }}
        </a>
      </li>
    </ul>

    <ul class="todo-list">
      <li v-for="(item, index) in todos" :key="index">
        <div class="todo-list-text">
          <div class="todo-name">
            <input type="checkbox" />
            {{ item.title }}
          </div>
          <!-- <span class="todo-remainingDays">2 days left</span> -->
          <button @click="removeTodo(index)">❌</button>
        </div>
        <div class="progress">
          <div
            :class="['progress-bar', item.pBarClass]"
            :style="{ width: item.progress + '%' }"
          ></div>
        </div>
      </li>
    </ul>
  </div>
</template>
إ

<script setup>
import "./TodoComp.css";
import { computed, reactive, ref, toRef, watch } from "vue";
let newTodo = ref("");
let progress = ref();
let todos = reactive([]);
const categories = reactive(["All", "Completed", "Uncompleted"]);

function addTodo() {
  let isRed = false;
  let isGreen = false;
  let isOrange = false;
  if (newTodo.value.trim() !== "") {
    if (progress.value <= 30) {
      isRed = true;
    }
    if (progress.value > 30 && progress.value < 70) {
      isRed = false;
      isOrange = true;
    }
    if (progress.value >= 70) {
      isOrange = false;
      isGreen = true;
      isRed = false;
    }
    todos.push({
      id: Date.now(),
      title: newTodo.value,
      progress: progress.value,
      pBarClass: computed(() => {
        if (isRed) return "red-progressBar";
        if (isGreen) return "green-progressBar";
        if (isOrange) return "orange-progressBar";
        return "";
      }),
    });

    console.log(todos);
    newTodo.value = "";
    progress.value = "";
  }
}

function removeTodo(index) {
  todos.splice(index, 1);
}
</script>

<style scoped></style>
