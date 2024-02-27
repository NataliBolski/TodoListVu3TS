<template>
  <ul class="todo-list">
    <AppTodoItem v-for="todo in todos" :key="todo.id" :todo="todo" @toggle-todo="toggleTodo" @remove-todo="removeTodo" />
  </ul>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import AppTodoItem from "./AppTodoItem.vue";
import { Todo } from "../types/Todo";

interface State {
  todos: Todo[];
}

export default defineComponent({
  components: {
    AppTodoItem,
  },
  data(): State {
    return {
      todos: [
        { id: 0, text: "Lear the basics of Vue", completed: true },
        { id: 2, text: "Learn the basics of Typescript", completed: false },
        { id: 1, text: "Subscribe to the channel", completed: false },
      ],
    };
  },
  methods: {
    toggleTodo(id: number){
      const targetTodo = this.todos.find((todo: Todo) => todo.id === id)

      if(targetTodo) {
        targetTodo.completed = !targetTodo.completed
      }
    },
    removeTodo(id: number){
      const targetTodo = this.todos.find((todo: Todo) => todo.id === id)

      if(targetTodo){
        this.todos = this.todos.filter((todo: Todo) => todo.id !== id)
      }

    }
  }
});
</script>
