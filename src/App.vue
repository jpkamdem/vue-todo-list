<template>
  <form @submit.prevent="addTodo">
    <fieldset role="group">
      <input type="text" placeholder="Veuillez écrire vos tâches ici..." v-model="newTodo">
      <button :disabled="newTodo.trim() === ''">Ajouter</button>
    </fieldset>
  </form>
  <div v-if="todos.length === 0">Vous n'avez aucune tâche à effectuer.</div>
  <div v-else>
    <ul>
      <li v-for="todo in sortedTodos()" :key="todo.date" :class="{ completed: todo.completed }" @click="deleteTodo(todo)">
        <input type="checkbox" v-model="todo.completed">
        {{ todo.title }}
      </li>
    </ul>
    <label>
      <input type="checkbox" v-model="hideCompleted">
      Masquer les éléments
    </label>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const todos = ref([]);
const newTodo = ref('');
const hideCompleted = ref(false);

const addTodo = () => {
  if (newTodo.value.trim() === '') {
    return;
  }
  todos.value.push({
    title: newTodo.value.trim(),
    completed: false,
    date: Date.now(),
  });
  newTodo.value = '';
}

const deleteTodo = (todo) => {
  todos.value = todos.value.filter(t => t.date !== todo.date);
}

const sortedTodos = () => {
    const sortedTodos = todos.value.toSorted((a, b) => a.completed > b.completed);
    if (hideCompleted.value) {
      return sortedTodos.filter(t => t.completed === false);
    }
    return sortedTodos;
}
</script>

<style>
.completed {
  opacity: .5;
  text-decoration: line-through;
}
</style>