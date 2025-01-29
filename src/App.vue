<script setup lang="ts">
import Dashboard from './pages/Dashboard.vue'
</script>

<template>
  <header class="navbar bg-light">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">
        <img alt="AStatus logo" class="logo" src="./assets/logo.png" />
      </a>
    </div>
  </header>

  <main>
    <Dashboard />
  </main>
</template>

<style scoped>
.logo {
  max-width: 100%;
  height: auto;
  width: 20vw;
  max-width: 150px;
  min-width: 50px;
  max-height: 10vh;
  object-fit: contain;
}
</style>
