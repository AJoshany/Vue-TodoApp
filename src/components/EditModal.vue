<template>
  <div>
    <div class="backdrop"></div>
    <div class="modal-main">
      <div class="modal-container">
        <input type="text" v-model="todoName" />
        <input type="text" v-model="progress" />
        <div class="btn-group">
          <button @click="emit('editComp', { todoName, progress })">
            Edit
          </button>
          <button @click="emit('onClose')">Close</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import "./EditModal.css";
const props = defineProps(["todoName", "todoProgress"]);
let todoName = ref(props.todoName);
let progress = ref(props.todoProgress);
const emit = defineEmits(["editComp", "onClose"]);
</script>
