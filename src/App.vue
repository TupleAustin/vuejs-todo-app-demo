<template>
  <div id="app">
    <h1>TodoApp</h1>
    <input type='text' placeholder='New todo...' v-model='newTodoText' />
    <button v-on:click='addTodo' v-bind:disabled='newTodoText.length === 0'>Add Todo</button>
    <ul>
      <li v-for='todo in todos' v-bind:class='{ "done": todo.done }'>
        <input type='checkbox' v-model='todo.done' />
        {{ todo.text }}
      </li>
    </ul>
    <button v-if='anyDone' @click='clearDoneItems'>Clear done items</button>
  </div>
</template>

<script>
class TodoItem {
  constructor(todoText) {
    this.text = todoText
    this.done = false
  }
}

export default {
  name: 'app',
  data () {
    return {
      newTodoText: '',
      todos: []
    }
  },
  computed: {
    anyDone () {
      return !!this.todos.filter((todo) => todo.done).length
    }
  },
  methods: {
    addTodo () {
      this.todos.push(new TodoItem(this.newTodoText))
      this.newTodoText = ''
    },
    clearDoneItems () {
      this.todos = this.todos.filter((todo) => !todo.done)
    }
  },
  watch: {
    todos: {
      deep: true,
      handler () {
        localStorage.setItem('todos', JSON.stringify(this.todos))
      }
    }
  },
  created () {
    const todos = localStorage.getItem('todos')
    this.todos = (todos ? JSON.parse(todos) : [])
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

ul li {
  display: block;
}

ul li.done {
  text-decoration: line-through;
  color: #999;
}
</style>
