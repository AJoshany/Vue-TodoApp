<template>
  <div class="todo-main">
    <!-- <h3>Todo-list</h3> -->
    <div class="input-group">
      <input type="text" placeholder="Enter Your Task" v-model="newTodo" />
      <div>
        <input type="number" placeholder="%" min="0" v-model="progress" />
        <button @click="addTodo">Add</button>
      </div>
    </div>

    <div class="todo-categories">
      <button
        @click="category = 'all'"
        class="categorybtn"
        :class="{ activeCategory: category === 'all' }"
      >
        All
      </button>
      <button
        @click="category = 'completed'"
        class="categorybtn"
        :class="{ activeCategory: category === 'completed' }"
      >
        Completed
      </button>
      <button
        @click="category = 'uncompleted'"
        class="categorybtn"
        :class="{ activeCategory: category === 'uncompleted' }"
      >
        Uncompleted
      </button>
    </div>

    <ul class="todo-list">
      <li v-for="(item, index) in filteredTodos" :key="index">
        <div class="todo-list-text">
          <div class="todo-name" :class="{ complete: item.isCompleted }">
            <input
              type="checkbox"
              @click="handleCompleted(item.id)"
              :checked="item.isCompleted"
            />
            {{ item.title }}
          </div>
          <div class="list-buttons">
            <button
              @click="editTodo(item)"
              :style="{ opacity: item.isCompleted ? 0.5 : 1 }"
            >
              ⛏
            </button>
            <button
              @click="removeTodo(index)"
              :style="{ opacity: item.isCompleted ? 0.5 : 1 }"
            >
              ❌
            </button>
          </div>
        </div>
        <div class="progress" :style="{ opacity: item.isCompleted ? 0.5 : 1 }">
          <div
            :class="['progress-bar', item.pBarClass]"
            :style="{ width: item.progress + '%' }"
          ></div>
        </div>
      </li>
    </ul>
    <EditModal
      :todoName="editingTodo.title"
      :todoProgress="editingTodo.progress"
      v-if="showEditModal"
      v-on:editComp="updateTodo"
      @onClose="closeModal"
    />
  </div>
</template>
إ

<script setup>
import "./TodoComp.css";
import EditModal from "./EditModal.vue";
import { computed, reactive, ref, toRef, watch } from "vue";

let newTodo = ref("");
let progress = ref();
let todos = reactive([]);
let editingTodo = reactive({});
let showEditModal = ref(false);
const category = ref("all");

const filteredTodos = computed(() => {
  if (category.value == "all") return todos;
  if (category.value === "completed") {
    return todos.filter((item) => {
      return item.isCompleted === true;
    });
  }
  if (category.value === "uncompleted") {
    return todos.filter((item) => {
      return item.isCompleted === false;
    });
  }
});

function changeProgressColor(progress) {
  let isRed = false;
  let isGreen = false;
  let isOrange = false;
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
  if (progress.value >= 100) {
    progress.value = 100;
  }
  if (isRed) return "red-progressBar";
  if (isGreen) return "green-progressBar";
  if (isOrange) return "orange-progressBar";
  return "";
}

function addTodo() {
  if (newTodo.value.trim() !== "") {
    todos.push({
      id: Date.now(),
      title: newTodo.value,
      progress: progress.value,
      isCompleted: false,
      pBarClass: changeProgressColor(progress),
    });
    newTodo.value = "";
    progress.value = "";
    category.value = "all";
  }
  // console.log(todos);
  // console.log(filteredTodos);
}

function removeTodo(index) {
  todos.splice(index, 1);
}
function handleCompleted(id) {
  let todo = todos.find((item) => {
    return item.id === id;
  });
  todo.isCompleted = !todo.isCompleted;
}

function editTodo(item) {
  showEditModal.value = true;
  editingTodo = { ...item };
  // console.log(editingTodo);
}

function updateTodo(editedTodo) {
  console.log(editedTodo);
  let sTodo = todos.find((item) => item.id === editingTodo.id);
  sTodo.title = editedTodo.todoName;
  sTodo.progress = editedTodo.progress;
  progress.value = editedTodo.progress;
  sTodo.pBarClass = changeProgressColor(progress);
  editingTodo.value = {};
  showEditModal.value = false;
  progress.value = "";
}

function closeModal() {
  showEditModal.value = false;
}
</script>

<style scoped></style>
