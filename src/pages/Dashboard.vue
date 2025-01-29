<template>
  <div
    id="container"
    class="container-sm shadow p-4
          d-flex flex-column justify-content-between
          flex-md-row
    ">
    <div id="SVGContainer" class="bg-light p-3 shadow">
      <ButtonUploadSVG @file-uploaded="handleSVGUploaded"/>
      <div v-if="svgContent" v-html="svgContent" class="svg-container"></div>
    </div>
    <div id="JSONContainer">
      <JSONStructure>
      </JSONStructure>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue';
import ButtonUploadSVG from '../components/buttons/ButtonUploadSVG.vue';

const svgContent = ref<string | null>(null);

// Function triggered when file is uploaded in child component
const handleSVGUploaded = (svg: string) => {
  svgContent.value = svg;
};
</script>
<style scoped>

</style>
