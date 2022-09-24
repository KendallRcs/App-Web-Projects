<template>
  <div class="wrapper">
    <h1>My Todo List</h1>
    <form @submit.prevent="addTodo">
      <input type="text" name="todo-text" v-model="newTodoText" placeholder="New todo">
    </form>
    <ul v-if="vtodos.length">
      <TodoItem v-for="todo in vtodos" :key="todo.id" :todo="todo" @remove="removeTodo"/>
    </ul>
    <p class="none" v-else>Nothing left in the list. Add a new todo in the input above.</p>
  </div>
</template>

<script>
// se importa TodoItem
import TodoItem from "./TodoItem.vue"

// por facilidad de crea el metodo como constante
let nextTodoId = 1

const createTodo = text => ({
  text,
  id: nextTodoId++
})

// se declara el componente TodoItem (vue)

export default {
  components: {
    TodoItem
  },
// creamos la data que se iniciara en la lista
  data() {
    return {
      vtodos: [
        createTodo("Learn Vue"),
        createTodo("Learn about single-file components"),
        createTodo("Ejemplo 01")
      ],

      newTodoText: ""
    }
  },

  methods: {
    addTodo() {
      // Obtenemos el valor de newTodoText
      const trimmedText = this.newTodoText.trim()

      // se valida que no este vacio y
      // se adiciona al arreglo vtodos, mediante el metodo push
      if (trimmedText) {
        this.vtodos.push(createTodo(trimmedText))
      }

      //Se borra el valor de newTodoText
      this.newTodoText = ""
    },

    removeTodo(item) {
      // filtramos para retirar el item igual a todo
      this.vtodos = this.vtodos.filter(todo => todo !== item)
    }
  }
}
</script>

<style lang="stylus">
*, *::before, *::after
  box-sizing border-box

html, body
  font 16px/1.2 BlinkMacSystemFont, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif
  padding 10px

.wrapper
  width 75%
  margin 0 auto

form
  margin-bottom 20px

input[type="text"]
  width 100%
  padding 10px
  border 1px solid #777

ul, li
  margin 0
  padding 0

p.none
  color #888
  font-size small
</style>

