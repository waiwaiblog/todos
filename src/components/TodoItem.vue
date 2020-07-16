<template>
    <div class="view">
        <input type="checkbox" class="toggle" :value="todo.completed" @input="onInput" :checked="todo.completed">
        <label @dblclick="editTodo">{{todo.title}}</label>
        <button class="destroy" @click="removeTodo(todo)"></button>
    </div>
</template>

<script>


  export default {
    name: 'TodoItem',
    props: {
      todo: {
        type: Object,
      },
    },
    methods: {
      removeTodo(todo) {
        this.$emit('remove-todo', todo);
      },
      onInput() {
        this.$emit('done', this.todo, !this.todo.completed);
      },
      editTodo() {
        this.$emit('editTodo', this.todo);
      }
    },
  }
</script>