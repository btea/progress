<script setup lang="ts">
import { reactive, ref, onBeforeUnmount, onMounted } from 'vue'

const one = ref()
const two = ref()
const three = ref()

const infoLeng = reactive({
  one: 0,
  two: 0,
  three: 0,
  clock: 1
})
const render = (progress: number) => {
  one.value.style.strokeDasharray = `${infoLeng.one * progress} ${infoLeng.one}`
  two.value.style.strokeDasharray = `${infoLeng.two * progress} ${infoLeng.two}`
  three.value.style.strokeDasharray = `${infoLeng.three * progress} ${infoLeng.three}`
}
let progress = 0, timer: ReturnType<typeof window.requestAnimationFrame>
const getProgress = () => {
  progress += 0.001 * infoLeng.clock
  if (progress >= 1) {
    infoLeng.clock = -1
  } 
  if(progress <= 0) {
    infoLeng.clock = 1
  }

  render(progress)
  timer = window.requestAnimationFrame(getProgress)
}

onMounted(() => {
  infoLeng.one = one.value.getTotalLength()
  infoLeng.two = two.value.getTotalLength()
  infoLeng.three = three.value.getTotalLength()
  timer = window.requestAnimationFrame(getProgress)
})
onBeforeUnmount(() => {
  window.cancelAnimationFrame(timer)
})
</script>

<template>
  <div class="view">
    <svg viewBox="0 0 600 600">
      <!-- custom gradient color -->
      <defs>
        <linearGradient id="gradient" x1="0" y1="0" x2="1" y2="1">
          <stop offset="0%" stop-color="#6cf" />
          <stop offset="50%" stop-color="#09f" />
          <stop offset="100%" stop-color="#f00" />
        </linearGradient>
      </defs>
      <path d="M 0,0 L 600,0 L 600,600 L 0,600 L 0,0 z" stroke-width="10" stroke="url(#gradient)" fill="none" ref="one"></path>
      <path d="M 300,0 L 600,300 L 300,600 L 0,300 L 300,0" stroke-width="5" stroke="url(#gradient)" fill="none" ref="two"></path>
      <circle cx="300" cy="300" r="200" stroke="url(#gradient)" fill="none" stroke-width="5" ref="three"></circle>
    </svg>
  </div>
</template>
<style lang="scss" scoped>
.view {
  width: 600px;
  height: 600px;
  margin: 50px auto;
}
</style>