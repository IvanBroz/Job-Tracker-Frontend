<script setup lang="ts">
import { ref, onMounted } from 'vue'

type Application = {
  id: number
  company: string
  position: string
  status: string
}

const applications = ref<Application[]>([])

const newCompany = ref('')
const newPosition = ref('')
const newStatus = ref('Pending')

const baseUrl = import.meta.env.VITE_API_URL

function loadApplications() {
  fetch(`${baseUrl}/applications`)
    .then(response => response.json())
    .then(data => {
      applications.value = data
    })
}

function addApplication() {
  fetch(`${baseUrl}/applications`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      company: newCompany.value,
      position: newPosition.value,
      status: newStatus.value,
    }),
  })
    .then(response => response.json())
    .then(() => {
      newCompany.value = ''
      newPosition.value = ''
      newStatus.value = 'Pending'
      loadApplications()
    })
}

onMounted(() => {
  loadApplications()
})
</script>

<template>
  <div class="form">
    <input v-model="newPosition" placeholder="Position" />
    <input v-model="newCompany" placeholder="Company" />

    <select v-model="newStatus">
      <option>Pending</option>
      <option>Interview Round 1</option>
      <option>Rejected</option>
    </select>

    <button @click="addApplication">Add Application</button>
  </div>

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
.form {
  margin: 40px auto 20px;
  display: flex;
  gap: 8px;
  justify-content: center;
}

table {
  margin: 0 auto;
  border-collapse: collapse;
}

table,
th,
td {
  border: 1px solid black;
  padding: 8px;
  text-align: center;
}

input,
select,
button {
  padding: 6px;
  cursor: pointer;
}
</style>
