<template>
  <div id="structure" class="container bg-light border border-3 border-secondary rounded-start d-flex flex-column justify-content-between">
    <div id="jsonStructure">
      <!-- <pre>{{ extractedAnimations }}</pre> -->
    </div>
    <div id="actions" class="d-flex flex-row justify-content-between">
      <button class="btn btn-secondary"><!-- @click="downloadJson"> -->
        <span class="bi bi-download me-2"></span>
        Download JSON
      </button>
      <button>Upload JSON</button> <!--@click="downloadJson"-->
    </div>
  </div>
</template>
<script setup lang="ts">

</script>
<style scoped>
#structure {
  height: 500px;
  min-width: 300px;
  max-width: 500px;
}
</style>
