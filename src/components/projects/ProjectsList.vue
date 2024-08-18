<script setup>
import { defineProps, ref, computed, watch } from 'vue';

import ProjectItem from './ProjectItem.vue';

// props
const props = defineProps({
  user: Object,
});

// data
const enteredSearchTerm = ref('');
const activeSearchTerm = ref('');

// computed
const availableProjects = computed(() => {
  if (activeSearchTerm.value) {
    return props.user.projects.filter((prj) =>
      prj.title.includes(activeSearchTerm.value)
    );
  }
  return props.user.projects;
});

const hasProjects = computed(() => {
  return props.user.projects && availableProjects.value.length > 0;
});

// watch
watch(enteredSearchTerm, (val) => {
  setTimeout(() => {
    if (val === enteredSearchTerm.value) {
      activeSearchTerm.value = val;
    }
  }, 300);
});

watch(props.user, () => {
  enteredSearchTerm.value = '';
});

// methods
function updateSearch(val) {
  enteredSearchTerm.value = val;
}
</script>

<template>
  <base-container v-if="user">
    <h2>{{ user.fullName }}: Projects</h2>
    <base-search
      v-if="hasProjects"
      @search="updateSearch"
      :search-term="enteredSearchTerm"
    ></base-search>
    <ul v-if="hasProjects">
      <project-item
        v-for="prj in availableProjects"
        :key="prj.id"
        :title="prj.title"
      ></project-item>
    </ul>
    <h3 v-else>No projects found.</h3>
  </base-container>
  <base-container v-else>
    <h3>No user selected.</h3>
  </base-container>
</template>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
