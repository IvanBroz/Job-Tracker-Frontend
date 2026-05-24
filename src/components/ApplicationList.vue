<script setup lang="ts">
import { ref, onMounted } from 'vue'

type Application = {
  id: number
  company: string
  position: string
  status: string
}

const applications = ref<Application[]>([])

const baseUrl = import.meta.env.VITE_API_URL

function loadApplications() {
  fetch(`${baseUrl}/applications`)
    .then(response => response.json())
    .then(data => {
      applications.value = data
    })
}

onMounted(() => {
  loadApplications()
})
</script>

<template>
  <div v-for="application in applications" :key="application.id">
    {{ application }}
  </div>
</template>
