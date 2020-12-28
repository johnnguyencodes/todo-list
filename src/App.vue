<template>
  <h1>Vue 3 Todo App</h1>
  <div>
    <label for="todo">New Todo</label>
    <input v-model="newTodo" @keyup.enter="onEnter" name="newTodo" type="text">
    <button @click.prevent="addNewTodo">Add New Todo</button>
    <button @click.prevent="toggleAll">Toggle All Todos</button>
  </div>
  <ul>
    <li v-for="(todo, index) in todos" :key="todo.id" class="todo">
      <h3 :class="{ done: todo.done }"> {{ todo.content }} </h3>
      <button @click="removeTodo(index)">Remove Todo</button>
      <button @click="toggleDone(todo)">Toggle Todo</button>
    </li>
  </ul>
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

<style>
body {
  font-family: sans-serif;
  padding-top: 1em;
  padding-bottom: 1em;
  font-size: 2em;
  width: 80%;
  margin: 0 auto;
}

input, textarea, button, p, div, section, article, select {
  display: 'block';
  width: 100%;
  font-family: sans-serif;
  font-size: 1em;
  margin: 0.5em;
}

button {
  cursor: pointer;
}
.done {
  text-decoration: line-through;
}
</style>
