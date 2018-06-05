<template>
  <div class="todo-list">
    <ul class="todos">
      <li v-if="todos.length === 0">No todos found</li>
      <li class="todo-item" v-for="todo in getFilterTodos()" v-on:click="toggleTodoStatus(todo)" v-bind:class="todo.status">
        {{ todo.desc }}
      </li>
    </ul>
    <form v-on:submit.prevent="addTodo">
      <input v-model="newTodoItem" />
      <button type="submit">Add</button>
    </form>
    <div class="filters">
      <a class="filter" href="#" v-for="filter in filters" v-on:click.prevent="changeFilter(filter.id)">{{ filter.label }}</a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ToDos',
  data () {
    return {
      newTodoItem: '',
      todos: [],
      filters: [
        {
          id: 'all',
          label: 'All'
        },
        {
          id: 'new',
          label: 'New'
        },
        {
          id: 'done',
          label: 'Done'
        }
      ],
      filter: 'all'
    }
  },
  methods: {
    addTodo () {
      this.todos = [...this.todos, {
        id: this.todos.length,
        desc: this.newTodoItem,
        status: 'new'
      }]
      this.newTodoItem = ''
    },
    toggleTodoStatus (todo) {
      this.todos = this.todos.map(item => {
        if (todo.id === item.id) {
          return {
            ...item,
            status: item.status === 'new' ? 'done' : 'new'
          }
        }
        return item
      })
    },
    changeFilter (currentFilter) {
      this.filter = currentFilter
    },
    getFilterTodos () {
      return this.todos.filter(item => item.status === this.filter || this.filter === 'all')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.done {
  text-decoration: line-through;
}
.filters {
  margin-top: 10px;
}
.filter {
  display: inline-block;
  margin-right: 10px;
}
</style>
