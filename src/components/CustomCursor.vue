<template>
  <div id="cursorDot" ref="dotEl"></div>
  <div id="cursorLabel" ref="labelEl"><span>{{ labelText }}</span></div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const dotEl = ref(null)
const labelEl = ref(null)
const labelText = ref('')

let mx = 0, my = 0, lx = 0, ly = 0
let rafId = null

function onMouseMove(e) {
  mx = e.clientX
  my = e.clientY
  if (dotEl.value) {
    dotEl.value.style.left = mx + 'px'
    dotEl.value.style.top = my + 'px'
  }
}

function labelLoop() {
  lx += (mx - lx) * 0.18
  ly += (my - ly) * 0.18
  if (labelEl.value) {
    labelEl.value.style.left = lx + 'px'
    labelEl.value.style.top = ly + 'px'
  }
  rafId = requestAnimationFrame(labelLoop)
}

function onMouseOver(e) {
  const target = e.target.closest && e.target.closest('[data-cursor]')
  if (!target) return
  labelText.value = target.dataset.cursor
  labelEl.value.classList.add('show')
}

function onMouseOut(e) {
  const target = e.target.closest && e.target.closest('[data-cursor]')
  if (!target) return
  const related = e.relatedTarget
  if (related && target.contains(related)) return
  labelEl.value.classList.remove('show')
}

onMounted(() => {
  window.addEventListener('mousemove', onMouseMove)
  document.addEventListener('mouseover', onMouseOver)
  document.addEventListener('mouseout', onMouseOut)
  labelLoop()
})

onUnmounted(() => {
  window.removeEventListener('mousemove', onMouseMove)
  document.removeEventListener('mouseover', onMouseOver)
  document.removeEventListener('mouseout', onMouseOut)
  if (rafId) cancelAnimationFrame(rafId)
})
</script>
