<template>
  <div class="flex flex-col items-center justify-center h-screen space-y-4">
    <app-input v-model="newTaskDescription"></app-input>
    <app-table v-model="tasks"></app-table>
    <p v-if="tasks.length > 0">Summary: {{ outstandingCount }} outstanding, {{ completedCount }} done</p>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue';
import AppInput from './AppInput.vue';
import AppTable from './AppTable.vue';


const newTaskDescription = ref<string>("");

export type TaskStatus = "outstanding" | "complete";

export interface Task {
  id: number,
  date: string,
  description: String,
  status: TaskStatus,
  urgent: boolean,
  isEditing: boolean,
}

const tasks = ref<Task[]>([])

let nextId = 1;

watch(newTaskDescription, (newTasks, oldTasks) => {
  const now = new Date();
  tasks.value?.push(
    {
      id: nextId++,
      date: new Intl.DateTimeFormat('en-GB', {
        hour: '2-digit',
        minute: '2-digit',
        day: '2-digit',
        month: '2-digit',
      }).format(now),
      description: newTaskDescription.value.trim(),
      status: "outstanding",
      urgent: false,
      isEditing: false,
    }
  )
}, { deep: true });

const outstandingCount = computed(() => tasks.value.filter(task => task.status === 'outstanding').length);

const completedCount = computed(() => tasks.value.filter(task => task.status === 'complete').length);

</script>
