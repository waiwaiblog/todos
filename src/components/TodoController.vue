<template>
  <footer class="footer"
          v-show="todos.length"
          v-cloak>
    <span class="todo-count">
      <strong>{{id}}{{remaining}}</strong> {{ remaining | pluralize }} left
    </span>
    <ul class="filters">
<!--      <li><a href="#/all"-->
<!--             :class="{selected: visibility === 'all'}">All</a></li>-->
<!--      <li><a href="#/active"-->
<!--             :class="{selected: visibility === 'active'}">Active</a></li>-->
<!--      <li><a href="#/completed"-->
<!--             :class="{selected: visibility === 'completed'}">Completed</a></li>-->
      <li>
        <router-link active-class="selected"
                       to="/all">
            All
        </router-link>
      </li>
      <li>
        <router-link active-class="selected"
                       to="/active">
            Active
        </router-link>
      </li>
      <li>
        <router-link active-class="selected"
                       to="/completed">
            Completed
        </router-link>
      </li>
    </ul>
    <button class="clear-completed"
            v-show="todos.length > remaining"
            @click="removeCompleted">
      Clear completed
    </button>
  </footer>
</template>

<script>
  export default {
    name: 'TodoController',
    props: {
      todos: {
        type: Array,
      },
      remaining: {
        type: Number,
      },
      visibility: {
        type: String
      },
      id: {
        type: String
      }
    },
    filters: {
      pluralize(value) {
        return value <= 1 ? 'item' : 'items';
      }
    },
    methods: {
      removeCompleted() {
        this.$emit('removeCompleted');
      }
    }
  }
</script>