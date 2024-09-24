<template>
  <form @submit.prevent="addTodo">
    <fieldset role="group">
      <!-- v-model permet d'assigner un champ input à une variable réactive (ref) -->
      <input type="text" placeholder="Veuillez écrire vos tâches ici..." v-model="newTodo" />
      <!-- Vérification de la valeur du champ de texte, pour éviter les espaces -->
      <button :disabled="newTodo.trim() === ''">Ajouter</button>
    </fieldset>
  </form>
  <!-- Affichage conditionnel, soit l'un soit l'autre -->
  <div v-if="todos.length === 0">Vous n'avez aucune tâche à effectuer.</div>
  <div v-else>
    <ul>
      <!-- Boucle simple, classe dynamique vérifiant la valeur de la propriété completed -->
      <li v-for="todo in sortedTodos" :key="todo.date" :class="{ completed: todo.completed }" @click="deleteTodo(todo)">
        <label>
          <!-- click.stop permet d'empêcher la fonction deleteTodo de fonctionner sur la checkbox -->
          <input type="checkbox" v-model="todo.completed" @click.stop />
          {{ todo.title }}
        </label>
      </li>
    </ul>
    <label>
      <input type="checkbox" v-model="hideCompleted" />
      Masquer les éléments
    </label>
    <p v-if="remainingTodos > 0">
      {{ remainingTodos }} tâche{{ remainingTodos > 1 ? 's' : '' }} à faire
    </p>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const todos = ref([
  {
    title: 'Tâche à faire 1',
    completed: true,
    date: 1
  },
  {
    title: 'Tâche à faire 2',
    completed: false,
    date: 2
  }
]);
const newTodo = ref("");
const hideCompleted = ref(false);

const addTodo = () => {
  if (newTodo.value.trim() === "") { // vérification de la valeur du champ de texte, comme au-dessus
    return; 
  }
  todos.value.push({
    title: newTodo.value.trim(),
    completed: false,
    date: Date.now(),
  });
  newTodo.value = ""; // Réinitialisation du champ de texte
};

const deleteTodo = (todo) => {
  todos.value = todos.value.filter((t) => t.date !== todo.date); // Date.now() est unique à chaque tâche, même celle avec le même nom, c'est plus précis
};

const sortedTodos = computed(() => { // optimisation grâce au computed, ne recalcule pas toute la fonction à chaque fois, uniquement lorsqu'une des valeurs dont l'objet dépend (ici todos.value & hideCompleted.value)
  const sortedTodos = todos.value.toSorted((a, b) => a.completed > b.completed);
  if (hideCompleted.value) {
    return sortedTodos.filter((t) => t.completed === false);
  }
  return sortedTodos;
});

const remainingTodos = computed(() => {
  return todos.value.filter(t => t.completed === false).length;
})
</script>

<style>
.completed {
  opacity: 0.5;
  text-decoration: line-through;
}
</style>
