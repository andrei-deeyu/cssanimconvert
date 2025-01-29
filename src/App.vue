<script setup lang="ts">
import Dashboard from './pages/Dashboard.vue'
</script>

<template>
  <header>
    <img alt="AStatus logo" class="logo" src="./assets/logo.svg" width="300" height="60" />
    <!--
      <div id="nav-item">

      </div>
    -->
  </header>

  <main>
    <Dashboard />
  </main>
</template>

<style scoped>
header {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 60px;
  line-height: 1.5;
  box-shadow: 0px 6px 12px rgba(0, 0, 0, 0.25);
}

.logo {
  margin-left: 1rem;
}
</style>
