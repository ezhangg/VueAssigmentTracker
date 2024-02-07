<template>
  <div>
    <h2>Assignment Tracker</h2>
    <button @click="toggleCompleted">{{ showCompleted ? 'Hide Completed' : 'Show Completed' }}</button>
    <ul>
      <li v-for="assignment in filteredAssignments" :key="assignment.id">
        <h3>{{ assignment.title }}</h3>
        <p>Due Date: {{ assignment.dueDate }}</p>
        <p>Days Remaining: {{ calculateDaysRemaining(assignment.dueDate) }}</p>
        <p>Percent Complete: {{ assignment.percentComplete }}%</p>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import assignmentsData from './assignments.json';

const showCompleted = ref(true);

const filteredAssignments = computed(() => {
  if (showCompleted.value) {
    return assignmentsData;
  } else {
    return assignmentsData.filter(assignment => assignment.percentComplete < 100);
  }
});

const toggleCompleted = () => {
  showCompleted.value = !showCompleted.value;
};

const calculateDaysRemaining = (dueDate) => {
  const now = new Date();
  const due = new Date(dueDate);
  const timeDiff = due.getTime() - now.getTime();
  return Math.ceil(timeDiff / (1000 * 3600 * 24));
};
</script>