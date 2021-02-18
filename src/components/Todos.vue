<template>
  <section class="todoapp">
    <header class="header">
      <h1>Todos</h1>
      <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
    </header>
    <div class="main">
      <input id="toggle-all" type="checkbox" class="toggle-all" v-model="allDone">
      <label for="toggle-all">Marquer comme fait</label>
      <ul class="todo-list">
        <li class="todo" v-for="todo in filteredTodos" :key=todo :class="{completed: todo.completed}">
          <div class="view">
            <input id="todo" type="checkbox" class="toggle" v-model="todo.completed">
            <label for="todo">{{ todo.name }}</label>
            <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
          </div>
        </li>
      </ul>
    </div>
    <footer class="footer" v-show="hasTodo">
      <span class="todo-count"><strong>{{ remaining }}</strong> tâches à faire</span>
      <ul class="filters">
        <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
        <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
        <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faite</a></li>
      </ul>
      <button class="clear-completed" @click.prevent="deleteCompleted" v-show="done">Effacer</button>
    </footer>
  </section>
</template>

<script>
export default {
  data () {
    return {
      todos: [{
        name: 'Tâche de test',
        completed: false
      }],
      newTodo: '',
      filter: 'all'
    }
  },
  methods: {
    addTodo () {
      this.todos.push({
        completed: false,
        name: this.newTodo
      })
      this.newTodo = ''
    },
    deleteTodo (todo) {
      this.todos = this.todos.filter(element => element !== todo)
    },
    deleteCompleted () {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  },
  computed: {
    allDone: {
      get () {
        return this.remaining === 0
      },
      set (value) {
        this.todos.forEach(todo => {
          todo.completed = value
        })
      }
    },
    hasTodo () {
      return this.todos.length > 0
    },
    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    },
    done () {
      return this.todos.filter(todo => todo.completed)
    },
    filteredTodos () {
      if (this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    }
  }
}
</script>

<style src="./todos.css">

</style>
