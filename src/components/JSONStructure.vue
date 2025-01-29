<template>
  <div id="structure" class="container bg-light border border-3 border-secondary rounded-start d-flex flex-column justify-content-between">
    <div id="jsonStructure">
      <pre>{{ extractedAnimations }}</pre>
    </div>
    <div id="actions" class="d-flex flex-row justify-content-between gap-3">
      <ButtonDownloadJSON
        :extractedAnimations="extractedAnimations"
        :SVGFilename="SVGFilename">
      </ButtonDownloadJSON>
      <ButtonUploadJSON>

      </ButtonUploadJSON>
    </div>
  </div>
</template>
<script setup lang="ts">
import { defineProps } from 'vue';

const props = defineProps<{
  extractedAnimations: any[];
  SVGFilename: string | null;
}>();

import ButtonDownloadJSON from './buttons/ButtonDownloadJSON.vue';
import ButtonUploadJSON from './buttons/ButtonUploadJSON.vue';
</script>
<style scoped>
#structure {
  height: 500px;
  min-width: 300px;
  max-width: 500px;
}
</style>
