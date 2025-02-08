<template>
  <div class="input-group mb-3 d-flex flex-column">
    <label class="input-group-text bg-white">Upload JSON</label>
    <input
      type="file"
      accept="application/json"
      class="form-control w-100"
      @change="handleFileUpload"
    />
  </div>
</template>
<script setup lang="ts">
import { defineEmits, defineExpose } from 'vue'

const emit = defineEmits(['file-uploaded'])

const handleFileUpload = async (event: Event) => {
  const file = (event.target as HTMLInputElement).files?.[0]
  if (file && file.type === 'application/json') {
    try {
      const content = await file.text();
      const jsonData = JSON.parse(content);

      emit('file-uploaded', {
        content: jsonData,
        name: file.name.split('.')[0],
      });
    } catch (error) {
      alert('Invalid JSON format')
    }
  } else {
    alert('Invalid JSON File')
  }
}

// Expose the function if the parent needs to trigger it manually
defineExpose({ handleFileUpload })
</script>
<style scoped></style>
