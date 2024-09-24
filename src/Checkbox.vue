<template>
  <label>
    <input type="checkbox" v-model="model">
    {{ title }}
  </label>
</template>

<script setup>
const model = defineModel();
const props = defineProps({
  title: String,
});
</script>