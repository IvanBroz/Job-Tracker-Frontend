<script setup lang="ts">
import { ref, onMounted } from 'vue'

type Application = {
  id: number
  company: string
  position: string
  jobLink: string
  applicationDate: string
  status: string
}

const applications = ref<Application[]>([])

const newCompany = ref('')
const newPosition = ref('')
const newJobLink = ref('')
const newApplicationDate = ref('')
const newStatus = ref('Beworben')

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
      jobLink: newJobLink.value,
      applicationDate: newApplicationDate.value,
      status: newStatus.value,
    }),
  })
    .then(response => response.json())
    .then(() => {
      newCompany.value = ''
      newPosition.value = ''
      newJobLink.value = ''
      newApplicationDate.value = ''
      newStatus.value = 'Beworben'

      loadApplications()
    })
}

function deleteApplication(id: number) {
  fetch(`${baseUrl}/applications/${id}`, {
    method: 'DELETE',
  }).then(() => {
    loadApplications()
  })
}

function statusClass(status: string) {
  return status === 'Abgelehnt' ? 'rejected' : ''
}

onMounted(() => {
  loadApplications()
})
</script>

<template>
  <div class="form">
    <input
      v-model="newPosition"
      placeholder="Jobbezeichnung"
    />

    <input
      v-model="newCompany"
      placeholder="Unternehmen"
    />

    <input
      v-model="newJobLink"
      placeholder="Link zur Stellenausschreibung"
    />

    <input
      v-model="newApplicationDate"
      type="date"
    />

    <select v-model="newStatus">
      <option>Beworben</option>
      <option>Interview 1</option>
      <option>Abgelehnt</option>
    </select>

    <button @click="addApplication">
      Hinzufügen
    </button>
  </div>

  <table>
    <tr>
      <th>Jobbezeichnung</th>
      <th>Unternehmen</th>
      <th>Stellenausschreibung</th>
      <th>Datum</th>
      <th>Status</th>
    </tr>

    <tr
      v-for="application in applications"
      :key="application.id"
    >
      <td>{{ application.position }}</td>

      <td>{{ application.company }}</td>

      <td>
        <a
          :href="application.jobLink"
          target="_blank"
        >
          Öffnen
        </a>
      </td>

      <td>{{ application.applicationDate }}</td>

      <td class="status-cell">
        <select
          v-model="application.status"
          :class="statusClass(application.status)"
        >
          <option>Beworben</option>
          <option>Interview 1</option>
          <option>Abgelehnt</option>
        </select>

        <button
          class="delete-button"
          @click="deleteApplication(application.id)"
        >
          ✕
        </button>
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
  flex-wrap: wrap;
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
}

.status-cell {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.rejected {
  background-color: red;
  color: white;
}

.delete-button {
  background: none;
  border: none;
  color: red;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  padding: 0;
}

.delete-button:hover {
  transform: scale(1.2);
}

a {
  text-decoration: none;
}
</style>
