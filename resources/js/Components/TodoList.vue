<template>
  <div class="todo-container">
    <h1>To-Do List</h1>
    <form @submit.prevent="addTask">
      <input v-model="newTask" placeholder="Add a new task" required />
      <button type="submit">Add Task</button>
    </form>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        {{ todo.task }}
        <button @click="deleteTask(todo.id)">X</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      todos: [],
      newTask: ''
    };
  },
  mounted() {
    this.fetchTasks();
  },
  methods: {
    fetchTasks() {
      axios.get('/api/todos').then(response => {
        this.todos = response.data;
      });
    },
    addTask() {
      axios.post('/api/todos', { task: this.newTask }).then(response => {
        this.todos.push(response.data);
        this.newTask = '';
      });
    },
    deleteTask(id) {
      axios.delete(`/api/todos/${id}`).then(() => {
        this.todos = this.todos.filter(todo => todo.id !== id);
      });
    }
  }
};
</script>

<style scoped>
.todo-container {
  max-width: 500px;
  margin: auto;
  text-align: center;
}
input {
  padding: 8px;
  margin-right: 10px;
  width: 70%;
}
button {
  padding: 8px 12px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  padding: 8px;
  margin: 5px 0;
  background-color: #f9f9f9;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
