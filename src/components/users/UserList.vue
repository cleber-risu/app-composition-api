<script setup>
import { computed, defineProps, ref, watch } from 'vue';

import UserItem from './UserItem.vue';

const props = defineProps(['users']);

const enteredSearchTerm = ref('');
const activeSearchTerm = ref('');

// watch
watch(enteredSearchTerm, (val) => {
  setTimeout(() => {
    if (val === enteredSearchTerm.value) {
      activeSearchTerm.value = val;
    }
  }, 300);
});

// methods
function updateSearch(val) {
  enteredSearchTerm.value = val;
}

function sort(mode) {
  sorting.value = mode;
}

// computed
const availableUsers = computed(() => {
  let users = [];
  if (activeSearchTerm.value) {
    users = this.users.filter((usr) =>
      usr.fullName.includes(this.activeSearchTerm)
    );
  } else if (props.users) {
    users = props.users;
  }
  return users;
});

const sorting = ref(null);
const displayedUsers = computed(() => {
  if (!sorting.value) {
    return availableUsers.value;
  }
  return availableUsers.value.slice().sort((u1, u2) => {
    if (sorting.value === 'asc' && u1.fullName > u2.fullName) {
      return 1;
    } else if (sorting.value === 'asc') {
      return -1;
    } else if (sorting.value === 'desc' && u1.fullName > u2.fullName) {
      return -1;
    } else {
      return 1;
    }
  });
});
</script>

<template>
  <base-container>
    <h2>Active Users</h2>
    <base-search
      @search="updateSearch"
      :search-term="enteredSearchTerm"
    ></base-search>
    <div>
      <button @click="sort('asc')" :class="{ selected: sorting === 'asc' }">
        Sort Ascending
      </button>
      <button @click="sort('desc')" :class="{ selected: sorting === 'desc' }">
        Sort Descending
      </button>
    </div>
    <ul>
      <user-item
        v-for="user in displayedUsers"
        :key="user.id"
        :user-name="user.fullName"
        :id="user.id"
        @list-projects="$emit('list-projects', $event)"
      ></user-item>
    </ul>
  </base-container>
</template>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
