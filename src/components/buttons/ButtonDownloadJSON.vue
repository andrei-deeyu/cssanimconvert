<template>
  <button class="btn btn-secondary" @click="downloadJson">
    <span class="bi bi-download me-2"></span>Download JSON
  </button>
</template>
<script setup lang="ts">
import { defineProps } from 'vue'

const props = defineProps<{
  extractedAnimations: object
  SVGFilename: string | null
}>()

const downloadJson = () => {
  const jsonData = JSON.stringify(props.extractedAnimations, null, 2)
  const blob = new Blob([jsonData], { type: 'application/json' })
  const url = URL.createObjectURL(blob)

  const link = document.createElement('a')
  link.href = url
  link.download = `${props.SVGFilename}_animations.json`
  document.body.appendChild(link)
  link.click()

  document.body.removeChild(link) // Cleanup
  URL.revokeObjectURL(url) // Free up memory
}
</script>
<style scoped>
#structure {
  height: 500px;
  min-width: 300px;
  max-width: 500px;
}
</style>
