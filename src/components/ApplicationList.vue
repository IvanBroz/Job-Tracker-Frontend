<script setup lang="ts">
import { ref, onMounted } from 'vue'

const applications = ref([])

const baseUrl = import.meta.env.VITE_API_URL

onMounted(() => {
  fetch(`${baseUrl}/applications`)
    .then(response => response.json())
    .then(data => {
      applications.value = data
    })
})
</script>

<template>
  <div v-for="application in applications" :key="application.id">
    {{ application }}
  </div>
</template>
