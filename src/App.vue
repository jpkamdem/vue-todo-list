<template>
  <form @submit.prevent="addTodo">
    <fieldset role="group">
      <input type="text" placeholder="Veuillez écrire vos tâches ici..." v-model="newTodo">
      <button>Ajouter</button>
    </fieldset>
  </form>
  <div v-if="todos.length === 0">Vous n'avez aucune tâche à effectuer.</div>
  <div v-else>
    <ul>
      <li v-for="todo in todos" :key="todo.date">
        {{ todo.title }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const todos = ref([]);
const newTodo = ref('');

const addTodo = () => {
  todos.value.push({
    title: newTodo.value.trim(),
    completed: false,
    date: 1
  });
  newTodo.value = '';
}
</script>

<style></style>