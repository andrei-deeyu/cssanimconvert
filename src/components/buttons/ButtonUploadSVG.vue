<template>
  <div class="input-group mb-3">
    <label class="input-group-text">Upload SVG</label>
    <input type="file" accept="image/svg+xml" class="form-control" @change="handleFileUpload" />
  </div>
</template>
<script setup lang="ts">
import { defineEmits, defineExpose } from 'vue'

const emit = defineEmits(['file-uploaded'])

const handleFileUpload = async (event: Event) => {
  const file = (event.target as HTMLInputElement).files?.[0]
  if (file && file.type === 'image/svg+xml') {
    try {
      const content = await file.text();

      emit('file-uploaded', {
        content,
        name: file.name.split('.')[0]
      });
    } catch (error) {
      return alert('Invalid SVG');
    }
  }
}

// Expose the function if the parent needs to trigger it manually
defineExpose({ handleFileUpload })
</script>
<style scoped></style>
