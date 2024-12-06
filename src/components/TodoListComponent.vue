<template>
  <div>
    <div class="content">
      <h2>Todos list</h2>
      
      <div class="list" v-for="todo in todos" :key="todo.id">
        <b>{{ todo.title }} {{ todo.completed ? `✅` : '❌' }}</b> <br/> {{ todo.description }}

        <div class="actions">
          <button class="edit" @click="editTodo(todo)">Edit</button>
          <button class="delete" @click="deleteTodo(todo.id)">Delete</button>
        </div>
      </div>
      
      <div class="edit popup" v-if="editingTodo">

        <form @submit.prevent="updateTodo">
          <div class="close" id="close-popup">
          ❌
          </div>
          <h3>Edit Todo</h3>
          <input v-model="editingTodo.title" placeholder="Todo Title" />
          <input v-model="editingTodo.description" placeholder="Todo Description" />
          <label>
            Completed:
            <input type="checkbox" v-model="editingTodo.completed" />
          </label>
          <button type="submit">Update</button>
        </form>
      </div>
    </div>
    </div>
</template>

<script>
import TodoService from '../TodoService';

export default {
  data() {
    return {
      todos: [],
      editingTodo: null
    };
  },
  created() {
    this.fetchTodos();
  },
  methods: {
    fetchTodos() {
      TodoService.getAllTodos().then(response => {
        this.todos = response.data;
      });
    },
    editTodo(todo) {
      this.editingTodo = { ...todo };
    },
    updateTodo() {
      TodoService.updateTodo(this.editingTodo).then(() => {
        this.fetchTodos();
        this.editingTodo = null;
      });
    },
    deleteTodo(todoId) {
      TodoService.deleteTodo(todoId).then(() => {
        this.fetchTodos();
      });
    },
    closePopup() {
      
    }
  }
};
</script>