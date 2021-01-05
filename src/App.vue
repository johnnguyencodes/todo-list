<template>
  <h1 class="text-center mt-3">Todo List</h1>
  <div class="col-10 offset-1 col-md-8 offset-md-2 col-xl-6 offset-xl-3 p-0 my-4">
    <div class="d-flex">
      <!-- v-model adds change listener to the input field, when there is a change, it automatically updates the ref assigned to newTodo -->
      <input v-model="newTodo" @keyup.enter="onEnter" name="newTodo" type="text" class="col-10 col-xl-11 border-right-0" placeholder="Add a new todo...">
      <!-- @click.prevent adds a click listener to the Add button while also calling event.preventDefault -->
      <button @click.prevent="addNewTodo" class="col-2 col-xl-1 bg-primary text-white rounded-right d-flex align-items-center justify-content-center m-0 p-0 pointer">Add</button>
    </div>
    <ul class="my-4 mx-0 p-0">
      <!-- v-for will repeat the li element for every item in the todos array-->
      <!-- v-bind sets the todos[i] property to the attribute, we can leave off v-bind and just use : instead -->
      <li v-for="(todo, index) in todos" :key="todo.id" class="d-flex border border-dark rounded my-4 py-1 px-3">
        <div class="col-lg-11 col-10 m-0 p-0">
          <!-- The class attribute is bounded to the done property of the todo.  In the UI, the todo will have the .done class applied if the todo's done property evaluates to true.
          If it evaluates to false, the .done class will not be applied  -->
          <h3 :class="{ done: todo.done }"> {{ todo.content }} </h3>
        </div>
        <div class="col-lg-1 col-2 d-flex m-0 p-0 justify-content-between align-items-center">
          <span @click="toggleDone(todo)"><i class="far fa-check-circle text-success pointer"></i></span>
          <span @click="removeTodo(index)"><i class="far fa-trash-alt text-danger pointer"></i></span>
        </div>

      </li>
      <div class="d-flex justify-content-center">
        <button @click.prevent="toggleAll" class="btn btn-secondary">Toggle All Todos</button>
      </div>
    </ul>
  </div>
</template>

<script>
// ref creates variables or properties that can respond to change and keeps track of changes to the input form and the array of todos.
  import { ref } from 'vue';

export default {
  setup() {

    // newTodo keeps track of the input field
    const newTodo = ref('');

    // todos is an array that keeps track of all todos entered from the input field
    const todos = ref([]);

    let todoIdTracker = 0;

    // addNewTodo is called when either the Enter Key is typed in the input field, or when the Add button is clicked.
    const addNewTodo = () => {
      todos.value.push({
        id: todoIdTracker,
        done: false,
        content: newTodo.value,
      });
      newTodo.value = '';
      todoIdTracker++;
    }

    const toggleDone = (todo) => {
      todo.done = !todo.done;
    }

    const removeTodo = (index) => {
      // splice will just remove that specific todo from the array
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
          // if all todos are done, then toggle them all as false
          if (completedTodos === totalTodos) {
            todo.done = false;
          // if not, then toggle them all as true
          } else {
            todo.done = true
          }
        })
    }

    const onEnter = () => {
      addNewTodo();
    }

    // returning variables and functions as properties within the return object will expose them to the template, gives access
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
.pointer {
  cursor: pointer;
}

.done {
  text-decoration: line-through;
}
</style>
