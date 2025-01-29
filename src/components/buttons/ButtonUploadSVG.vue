<template>
<div class="input-group mb-3">
  <label class="input-group-text">Upload SVG</label>
  <input type="file" accept="image/svg+xml" class="form-control" @change="handleFileUpload" />
</div>
</template>
<script setup lang="ts">
import { defineEmits, defineExpose } from 'vue';

const emit = defineEmits(['file-uploaded']);

const handleFileUpload = (event: Event) => {
  const file = (event.target as HTMLInputElement).files?.[0];
  if (file && file.type === 'image/svg+xml') {
    const reader = new FileReader();
    reader.onload = () => {
      emit('file-uploaded', {
        content: reader.result as string,
        name: file.name
      });
    };

    reader.readAsText(file);
  }
};

// Expose the function if the parent needs to trigger it manually
defineExpose({ handleFileUpload });
</script>
<style scoped>

</style>
