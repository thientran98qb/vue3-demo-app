<template>
  <div class="container w-[500px] mx-auto px-2">
    <div class="border shadow p-8 rounded">
      <h1 class="text-2xl font-bold underline">Todo App</h1>
      <div class="flex items-center">
        <input
          type="text"
          class="block flex-1 w-full px-3 py-2 bg-white border border-slate-300 rounded-md shadow-sm placeholder-slate-400
          focus:outline-none focus:border-pink-500"
          v-model="todoText"
          ref="inputTodoRef"
        />
        <button
          class="bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500 p-2 text-white my-2 ml-3 hover:bg-purple-600 transition-opacity rounded-sm font-bold hover:opacity-90"
          @click="addTodo"
          :disabled="todoText === ''"
        >
          Add Todo
        </button>
      </div>
      <ul class="p-0">
          <todo-item
            v-for="todo in todos"
            :todo="todo"
            :key="todo.id"
            @deleteItem="deleteItemTodo"
            @updateItem="updateItemTodo"
            @completed="completedTask"
          />
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref, toRefs } from '@vue/runtime-core';
import TodoItem from './components/TodoItem/TodoItem.vue';
import { TodoType } from './types/Todo';
import { v4 as uuidv4 } from "uuid";

export default defineComponent({
  components: {
    TodoItem
  },
  setup() {
    const inputTodoRef = ref<HTMLElement | null>(null)
    const state = reactive({
      todos : [
        {id: 1, title: "Go to shopping", completed: false},
        {id: 2, title: "Doing homework", completed: true},
        {id: 3, title: "Play badminton", completed: false}
      ] as Array<TodoType>,
      todoText: ''
    })
    const addTodo = () => {
      state.todos.push({
        id: uuidv4(),
        title: state.todoText,
        completed: false
      })
      state.todoText = ''
      inputTodoRef.value?.focus()
    }

    const deleteItemTodo = (id: number | string) => {
      state.todos.splice(state.todos.findIndex(todo => todo.id === id), 1);
    }

    const updateItemTodo = (id: number | string, textUpdate: string) => {
      state.todos = state.todos.map(todo => todo.id === id ? {
        ...todo,
        title: textUpdate
      } : todo)
    }

    const completedTask = (id: number | string) => {
      state.todos = state.todos.map(todo => todo.id === id ? {
        ...todo,
        completed: !todo.completed
      } : todo)
    }

    return {
      ...toRefs(state),
      addTodo,
      inputTodoRef,
      deleteItemTodo,
      updateItemTodo,
      completedTask
    }
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
