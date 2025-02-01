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
const extractedAnimations = ref<any[]>([])

const animationData = ref<any[]>([]) // Store JSON animations

// Function triggered when file is uploaded in child component
const handleSVGUploaded = async (svg: { content: string; name: string }) => {
  svgContent.value = svg.content
  svgFilename.value = svg.name

  extractAnimationsFromSVG(svg.content)

  await nextTick();
  if(svgIframe.value) {
    svgIframe.value.srcdoc = svg.content;
  }
  // applyAnimationsFromJSON();  -- ?
}

const handleJSONUploaded = (json: { content: any; name: string }) => {
  animationData.value = json.content

  applyAnimationsFromJSON()
}

const extractAnimationsFromSVG = (svgString: string) => {
  const parser = new DOMParser()
  const svgDoc = parser.parseFromString(svgString, 'image/svg+xml')
  const styleTags = svgDoc.querySelectorAll('style')

  extractedAnimations.value = []

  styleTags.forEach((styleTag) => {
    const cssText = styleTag.textContent || ''
    const lines = cssText.split('\n').map((line) => line.trim())

    let currentAnimation: { name: string; keyframes: string[] } | null = null

    lines.forEach((line) => {
      if (line.startsWith('@keyframes')) {
        // Start a new animation block
        const name = line.split(' ')[1]
        currentAnimation = { name, keyframes: [] }
      } else if (currentAnimation && line.endsWith('}')) {
        // Close the animation block
        extractedAnimations.value.push(currentAnimation)
        currentAnimation = null
      } else if (currentAnimation) {
        // Collect keyframe rules
        currentAnimation.keyframes.push(line)
      }
    })
  })
}

const applyAnimationsFromJSON = () => {
  if (!svgContent.value || animationData.value.length === 0) return

  const parser = new DOMParser()
  const svgDoc = parser.parseFromString(svgContent.value, 'image/svg+xml')
  let styleTag = svgDoc.querySelector('style')

  if (!styleTag) {
    styleTag = document.createElement('style')
    svgDoc.documentElement.appendChild(styleTag)
  }

  const newStyles = animationData.value
    .map(({ name, keyframes }) => `@keyframes ${name} { ${keyframes.join(' ')} }`)
    .join('\n')

  styleTag.textContent += newStyles // Append styles instead of replacing
  svgContent.value = new XMLSerializer().serializeToString(svgDoc)
}

// Reactive computed property for displaying the modified SVG
const animatedSVG = computed(() => svgContent.value)
</script>
<style scoped></style>
