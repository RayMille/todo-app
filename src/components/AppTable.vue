<template>
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
</template>
  
<script setup lang="ts">
import { PencilIcon, CheckCircleIcon, TrashIcon, ArrowUturnLeftIcon, CloudArrowUpIcon } from '@heroicons/vue/24/outline';
import type { Task } from './AppTodoList.vue';

const tasks = defineModel<Task[]>()

const deleteTask = (id: number) => {
    tasks.value = tasks?.value?.filter(task => task.id !== id);
};

const markComplete = (id: number) => {
    const task = tasks?.value?.find(task => task.id === id);
    if (task) {
        task.status = task.status === 'outstanding' ? 'complete' : 'outstanding';
    }
};

const editTask = (id: number) => {
    const task = tasks?.value?.find(task => task.id === id);
};

const editDescription = (task: Task) => {
    task.isEditing = true;
};

const saveDescription = (task: Task) => {
    task.isEditing = false;
};

</script>