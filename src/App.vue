<template>
  <h1>Vue 3 Todo App</h1>
  <div>
    <label for="todo">New Todo</label>
    <input v-model="newTodo" @keyup.enter="onEnter" name="newTodo" type="text">
    <button @click.prevent="addNewTodo">Add New Todo</button>
  </div>
  <ul>
    <li v-for="(todo, index) in todos" :key="todo.id" class="todo">
      <h3 :class="{ done: todo.done }" @click="toggleDone(todo)"> {{ todo.content }} </h3>
      <button @click="removeTodo(index)">Remove Todo</button>
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

    const onEnter = () => {
      addNewTodo();
    }

    return {
      newTodo,
      todos,
      addNewTodo,
      toggleDone,
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

.todo {
  cursor: pointer;
}
.done {
  text-decoration: line-through;
}
</style>
