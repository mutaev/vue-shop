<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String
})
</script>

<template>
  <div class="flex justify-center items-center flex-col text-center w-72 mx-auto">
    <img height="90" width="90" :src="imageUrl" alt="Info image"/>
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-400">{{ description }}</p>
  </div>
</template>


