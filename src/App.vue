<template>
      <div class="container w-100 p-3 rounded">
            <h1>ToDo List</h1>
            <Todo />
      </div>
</template>

<script>
import Todo from "./components/Todo-List.vue";

export default {
      name: "App",
      components: {
            Todo,
      },
};
</script>

<style></style>
