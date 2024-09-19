<template>
<div v-if="todos.length === 0">Vous n'avez aucune tâche à effectuer.</div>
<form @submit.prevet="addTodo">
  <fieldset role="group">
    <input type="text" placeholder="Veuillez écrire vos tâches ici...">
    <button>Ajouter</button>
  </fieldset>
</form>
</template>

<script setup>
import { ref } from 'vue';

const todos = ref([]);
</script>

<style></style>