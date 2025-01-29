<template>
  <div class="flex flex-col items-center justify-center h-screen space-y-4">
    <app-input v-model="newTaskDescription"></app-input>
    <div class="relative overflow-x-auto">
      <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
        <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
          <tr>
            <th scope="col" class="px-6 py-3">
              Date
            </th>
            <th scope="col" class="px-6 py-3">
              Description
            </th>
            <th scope="col" class="px-6 py-3">
              Action
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="task in tasks" :key="task.id"
            class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 border-gray-200">
            <td class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
              {{ task.date }}
            </td>
            <td class="px-6 py-4">
              <div v-if="!task.isEditing" @click="editDescription(task)">
                {{ task.description }}
              </div>
              <input v-else v-model="task.description" @blur="saveDescription(task)"
                class="p-2 rounded bg-gray-200 w-full" />
            </td>
            <td class="px-6 py-4 flex space-x-4 justify-start">
              <button @click="editTask(task.id)" :disabled="task.status === 'complete'" :class="{
                'bg-yellow-500 hover:bg-yellow-600': task.status !== 'complete',
                'bg-gray-400 cursor-not-allowed': task.status === 'complete'
              }" class="flex items-center justify-center text-white p-2 rounded">
                <PencilIcon class="h-5 w-5" v-if="!task.isEditing" />
                <CloudArrowUpIcon class="h-5 w-5" v-else />
              </button>
              <button @click="markComplete(task.id)"
                :class="{ 'bg-green-500': task.status === 'complete', 'bg-blue-500': task.status === 'outstanding' }"
                class="flex items-center justify-center text-white p-2 rounded hover:bg-green-600">
                <CheckCircleIcon v-if="task.status === 'outstanding'" class="h-5 w-5" />
                <ArrowUturnLeftIcon v-else class="h-5 w-5" />
              </button>
              <button @click="deleteTask(task.id)"
                class="flex items-center justify-center bg-red-500 text-white p-2 rounded hover:bg-red-600">
                <TrashIcon class="h-5 w-5" />
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <p v-if="tasks.length > 0">Summary: {{ outstandingCount }} outstanding, {{ completedCount }} done</p>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue';
import AppInput from './AppInput.vue';
import { PencilIcon, CheckCircleIcon, TrashIcon, ArrowUturnLeftIcon, CloudArrowUpIcon } from '@heroicons/vue/24/outline';


const newTaskDescription = ref<string>("");

type TaskStatus = "outstanding" | "complete";

interface Task {
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

const deleteTask = (id: number) => {
  tasks.value = tasks.value.filter(task => task.id !== id);
};

const markComplete = (id: number) => {
  const task = tasks.value.find(task => task.id === id);
  if (task) {
    task.status = task.status === 'outstanding' ? 'complete' : 'outstanding';
  }
};

const editTask = (id: number) => {
  const task = tasks.value.find(task => task.id === id);
};

const editDescription = (task: Task) => {
  task.isEditing = true;
};

const saveDescription = (task: Task) => {
  task.isEditing = false;
};
</script>
