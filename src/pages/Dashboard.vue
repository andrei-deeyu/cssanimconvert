<template>
  <div
    id="container"
    class="container-sm shadow p-4 d-flex flex-column justify-content-between flex-md-row"
  >
    <div id="SVGContainer" class="bg-light p-3 shadow">
      <ButtonUploadSVG @file-uploaded="handleSVGUploaded" />
      <iframe ref="svgIframe" class="border shadow w-100"></iframe>
    </div>
    <div id="JSONContainer">
      <JSONStructure :extractedAnimations="extractedAnimations" :SVGFilename="svgFilename" />
      <div id="actions" class="d-flex flex-row justify-content-between gap-3">
        <ButtonDownloadJSON :extractedAnimations="extractedAnimations" :SVGFilename="svgFilename" />
        <ButtonUploadJSON @file-uploaded="handleJSONUploaded" />
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref, computed, nextTick } from 'vue'
import ButtonUploadSVG from '../components/buttons/ButtonUploadSVG.vue'
import JSONStructure from '../components/JSONStructure.vue'
import ButtonDownloadJSON from '../components/buttons/ButtonDownloadJSON.vue'
import ButtonUploadJSON from '../components/buttons/ButtonUploadJSON.vue'

const svgIframe = ref<HTMLIFrameElement | null>(null)
const svgContent = ref<string | null>(null)
const svgFilename = ref<string | null>(null)

const animations = ref<any[]>([])
const extractedAnimations = ref<object>({})

// Function triggered when file is uploaded in child component
const handleSVGUploaded = async (svg: { content: string; name: string }) => {
  svgContent.value = svg.content
  svgFilename.value = svg.name


  const parser = new DOMParser();
  const doc = parser.parseFromString(svg.content, 'image/svg+xml');

  const root = doc.documentElement;

  const iframeDoc = svgIframe.value?.contentDocument//.contentWindow?.document;

  // Clear the iframe before adding content
  // iframeDoc.body.innerHTML = '';

  const newSVGContainer = buildTree(root);

  iframeDoc?.body.appendChild(newSVGContainer);

}



function buildTree(node: Element): Element {
  // Clone the node to preserve attributes
  const newNode = document.createElementNS('http://www.w3.org/2000/svg', node.tagName);

  // Copy attributes
  for (let i = 0; i < node.attributes.length; i++) {
    const attr = node.attributes[i];
    newNode.setAttribute(attr.name, attr.value);
  }

  // If the node contains text (like in <style>), copy it
  if (node.tagName === 'style' || node.childNodes.length === 1 && node.firstChild?.nodeType === Node.TEXT_NODE) {
    newNode.textContent = node.textContent;
  }

  // Recursively build and append child nodes
  Array.from(node.children).forEach((child) => {
    newNode.appendChild(buildTree(child));
  });

  return newNode;
}


const handleJSONUploaded = (json: { content: any; name: string }) => {
  animations.value = json.content

  applyAnimationsFromJSON()
}

const applyAnimationsFromJSON = () => {
  if (!svgContent.value || animations.value.length === 0) return

  const parser = new DOMParser()
  const svgDoc = parser.parseFromString(svgContent.value, 'image/svg+xml')
  let styleTag = svgDoc.querySelector('style')

  if (!styleTag) {
    styleTag = document.createElement('style')
    svgDoc.documentElement.appendChild(styleTag)
  }

  const newStyles = animations.value
    .map(({ name, keyframes }) => `@keyframes ${name} { ${keyframes.join(' ')} }`)
    .join('\n')

  styleTag.textContent += newStyles // Append styles instead of replacing
  svgContent.value = new XMLSerializer().serializeToString(svgDoc)
}

// Reactive computed property for displaying the modified SVG
const animatedSVG = computed(() => svgContent.value)
</script>
<style scoped></style>
