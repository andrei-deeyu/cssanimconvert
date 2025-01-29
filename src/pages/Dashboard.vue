<script setup lang="ts">
import ButtonUploadSVG from '../components/buttons/ButtonUploadSVG.vue';
import JSONStructure from '../components/JSONStructure.vue';

</script>

<template>
  <div id="container">
    <div id="SVGContainer">
      <ButtonUploadSVG>
          <template #text>upload SVG</template>
      </ButtonUploadSVG>
    </div>
    <div id="JSONContainer">
      <JSONStructure>

      </JSONStructure>
    </div>
  </div>
</template>

<style scoped>
#container {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  width: 100%;
}
</style>
