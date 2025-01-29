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
      <JSONStructure
        :extractedAnimations="extractedAnimations">
      </JSONStructure>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue';
import ButtonUploadSVG from '../components/buttons/ButtonUploadSVG.vue';
import JSONStructure from '../components/JSONStructure.vue';

const svgContent = ref<string | null>(null);
const extractedAnimations = ref<any[]>([]);

const extractAnimations = (svgString: string) => {
  const parser = new DOMParser();
  const svgDoc = parser.parseFromString(svgString, 'image/svg+xml');
  const styleTags = svgDoc.querySelectorAll('style');
  extractedAnimations.value = [];

  styleTags.forEach((styleTag) => {
    const cssText = styleTag.textContent || '';
    const animationRegex = /@keyframes\s+(\w+)\s*{([^}]*)}/g;
    let match;

    while ((match = animationRegex.exec(cssText)) !== null) {
      const name = match[1];
      const keyframes = match[2].trim().split(/\s*}\s*/).filter(Boolean).map(kf => kf + '}');
      extractedAnimations.value.push({ name, keyframes });
    }
  });
};

// Function triggered when file is uploaded in child component
const handleSVGUploaded = (svg: string) => {
  svgContent.value = svg;
  extractAnimations(svg);
};
</script>
<style scoped>

</style>
