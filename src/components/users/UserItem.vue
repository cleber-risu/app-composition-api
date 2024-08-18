<script setup>
import { defineProps, defineEmits } from 'vue';

// props
const props = defineProps(['id', 'userName']);

// emits
const emit = defineEmits(['list-projects']);

// methods
function viewProjects() {
  emit('list-projects', props.id);
}
</script>

<template>
  <li>
    <h3>{{ userName }}</h3>
    <button @click="viewProjects">View Projects</button>
  </li>
</template>

<style scoped>
li {
  margin: 0.5rem 0;
  padding: 1rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.26);
  border: 1px solid #ccc;
}

li h3 {
  margin: 0;
}
</style>
