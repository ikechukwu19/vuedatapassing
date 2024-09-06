<template>
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
</template>

<script setup>
const props = defineProps({
  todos: {
    type: Array,
    required: true,
  },
  activeTodos: {
    type: Number,
    required: true,
    default: 0,
  },
});
</script>

<style>
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
