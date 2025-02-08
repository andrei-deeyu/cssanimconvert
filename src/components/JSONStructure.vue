<template>
  <div class="container bg-light border border-3 border-secondary rounded-start d-flex flex-column justify-content-between">
    <div id="jsonStructure">
      <pre>{{ extractedAnimations }}</pre>
    </div>
  </div>
</template>
<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps<{
  extractedAnimations: object
}>()
</script>
<style scoped>
div.container {
  height: 500px;
  min-width: 300px;
  max-width: 500px;
  overflow-y: scroll;
}
</style>
