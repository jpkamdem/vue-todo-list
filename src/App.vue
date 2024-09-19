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
      <li v-for="todo in sortedTodos()" :key="todo.date" :class="{ completed: todo.completed }">
        <input type="checkbox" v-model="todo.completed">
        {{ todo.title }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const todos = ref([
  {
    title: 'Tâche de test',
    completed: true,
    date: 1
  },
  {
    title: 'Tâche de test 2',
    completed: false,
    date: 2
  }
]);
const newTodo = ref('');

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

const sortedTodos = () => {
  return todos.value.toSorted((a, b) => a.completed > b.completed);
}
</script>

<style>
.completed {
  opacity: .5;
  text-decoration: line-through;
}
</style>