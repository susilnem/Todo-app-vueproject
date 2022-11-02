<template>
  <div id="app" class="w-3/5 mx-auto p-4 bg-gray-800 rounded-md m-4 relative overflow-hidden pt-14 mt-20">
    <div class="w-full absolute bg-purple-600 top-0 right-0 left-0 text-center text-white font-semibold text-x1 py-2">
      <h1>Main page of Todo</h1>
    </div>
    <form action="" class="border-b-2 border-purple-200 py-4">
      <div class="flex items-center">
        <label class="text-white text-lg pr-2">Todo Name: </label>
        <input v-model="newTodo" type="text" class="flex-1 px-2 py-1 bg-gray-300 rounded-sm text-gray-800" />
      </div>
      <div class="mt-5 flex items-center justify-between">
        <div>
          <!-- add todo button  -->
          <button @click.prevent="addNewTodo()"
            class="px-4 py-2 text-xm font-semibold text-white bg-purple-800 rounded-sm focus:outline-none hover:bg-purple-500 focus:ring-4 ring-purple-800 ring-opacity-25 transition duration-100 ease-in-out transform hover:scale-105">
            Add Todo
          </button>
          <!-- edit button  -->
          <button v-show="hide"
            class="ml-3 px-4 py-2 text-xm font-semi bold text-white bg-purple-800 rounded-sm focus:outline-none hover:bg-purple-500 focus:ring-4 ring-purple-800 ring-opacity-25 transition duration-100 ease-in-out transform hover:scale-105"
            @click.prevent="editTodo(newedited)">
            Edit Todo
          </button>
        </div>
        <!-- tick  button  -->
        <button class="p-3 rounded-full focus:outline-none hover:bg-purple-600 hover:bg-opacity-25 mr-3"
          @click="allCompleteTodo()">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="w-6 h-6 fill-current text-purple-500">
            <path fill-rule="evenodd"
              d="M2.25 12c0-5.385 4.365-9.75 9.75-9.75s9.75 4.365 9.75 9.75-4.365 9.75-9.75 9.75S2.25 17.385 2.25 12zm13.36-1.814a.75.75 0 10-1.22-.872l-3.236 4.53L9.53 12.22a.75.75 0 00-1.06 1.06l2.25 2.25a.75.75 0 001.14-.094l3.75-5.25z"
              clip-rule="evenodd" />
          </svg>
        </button>
      </div>
    </form>

    <div id="todos" class="space-y-4 my-5">
      <div v-for="todo in todos" :key="todo.id" @click="todoCompleted(todo)" id="todo"
        class="py-4 px-2 rounded-lg bg-gray-300 cursor-pointer transition duration-120 ease-in-out transform hover:scale-100 flex items-center justify-between"
        :class="{ 'bg-red-600 line-through text-white': todo.isCompleted }">
        <h1>{{ todo.text }}</h1>
        <div class="flex items-center">
          <!-- edit button  -->
          <span class="p-3 rounded-full focus:outline-none hover:bg-purple-600 hover:bg-opacity-30 hover:scale-110"
            @click="hidebutton(todo); ">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-6 h-6">
              <path
                d="M21.731 2.269a2.625 2.625 0 00-3.712 0l-1.157 1.157 3.712 3.712 1.157-1.157a2.625 2.625 0 000-3.712zM19.513 8.199l-3.712-3.712-12.15 12.15a5.25 5.25 0 00-1.32 2.214l-.8 2.685a.75.75 0 00.933.933l2.685-.8a5.25 5.25 0 002.214-1.32L19.513 8.2z" />
            </svg>
          </span>
          <!-- delete button -->
          <span class="p-3 rounded-full focus:outline-none hover:bg-purple-600 hover:bg-opacity-30 hover:scale-110"
            :class="{ '': !todo.isCompleted }" @click="deleteTodo(todo)">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="w-6 h-6 fill-current">
              <path fill-rule="evenodd"
                d="M16.5 4.478v.227a48.816 48.816 0 013.878.512.75.75 0 11-.256 1.478l-.209-.035-1.005 13.07a3 3 0 01-2.991 2.77H8.084a3 3 0 01-2.991-2.77L4.087 6.66l-.209.035a.75.75 0 01-.256-1.478A48.567 48.567 0 017.5 4.705v-.227c0-1.564 1.213-2.9 2.816-2.951a52.662 52.662 0 013.369 0c1.603.051 2.815 1.387 2.815 2.951zm-6.136-1.452a51.196 51.196 0 013.273 0C14.39 3.05 15 3.684 15 4.478v.113a49.488 49.488 0 00-6 0v-.113c0-.794.609-1.428 1.364-1.452zm-.355 5.945a.75.75 0 10-1.5.058l.347 9a.75.75 0 101.499-.058l-.346-9zm5.48.058a.75.75 0 10-1.498-.058l-.347 9a.75.75 0 001.5.058l.345-9z"
                clip-rule="evenodd" />
            </svg>
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
const newTodo = ref("");
var newedited = ref("");
const todos = ref([]);
let hide = ref(false);
onMounted(() => {
  if (localStorage.getItem("mainTodos")) {
    let mainTodos = JSON.parse(localStorage.getItem("mainTodos"));
    mainTodos.forEach((todo) => {
      todos.value.push(todo);
    });
  }
});

function addNewTodo() {
  if (newTodo.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: newTodo.value,
      isCompleted: false,
    });

    newTodo.value = "";
    localStorage.setItem("mainTodos", JSON.stringify(todos.value));
  }
}
function todoCompleted(todo) {
  todo.isCompleted = !todo.isCompleted;
  localStorage.setItem("mainTodos", JSON.stringify(todos.value));
}

function hidebutton(todo) {
  hide.value = !hide.value;
  newTodo.value = todo.text;
  newedited.value = todo.id;
}
function editTodo(newedited) {

  newedited = todos.value.find((item) => item.id === newedited)
  newedited.text = newTodo.value;
  newedited.isCompleted = false

}

function deleteTodo(todo) {
  todos.value = todos.value.filter((item) => {
    return item != todo;
  });
  localStorage.setItem("mainTodos", JSON.stringify(todos.value));
}
function allCompleteTodo(todo) {
  todos.value.filter((todo) => {
    todo.isCompleted = true;
    localStorage.setItem("mainTodos", JSON.stringify(todos.value));
  });
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

* {
  font-family: "Poppins", sans-serif;
}
</style>
