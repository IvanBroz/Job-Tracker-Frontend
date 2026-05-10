<script setup>

const applications = [

  {

    company: 'Siemens',

    position: 'Junior Developer',

    status: 'Bewerbung gesendet',

  },

  {

    company: 'BMW',

    position: 'Backend Developer',

    status: 'Interview',

  },

]

</script>

<template>

  <div v-for="application in applications" :key="application.company">

    {{ application }}

  </div>

</template>
