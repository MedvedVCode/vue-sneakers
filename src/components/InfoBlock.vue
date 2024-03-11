<template>
  <div class="flex flex-col items-center justify-center h-full">
    <img width="80" height="80" :src="imageUrl" :alt="title" />
    <h2 class="mt-4 text-xl font-medium">{{ title }}</h2>
    <p class="mt-4 mb-4 w-3/5 text-gray-400">{{ decription }}</p>
  </div>
</template>

<script setup>
defineProps({
  title: String,
  decription: String,
  imageUrl: String
})
</script>
