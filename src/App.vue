<template>
  <h1 class="text-center mt-3">Vue 3 Todo List</h1>
  <div class="col-10 offset-1 col-md-8 offset-md-2 col-xl-6 offset-xl-3 p-0 my-4">
    <div class="d-flex">
      <input v-model="newTodo" @keyup.enter="onEnter" name="newTodo" type="text" class="col-10 col-xl-11 border-right-0">
      <span @click.prevent="addNewTodo" class="col-2 col-xl-1 bg-primary text-white rounded-right d-flex align-items-center justify-content-center m-0 p-0 pointer">Add</span>
    </div>
    <ul class="my-4 mx-0 p-0">
      <li v-for="(todo, index) in todos" :key="todo.id" class="todo d-flex border-dark rounded my-4 py-1 px-3">
        <div class="col-11 m-0 p-0">
          <h3 :class="{ done: todo.done }"> {{ todo.content }} </h3>
        </div>
        <div class="col-1 d-flex m-0 p-0 justify-content-between align-items-center">
          <span @click="toggleDone(todo)"><i class="far fa-check-circle text-success pointer"></i></span>
          <span @click="removeTodo(index)"><i class="far fa-trash-alt text-danger pointer"></i></span>
        </div>

      </li>
      <div class="d-flex justify-content-center">
        <button @click.prevent="toggleAll" class="btn btn-secondary">Clear Todos</button>
      </div>
    </ul>
  </div>
</template>

<script>
  import { ref } from 'vue';

export default {
  setup() {

    const newTodo = ref('');
    const todos = ref([]);

    const addNewTodo = () => {
      todos.value.push({
        id: Date.now(),
        done: false,
        content: newTodo.value,
      });
      newTodo.value = '';
    }

    const toggleDone = (todo) => {
      todo.done = !todo.done;
    }

    const removeTodo = (index) => {
      todos.value.splice(index, 1);
    }

    const toggleAll = () => {
      const totalTodos = todos.value.length;
      let completedTodos = 0;

      todos.value.forEach((todo) => {
        if (todo.done === true) {
          completedTodos++;
        }});

        todos.value.forEach((todo) => {
          if (completedTodos === totalTodos) {
            todo.done = false;
          } else {
            todo.done = true
          }
        })
    }

    const onEnter = () => {
      addNewTodo();
    }

    return {
      newTodo,
      todos,
      addNewTodo,
      toggleDone,
      removeTodo,
      toggleAll,
      onEnter,
    }
  }
}
</script>
