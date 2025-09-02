<template>
    <div>
        <div class="backdrop">
        </div>
        <div class="modal-main">
           <input type="text" v-model="todoName">
           <input type="text" v-model="progress">
           <div>
            <button>Edit</button>
            <button>Close</button>
           </div>
        </div>
    </div>
    
</template>

<script setup>
import { ref } from 'vue';
import './EditModal.css'
const props = defineProps(['todoName','todoProgress'])
let todoName = ref(props.todoName)
let progress = ref(props.todoProgress)
</script>