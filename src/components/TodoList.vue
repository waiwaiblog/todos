<template>
  <section class="main" v-show="todos.length" v-cloak>
    <input type="checkbox" id="toggle-all" class="toggle-all" :checked="allDone" :value="allDone" @change="onInput">
    <label for="toggle-all"></label>
    <draggable>
      <transition-group tag="ul" class="todo-list" name="todo-item">
        <li class="todo" v-for="todo of filteredTodos.slice().reverse()" :key="todo.id" :class="['todo-item', { completed: todo.completed, editing: todo == editedTodo}]">
          <todo-item :todo="todo" @remove-todo="removeTodo" @done="done" @editTodo="editTodo"></todo-item>
          <todo-edit :todo="todo" @done-edit="doneEdit" @cancel-edit="cancelEdit" :editedTodo="editedTodo"></todo-edit>
        </li>
      </transition-group>
    </draggable>
  </section>
</template>

<script>
  import TodoItem from './TodoItem';
  import TodoEdit from './TodoEdit';
  import draggable from 'vuedraggable'

  export default {
    name: 'TodoList',
    data() {
      return {
        editedTodo: null
      }
    },
    props: {
      todos: {
        type: Array,
      },
      filteredTodos: {
        type: Array,
      },
      allDone: {
        type: Boolean
      }
    },
    components: {
      TodoItem,
      TodoEdit,
      draggable
    },
    methods: {
      removeTodo(todo) {
        this.$emit('remove-todo', todo);
      },
      done(todo, completed) {
        this.$emit('done', todo, completed);
      },
      onInput() {
        this.$emit('allDone', !this.allDone);
      },
      editTodo(todo) {
        this.editedTodo = todo;
      },
      doneEdit(todoTitle) {
        if (!this.editedTodo) {
          return;
        }
        const title = todoTitle.trim();
        if (title) {
          this.editedTodo.title = title;
        } else {
          this.removeTodo(this.editedTodo);
        }

        this.editedTodo = null;
      },
      cancelEdit() {
        this.editedTodo = null;
      }
    }
  }
</script>


<style scoped>
  [v-cloak] {
    display: none;
  }
  .todo-item {
    transition: 1s;
  }
  .todo-item-enter, .todo-item-leave-to {
    opacity: 0;
    transform: translateX(200px);
  }
  .todo-item-leave-active {
    position: absolute;
  }
</style>