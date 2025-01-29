<template>
    <form @submit.prevent="saveData" class="flex items-center space-x-4">
        <div class="flex-1">
            <input v-model="userInput" type="text" id="first_name" class=" 
            text-sm rounded-lg focus:border-blue-500 block w-full p-2.5 bg-gray-700 border-gray-600 placeholder-gray-400 
            text-white focus:ring-blue-500 " placeholder="Task Description" required>
        </div>
        <button type="submit"
            class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Save</button>
    </form>
</template>
  
<script setup lang="ts">
import { ref } from 'vue';

const taskDescription = defineModel<string>()

const userInput = ref<string>()

const saveData = () => {
    taskDescription.value = userInput.value;
    userInput.value = '';
};
</script>
  