<template>
  <div class="content">
    <form @submit.prevent="addTodo">
      <h2>Add Todo</h2>
      <input v-model="todo.title" placeholder="Todo Title" />
      <input v-model="todo.description" placeholder="Todo Description" />
      <label>
        Completed:
        <input type="checkbox" v-model="todo.completed" />
      </label>
      <button type="submit">Add</button>
    </form>
  </div>
</template>

<script>
import TodoService from '../TodoService';
export default {
  data() {
    return {
      todo: {
        title: '',
        description: '',
        completed: false
      }
    };
  },
  methods: {
    addTodo() {
      TodoService.createTodo(this.todo).then(() => {
        this.todo.title = '';
        this.todo.description = '';
        this.todo.completed = false;
        this.$router.push('/')
      });
    }
  }
};
</script>