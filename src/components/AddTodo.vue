<template>
  <form @submit.prevent="addTodo">
    <input type="text" placeholder="Enter todo" v-model="newTodo" />
    <button>Add Todo</button>
  </form>
</template>

<script setup>
import { ref } from "vue";
const newTodo = ref("");
const emits = defineEmits(["addTodo"]);

function addTodo() {
  emits("addTodo", newTodo.value);
  newTodo.value = "";
}
</script>

<style>
form {
  padding: 16px;
  border-bottom: 1px solid #333;
  display: flex;
  align-items: center;
  gap: 8px;
}

input {
  border: 1px solid #262626;
  padding: 8px;
}

button {
  border: none;
  padding: 8px;
  background-color: black;
  color: white;
}
</style>
