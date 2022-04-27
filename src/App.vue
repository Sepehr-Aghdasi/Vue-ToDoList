<template>
      <div class="container w-100 p-3 rounded">
            <h1>ToDo List</h1>
            <label>Add new task</label>
            <form @submit.prevent="addTodo()" class="row m-3">
                  <input
                        v-model="newTodo"
                        class="col-9 border rounded p-3"
                        placeholder="Enter A Task"
                        type="text"
                  />
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
                        class="todo-item my-2 d-flex justify-content-between align-items-center"
                  >
                        <div class="d-flex">
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
                        </div>
                        <div class="mx-3 d-flex align-item-center" v-if="todo.isEditing">
                              <input
                                    v-model="newTodo"
                                    class="col-8 border rounded p-1"
                                    placeholder="Edit Task"
                                    type="text"
                              />
                              <button class="btn btn-danger" @click="todo.isEditing = false">
                                    Submit
                              </button>
                        </div>
                        <div>
                              <button @click="removeTodo(todo)" class="btn ms-3 text-danger">
                                    <svg
                                          xmlns="http://www.w3.org/2000/svg"
                                          width="26"
                                          height="26"
                                          fill="currentColor"
                                          class="bi bi-x-circle"
                                          viewBox="0 0 16 16"
                                    >
                                          <path
                                                d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"
                                          />
                                          <path
                                                d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z"
                                          />
                                    </svg>
                              </button>
                              <button @click="editTodo(todo, todo.id)" class="btn ms-3 text-danger">
                                    <span>Edit</span>
                              </button>
                        </div>
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
                              isEditing: false,
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
                  console.log(index);
                  saveData();
            };

            const editTodo = (todo, todoId) => {
                  todo.isEditing = !todo.isEditing;
                  console.log(todo);

                  for (let i in todos.value) {
                        if (todos.value[i].id === todoId ) {
                              todos.value[i].content = newTodo.value;

                              break; //Stop this loop, we found it!
                        }
                  }

                  console.log(todos.value);
                  // https://stackoverflow.com/questions/4689856/how-to-change-value-of-object-which-is-inside-an-array-using-javascript-or-jquer
            };

            console.log(todos.value);

            return {
                  newTodo,
                  todos,
                  addTodo,
                  completedTodo,
                  removeTodo,
                  editTodo,
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
ol,
ul {
      padding-left: 2rem !important;
      padding-right: 2rem !important;
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
.todo-item {
      border: 1px solid #ddd;
      border-radius: 5px;
      padding: 15px;
}
</style>
