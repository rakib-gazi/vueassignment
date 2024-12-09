<script setup>
import { ref, computed } from 'vue';

const tasks = ref(JSON.parse(localStorage.getItem('tasks')) || []);
const newTask = ref('');
const showForm = ref(false);

const addTask = () => {
  if (newTask.value.length >= 3) {
    tasks.value.push({ name: newTask.value, completed: false });
    saveTasksToLocalStorage();
    newTask.value = '';
  } else {
    alert('Task name must be at least 3 characters long.');
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
  saveTasksToLocalStorage();
};

const toggleTaskCompletion = (index) => {
  tasks.value[index].completed = !tasks.value[index].completed;
  saveTasksToLocalStorage();
};

const toggleForm = () => {
  showForm.value = !showForm.value;
};

const saveTasksToLocalStorage = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
};


const completedTasks = computed(() =>
  tasks.value.filter((task) => task.completed)
);


const getTaskClasses = (task) => {
  return task.completed
    ? 'bg-green-100 border-2 border-red-500'
    : 'bg-white border-2 border-blue-500';
};
</script>

<template>
  <div class="space-y-6 p-6">
   
    <button
      class="w-full px-4 py-2 text-white bg-cyan-950 rounded-md font-semibold hover:bg-blue-600"
      @click="toggleForm"
    >
      {{ showForm ? "Hide Add Task Form" : "Show Add Task Form" }}
    </button>

    
    <form
      v-if="showForm"
      @submit.prevent="addTask"
      class="flex space-x-4 bg-white p-4 rounded-md shadow-md"
    >
      <input
        v-model="newTask"
        type="text"
        placeholder="Enter Task Name"
        class="flex-1 px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring focus:ring-blue-200"
      />
      <button
        type="submit"
        class="px-4 py-2 bg-green-500 text-white rounded-md hover:bg-green-600"
      >
        Add
      </button>
    </form>

    <p v-if="tasks.length === 0" class="text-center text-cyan-950 font-semibold text-xl">
      No tasks available
    </p>

    
    <ul v-else class="space-y-4">
      <li
        v-for="(task, index) in (tasks)"
        :key="index"
        :class="getTaskClasses(task)"
        class="flex justify-between items-center p-4 rounded-md shadow-md"
      >
        <span>{{ task.name }}</span>
        <div class="space-x-2">
          <button
            @click="toggleTaskCompletion(index)"
            class="px-3 py-1 text-white rounded-md"
            :class="task.completed ? 'bg-yellow-500' : 'bg-cyan-950'"
          >
            {{ task.completed ? "Undo" : "Complete" }}
          </button>
          <button
            @click="deleteTask(index)"
            class="px-3 py-1 bg-red-500 text-white rounded-md hover:bg-red-600"
          >
            Delete
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>
