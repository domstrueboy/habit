<script setup>
import Streak from './components/Streak.vue'
</script>

<template>
  <Streak :data="['20.8.23', ['25.8.23', 5], '1.9.23', ['20.9.23', 10]]" />
  <button>✔</button>
</template>

<style scoped>
</style>
