<script setup lang="ts">
import { ref, onBeforeUnmount, onMounted } from 'vue'

const path = ref()
let length: number
const render = (progress: number) => {
  path.value.style.strokeDasharray = `${length * progress} ${length}`
}
let progress = 0, timer: ReturnType<typeof window.requestAnimationFrame>
const getProgress = () => {
  progress += 0.002
  if (progress >= 1) {
    progress = 0
    window.cancelAnimationFrame(timer)
  }
  render(progress)
  timer = window.requestAnimationFrame(getProgress)
}

onMounted(() => {
  length = path.value.getTotalLength()
  timer = window.requestAnimationFrame(getProgress)
})
onBeforeUnmount(() => {
  window.cancelAnimationFrame(timer)
})
</script>

<template>
  <div class="view">
    <svg viewBox="0 0 800 600">
      <path d="M 0,0 L 800,0 L 800,600 L 0,600 L 0,0" stroke-width="10" stroke="#6cf" fill="none" ref="path"></path>
    </svg>
  </div>
</template>
<style lang="scss" scoped>
.view {
  width: 800px;
  height: 600px;
  margin: 0 auto;
}
</style>