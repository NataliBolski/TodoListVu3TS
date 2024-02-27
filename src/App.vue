<template>
  <AppHeader />
  <AppFilters :active-filter="activeFilter" @set-filter="setFilter" />

  <main class="app-main">
    <AppTodoList
      :todos="filtredTodo"
      @toggle-todo="toggleTodo"
      @remove-todo="removeTodo"
    />
    <AppAddTodo @add-todo="addTodo" />
  </main>

  <AppFooter :stats="stats" />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import AppHeader from "./components/AppHeader.vue";
import AppFilters from "./components/AppFilters.vue";
import AppTodoList from "./components/AppTodoList.vue";
import AppAddTodo from "./components/AppAddTodo.vue";
import AppFooter, { Stats } from "./components/AppFooter.vue";
import { Todo } from "@/types/Todo";
import { Filter } from "@/types/Filter";

interface State {
  todos: Todo[];
  activeFilter: Filter;
}
export default defineComponent({
  components: {
    AppHeader,
    AppFilters,
    AppTodoList,
    AppAddTodo,
    AppFooter,
  },
  data(): State {
    return {
      todos: [
        { id: 0, text: "Lear the basics of Vue", completed: true },
        { id: 2, text: "Learn the basics of Typescript", completed: false },
        { id: 1, text: "Subscribe to the channel", completed: false },
      ],
      activeFilter: "All",
    };
  },
  computed: {
    filtredTodo(): Todo[] {
      switch (this.activeFilter) {
        case "Active":
          return this.activeTodo;
        case "Done":
          return this.doneTodo;
        case "All":
        default:
          return this.todos;
      }
    },
    stats(): Stats{
      return {
        active: this.activeTodo.length,
        done: this.doneTodo.length,
      }
    },
    activeTodo(): Todo[]{
      return this.todos.filter(todo => !todo.completed)
    },
    doneTodo(): Todo[]{
      return this.todos.filter(todo => todo.completed)
    }
  },
  methods: {
    addTodo(todo: Todo) {
      this.todos.push(todo);
    },
    toggleTodo(id: number) {
      const targetTodo = this.todos.find((todo: Todo) => todo.id === id);

      if (targetTodo) {
        targetTodo.completed = !targetTodo.completed;
      }
    },
    removeTodo(id: number) {
      const targetTodo = this.todos.find((todo: Todo) => todo.id === id);

      if (targetTodo) {
        this.todos = this.todos.filter((todo: Todo) => todo.id !== id);
      }
    },
    setFilter(filter: Filter) {
      this.activeFilter = filter;
    },
  },
});
</script>
