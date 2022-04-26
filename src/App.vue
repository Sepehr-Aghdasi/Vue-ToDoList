<template>
      <div style="background-color: #4267b2" class="container w-100 p-3 rounded">
            <h1 class="text-white">ToDo List</h1>
            <label class="text-white">Add new task</label>
            <form @submit.prevent="addTodo()" class="row m-3">
                  <input v-model="newTodo" class="col-9 rounded p-3" type="text" />
                  <div class="col-3 d-flex justify-content-end p-0">
                        <button class="btn fw-bold rounded btn-info text-white">
                              Add new todo
                        </button>
                  </div>
            </form>
            <h3 class="d-flex m-3 pt-3">Task list :</h3>
            <ul>
                  <li
                        v-for="(todo, index) in todos"
                        :key="index"
                        class="my-2 d-flex justify-content-start align-items-center"
                  >
                        <label
                              @click="completedTodo(todo)"
                              :class="{ completed: todo.completed }"
                              :for="todo.id"
                              class="fw-bold fs-5"
                        >
                              <input
                                    type="checkbox"
                                    :checked="todo.completed"
                                    :name="todo.id"
                                    class="me-3"
                              />
                              {{ todo.content }}
                        </label>
                        <button @click="removeTodo(todo)" class="ms-3">Remove task</button>
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
ul li {
      list-style-type: none !important;
}
input[type="checkbox"] {
      width: 30px;
      height: 30px;
      border-radius: 50%;
      transition: box-shadow 0.3s;
      background: lightgrey;
      cursor: pointer;
      border: 0;
      appearance: none;
      -webkit-appearance: none;
}

input[type="checkbox"]:checked {
      box-shadow: inset 0 0 0 20px blue;
}
label {
      cursor: pointer;
      display: flex;
      align-items: center;
}

.completed {
      text-decoration: line-through !important;
}
</style>
