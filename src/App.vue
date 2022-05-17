<template>
  <div class="todos-section">
    <div class="w-full px-12">
      <legend class="section-title">Todos:</legend>
      <div class="input-section">
        <input type="text"
          placeholder="Water the plants" 
          class="my-4"
          v-model="newTodoTitle"
          @keydown.enter="createTodo"
        >
        <PaperAirplaneIcon 
          class="send-button"
          @click="createTodo"
        />
      </div>
    </div>
    
    <div id="todos-list" class="mt-4 border-neutral-700 divide-y divide-neutral-700 px-12 h-full overflow-y-auto">
      <div 
        v-for="todo in todos" 
        :key="todo.id" 
        class="relative flex items-start"
      >
        <todo-item
          :todo="todo"
          @updated-check="updatedCheck"
        />
      </div>
    </div>
  </div>
</template>1

<script>
import TodoItem from './components/TodoItem.vue';
import axios from 'axios';
import { PaperAirplaneIcon } from '@heroicons/vue/solid'

export default {
  name: "App",

  components: {
    TodoItem,
    PaperAirplaneIcon
  },

  data: function () {
    return {
      todos: [],
      apiUrl: process.env.VUE_APP_API_URL,
      newTodoTitle: '',
    }
  },

  created() {
    this.getTodos();    
  },

  methods: {
    updatedCheck(todo) {
      axios.put(this.apiUrl + todo.id, {
        'completed_at': todo.completed_at
      });
    },

    async getTodos() {
      const response = await fetch(this.apiUrl);
      this.todos = await response.json();
    },

    createTodo() {
      if (! this.newTodoTitle) {
        return;
      }

      const todo = {
        'title': this.newTodoTitle,
        'completed_at': null
      };

      this.todos.push(todo);

      axios.post(this.apiUrl, todo);

      this.newTodoTitle = '';

      this.scrollToBottom();
    },

    scrollToBottom() {
      const todosList = document.getElementById('todos-list');

      this.$nextTick(() => {
        todosList.scrollTo({
          top: todosList.scrollHeight,
          behavior: 'smooth'
        })
      })
    }
  }
};
</script>

<style>
  @import '../public/dist/style.css';

  input[type=text] {
    @apply text-lg shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md bg-neutral-700 text-white px-2 py-0.5
  };

  body {
    @apply bg-neutral-900 h-screen w-screen text-gray-100
  };

  #app {
    @apply flex items-center justify-center w-full h-full
  };

  .todos-section {
    @apply bg-neutral-800 rounded-3xl w-5/6 shadow-2xl h-1/2 py-12 flex flex-col
  };

  .section-title {
    @apply text-xl font-bold
  };

  .input-section {
    @apply flex flex-row gap-2 items-center
  };

  .send-button {
    @apply h-6 w-6 text-orange-500 rotate-90 block cursor-pointer sm:hidden
  };
</style>
