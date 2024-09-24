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
const emit = defineEmits([
  'check',
  'uncheck'
]);

const onChange = (e) => {
  if (e.currentTarget.checked) {
    return emit('check', e.currentTarget);
  }
  return emit('uncheck', e.currentTarget);
}
</script>