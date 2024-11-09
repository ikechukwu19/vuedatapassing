<template>
  <main>
    <h2>Todos</h2>
    <div class="wrapper">
      <AddTodo @addTodo="addTodo2" />
      <Todos :todos="todos" :activeTodos="activeTodos" @toggle="toggleTodo" />
      <!-- <form @submit.prevent="addTodo">
        <input type="text" placeholder="Enter todo" v-model="newTodo" />
        <button>Add Todo</button>
      </form> -->
      <!-- <div>
        <div class="todo_wrapper">
          <h3>Active Todos {{ activeTodos }}</h3>
          <div
            class="todo"
            :class="todo.completed ? 'completed' : ''"
            v-for="todo in todos"
            :key="todo.id"
          >
            <input
              type="checkbox"
              name="check"
              :id="todo.id"
              v-model="todo.completed"
            />
            <label :for="todo.id">
              {{ todo.title }}
            </label>
          </div>
        </div>
      </div> -->
    </div>
  </main>
</template>

<script setup>
import { ref, computed } from "vue";
import Todos from "@/components/Todos.vue";
import AddTodo from "@/components/AddTodo.vue";

const newTodo = ref("");
const todos = ref([
  {
    id: 1,
    title: "Do laundry",
    completed: false,
  },
  {
    id: 2,
    title: "Read a book",
    completed: false,
  },
  {
    id: 3,
    title: "Go to gym",
    completed: false,
  },
  {
    id: 4,
    title: "Visit a friend",
    completed: true,
  },
]);

const activeTodos = computed(() => {
  const active = todos.value.filter((todo) => todo.completed === false);
  return active.length;
});

function addTodo2(data) {
  if (data === "") return window.alert("Todo cannot be empty");
  todos.value.push({
    id: todos.value.length + 1,
    title: data,
    completed: false,
  });
}

function toggleTodo(id) {
  todos.value.forEach((todo) => {
    if (todo.id === id) {
      todo.completed = !todo.completed;
    }
  });
}

const addTodo = () => {
  if (newTodo.value === "") return window.alert("Todo cannot be empty");
  todos.value.push({
    id: todos.value.length + 1,
    title: newTodo.value,
    completed: false,
  });
  newTodo.value = "";
};
</script>

<style scoped>
main {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  padding: 30px;
}

.wrapper {
  border: 1px solid #333;
  /* padding: 16px; */
  border-radius: 8px;
}

h2 {
  padding: 0 16px;
}

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

.completed {
  text-decoration: line-through;
  color: rgba(0, 0, 0, 0.212);
}

.todo_wrapper {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 16px;
}

.todo {
  /* border: 1px solid #333; */
  display: flex;
  align-items: center;
  gap: 8px;
}

label {
  cursor: pointer;
}
</style>
