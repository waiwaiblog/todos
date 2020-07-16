<template>
  <input type="text" id="edit" class="edit" :value="todo.title" @input="onInput" @keypress.enter="doneEdit" @keyup.esc="cancelEdit" @blur="cancelEdit" v-todo-focus="todo == editedTodo">
</template>

<script>
  export default {
    name: 'TodoEdit',
    data() {
      return {
        editedTitle: null
      }
    },
    mounted() {
      this.editedTitle = this.todo.title;
    },
    methods: {
      onInput(e) {
        this.editedTitle = e.target.value;
      },
      doneEdit(e) {
        this.editedTitle = e.target.value;
        this.$emit('done-edit', this.editedTitle);
      },
      cancelEdit(e) {
        e.target.value = this.todo.title;
        this.$emit('cancel-edit');
      }
    },
    props: {
      todo: {
        type: Object
      },
      editedTodo: {
        type: Object
      }
    },
    directives: {
      ['todo-focus'](element, binding) {
        if(binding.value) {
          element.focus();
        }
      }
    }
  }
</script>