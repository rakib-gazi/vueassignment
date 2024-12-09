<script setup>
import TaskList from "./components/TaskList.vue";
</script>

<template>
  <div class="min-h-screen flex justify-center items-center">
    <div class="w-full lg:w-1/2 mx-auto p-1 lg:p-6 bg-gray-100 shadow-lg rounded-lg">
      <h1 class="text-3xl font-bold text-center text-cyan-950 mb-6">
        Task Management App
      </h1>
      <TaskList />
    </div>
  </div>
</template>
