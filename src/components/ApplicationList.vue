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
  <table>

    <tr>
      <th>Position</th>
      <th>Company</th>
      <th>Job ID</th>
      <th>Page</th>
      <th>Date</th>
      <th>Status</th>
    </tr>

    <tr v-for="application in applications" :key="application.id">
      <td>{{ application.position }}</td>
      <td>{{ application.company }}</td>
      <td>{{ application.id }}</td>
      <td>LinkedIn</td>
      <td>24.05.2026</td>
      <td>

        <select v-model="application.status">

          <option>Pending</option>

          <option>Interview Round 1</option>

          <option>Rejected</option>

        </select>

      </td>
    </tr>

  </table>
</template>
<style scoped>
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
  padding: 8px;
}
select {

  cursor: pointer;

  padding: 4px;

}
</style>
