<template>
  <label>
    <input type="checkbox">
    {{ title }}
  </label>
</template>

<script setup>
defineProps({
  title: String,
})
</script>