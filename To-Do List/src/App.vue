<script setup>
import { ref } from "vue";

const newTodo = ref("");
const todoList = ref([]);

const addTodo = () => {
  if (newTodo.value.trim() === "") return;

  todoList.value.push({
    id: Date.now(),
    text: newTodo.value,
  });

  newTodo.value = "";
};

const removeTodo = (id) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== id);
};
</script>

<template>
  <div class="container">
    <h1>📝 Vue To-Do List</h1>
    <input
      type="text"
      placeholder="Enter your task here!"
      v-model="newTodo"
      @keyup.enter="addTodo"
    />
    <button @click="addTodo">Add</button>
    <ul>
      <li v-for="todo in todoList" :key="todo.id">
        {{ todo.text }}
        <button @click="removeTodo(todo.id)">❌</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.container {
  max-width: 400px;
  margin: 50px auto;
  font-family: sans-serif;
}

input {
  width: 70%;
  padding: 8px;
}

button {
  padding: 8px;
  margin-left: 5px;
}

ul {
  padding: 0;
  width: 87%;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 10px;
}
</style>
