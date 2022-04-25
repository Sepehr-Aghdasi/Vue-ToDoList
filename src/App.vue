<template>
      <div style="background-color: #4267b2" class="container w-100 p-3 rounded">
            <h1 class="text-white">ToDo List</h1>
            <label class="text-white">Add new task</label>
            <form @submit.prevent="addTodo()" class="row m-3">
                  <input v-model="newTodo" class="col-9 rounded p-3" type="text" />
                  <div class="col-3 d-flex justify-content-end p-0">
                        <button class="btn rounded btn-info text-white">Add new todo</button>
                  </div>
            </form>
            <h3 class="d-flex m-3 pt-3">Task list :</h3>
            <ul>
                  <li v-for="(todo, index) in todos" :key="index">
                        <!-- <span> {{ index }} </span> -->
                        <input type="checkbox" :name="todo" />
                        <label
                              @click="completedTodo(todo)"
                              :class="{ completed: todo.completed }"
                              :for="todo"
                        >
                              {{ todo.content }}
                        </label>
                        <button @click="removeTodo(todo)">Remove task</button>
                  </li>
            </ul>
            <h4 v-if="todos.length === 0">Empty List</h4>
      </div>
</template>

<script>
import { ref } from "@vue/reactivity";
import "bootstrap/dist/css/bootstrap.min.css";
import "bootstrap";

export default {
      name: "App",
      setup() {
            const newTodo = ref("");
            const todosData = JSON.parse(localStorage.getItem("todos")) || [];
            const todos = ref(todosData);

            const addTodo = () => {
                  if (newTodo.value) {
                        todos.value.push({
                              id: Date.now(),
                              content: newTodo.value,
                              completed: false,
                        });
                        newTodo.value = "";
                  }
                  saveData();
            };

            const saveData = () => {
                  const data = JSON.stringify(todos.value);
                  localStorage.setItem("todos", data);
                  console.log(todos.value);
            };

            const completedTodo = (todo) => {
                  todo.completed = !todo.completed;
                  saveData();
            };

            const removeTodo = (index) => {
                  todos.value.splice(index, 1);
                  saveData();
            };

            return {
                  newTodo,
                  todos,
                  addTodo,
                  removeTodo,
                  completedTodo,
            };
      },
};
</script>

<style>
* {
      margin: 0;
      padding: 0;
      border: none;
      outline: none;
      text-decoration: none;
      list-style-type: none;
      box-sizing: border-box;
      text-transform: capitalize !important;
}
#app {
      font-family: Avenir, Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      color: #2c3e50;
      margin: 60px auto 0 auto;
      width: 50%;
}
/* form {
      display: flex;
      flex-direction: column;
      flex-wrap: wrap;
      align-content: space-around;
      justify-content: center;
      align-items: center;
} */
ul li {
      list-style-type: none !important;
}
.completed {
      text-decoration: line-through !important;
}
</style>
