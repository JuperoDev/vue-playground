<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    
    <!-- Display fetched todo item -->
    <div v-if="todo">
      <h3>Fetched Todo Item:</h3>
      <p><strong>ID:</strong> {{ todo.id }}</p>
      <p><strong>Title:</strong> {{ todo.title }}</p>
      <p><strong>Completed:</strong> {{ todo.completed }}</p>
    </div>

    <!-- Form to create a new todo -->
    <h3>Create a New Todo</h3>
    <form @submit.prevent="createTodo">
      <label for="title">Title:</label>
      <input type="text" v-model="newTodoTitle" id="title" placeholder="Enter title" />

      <label for="completed">Completed:</label>
      <input type="checkbox" v-model="newTodoCompleted" id="completed" />

      <button type="submit">Create Todo</button>
    </form>

    <!-- Display created todo -->
    <div v-if="createdTodo">
      <h3>Created Todo Item:</h3>
      <p><strong>ID:</strong> {{ createdTodo.id }}</p>
      <p><strong>Title:</strong> {{ createdTodo.title }}</p>
      <p><strong>Completed:</strong> {{ createdTodo.completed }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
// import HelloWorld from './components/HelloWorld.vue';

// Reactive variables
const todo = ref(null);  // For fetched todo data
const newTodoTitle = ref('');  // For new todo's title
const newTodoCompleted = ref(false);  // For new todo's completed status
const createdTodo = ref(null);  // For storing the response of the created todo

// Fetch todo from the API
const fetchTodo = async () => {
  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/todos/1');
    todo.value = response.data;
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};

// Create a new todo
const createTodo = async () => {
  try {
    const response = await axios.post('https://jsonplaceholder.typicode.com/todos', {
      title: newTodoTitle.value,
      completed: newTodoCompleted.value
    });
    createdTodo.value = response.data;  // Store the created todo data
    console.log('Todo created:', response.data);
  } catch (error) {
    console.error('Error creating todo:', error);
  }
};

// Fetch todo when the component is mounted
onMounted(() => {
  fetchTodo();
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

form {
  margin-top: 20px;
}

input[type="text"] {
  margin-right: 10px;
}

button {
  margin-top: 10px;
}
</style>
