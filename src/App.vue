<template>
  <section id="app" class="todoapp">

    <header class="header">
      <transition appear name="todo-head">
        <h1>todos</h1>
      </transition>
      <todo-search :value="searchText" @input="searchText = $event"></todo-search>
      <todo-input @add-todo="addTodo"></todo-input>
    </header>
    <draggable>
      <todo-list :todos="todos" :filtered-todos="filteredTodos" @remove-todo="removeTodo" @done="done" :allDone="allDone" @allDone="onAllDone"></todo-list>
    </draggable>
    <todo-controller :remaining="remaining" :todos="todos" :visibility="visibility" @removeCompleted="removeCompleted"></todo-controller>
  </section>
</template>


<script>
  import TodoInput from './components/TodoInput';
  import TodoList from './components/TodoList';
  import TodoController from "./components/TodoController";
  import TodoSearch from "./components/TodoSearch";

  const STORAGE_KEY = 'todos-vuejs-2.6';
  const todoStorage = {
    fetch() {
      const todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
      todos.forEach(function(todo, index) {
        todo.id = index;
      });
      todoStorage.uid = todos.length;
      return todos;
    },
    save(todos) {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
    }
  }
  const filters = {
    all(todos, searchText) {
      return todos.filter( todo => todo.title.indexOf(searchText) !== -1);
    },
    active(todos, searchText) {
      return todos.filter((todo) =>
        !todo.completed && todo.title.indexOf(searchText) !== -1
      );
    },
    completed(todos, searchText) {
      return todos.filter((todo) =>
      todo.completed && todo.title.indexOf(searchText) !== -1
    );
    }
  }
  export default {
    name: 'app',
    components: {
      TodoController,
      TodoInput,
      TodoList,
      TodoSearch,
    },
    data() {
      return {
        todos: todoStorage.fetch(),
        visibility: 'all',
        show: true,
        searchText: '',
      }
    },
    computed: {
      filteredTodos() {
        return filters[this.visibility](this.todos, this.searchText);
      },
      remaining() {
        const todos = filters.active(this.todos, this.searchText);
        return todos.length;
      },
      allDone: {
        get() {
          return this.remaining === 0;
        },
        set(value) {
          this.todos.forEach(todo => todo.completed = value);
        }
      }
    },
    methods: {
      addTodo(todoTitle) {
        const newTodo = todoTitle && todoTitle.trim();
        if (!newTodo) return;
        this.todos.push({
          id: todoStorage.uid++,
          title: newTodo,
          completed: false
        });
        this.visibility = 'all';
        // todoStorage.save(this.todos);
      },
      removeTodo(todo) {
        const number = this.todos.indexOf(todo, 0);
        this.todos.splice(number, 1);
        // todoStorage.save(this.todos);
      },
      done(todo, completed) {
        todo.completed = completed;
      },
      // onHashChange() {
      //   const visibility = window.location.hash.replace(/#\/?/, '');
      //   this.visibility = visibility;
      // },
      onHashChange() {
        const visibility = this.$route.params.id;
        this.visibility = visibility;
      },
      onAllDone(done) {
        this.allDone = done;
      },
      removeCompleted() {
        this.todos = filters.active(this.todos, this.searchText);
      },

    },
    watch: {
      todos: {
        handler(todos) {
          todoStorage.save(todos);
        },
        deep: true
      },
      $route() {
        console.log('監視中です');
        this.onHashChange();
      }
    }
  }
</script>


<style scoped>
  @import url("https://unpkg.com/todomvc-app-css@2.2.0/index.css");

  .todo-head-enter-active {
    transition: 3s;
  }
  .todo-head-enter {
    opacity: 0;
    transform: translateY(-40px);
  }

</style>