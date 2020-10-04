<template>
  <h1>Vue 3 Todo App</h1>
  <form @submit.prevent="addNewTodo">
    <label>New Todo</label>
    <input v-model="newTodo" name="newTodo">
    <button>Add New Todo</button>
  </form>
  <button @click="markAllDone()">Mark All As Done</button>
  <button @click="deleteAll()">Delete All</button>
  <button @click="deleteSelected()">Deleted Selected</button>
  <div>
    <ol> 
      <li v-for="(todo, index) in todos" :key="todo.id" class="todo">
        <input type="checkbox" @click="addToSelected(index)">
        <h3 @click="toggleDone(todo)" :class="{ done: todo.done }">{{todo.content}}</h3>
        <button @click="removeTodo(index)">Remove Todo</button>
      </li>
    </ol>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup(){
    const newTodo = ref('');
    const todos = ref([]);
    const selected = ref([]);

    function addNewTodo(){ 
      todos.value.push({
        content: newTodo.value,
        id: Date.now(),
        done: false,
      });
      newTodo.value = '';
    }
    function toggleDone(todo){
      todo.done = !todo.done;
    }
    function removeTodo(index){
      todos.value.splice(index, 1);
    }
    function markAllDone(){
      todos.value.forEach(todo => { 
        if(todo.done !== true){
          todo.done = true;
        }
      })
    }
    function deleteAll(){
      todos.value = [];
    }
    function addToSelected(index){
      selected.value.push(index);
      console.log(selected.value);
    }
    function deleteSelected(){
      selected.value.forEach(index => {
        if(index > 0){
          todos.value.splice(index-1, 1);  
        }else{
          todos.value.splice(index, 1);
        }
      })
    }

    return {
      addNewTodo,
      newTodo,
      todos,
      toggleDone,
      removeTodo,
      markAllDone,
      deleteAll,
      addToSelected,
      deleteSelected
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
