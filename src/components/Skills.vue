<template>
  <section id="skills">
    <span class="eyebrow">Toolkit</span>
    <h2 class="split-title" ref="titleEl">The stack, connected.</h2>
    <p class="sec-lede">Move your cursor through it.</p>
    <div id="constellation" ref="consWrapEl">
      <canvas id="consCanvas" ref="canvasEl"></canvas>
      <div
        class="cnode"
        v-for="(n, i) in nodes"
        :key="i"
        :style="{ left: n.x + 'px', top: n.y + 'px' }"
        data-cursor=""
      >{{ n.label }}</div>
    </div>
  </section>
</template>

<script setup>
import { ref, reactive, onMounted, onUnmounted, nextTick } from 'vue'
import { gsap } from '../gsapSetup'

const titleEl = ref(null)
const consWrapEl = ref(null)
const canvasEl = ref(null)
const nodes = ref([])

const skills = ['React.js', 'Next.js', 'Vue.js', 'JavaScript', 'Tailwind', 'SCSS', 'Bootstrap', 'Node.js', 'WordPress', 'Webflow', 'Git']

let cctx, cw, ch
let nodePos = []
const consMouse = { x: -999, y: -999, active: false }
let rafId = null

function layoutConstellation() {
  const wrap = consWrapEl.value
  const canvas = canvasEl.value
  cw = wrap.clientWidth
  ch = wrap.clientHeight
  canvas.width = cw
  canvas.height = ch

  nodePos = []
  const golden = (137.508 * Math.PI) / 180
  const newNodes = skills.map((s, i) => {
    const t = i / (skills.length - 1)
    const radius = 40 + t * (Math.min(cw, ch) * 0.42)
    const angle = i * golden
    const x = cw / 2 + Math.cos(angle) * radius
    const y = ch / 2 + Math.sin(angle) * radius * 0.62
    nodePos.push({ x, y, label: s })
    return { x, y, label: s }
  })
  nodes.value = newNodes
}

function onMouseMove(e) {
  const r = consWrapEl.value.getBoundingClientRect()
  consMouse.x = e.clientX - r.left
  consMouse.y = e.clientY - r.top
  consMouse.active = true
}
function onMouseLeave() {
  consMouse.active = false
}

function drawConstellation() {
  cctx.clearRect(0, 0, cw, ch)
  for (let i = 0; i < nodePos.length; i++) {
    for (let j = i + 1; j < nodePos.length; j++) {
      const a = nodePos[i], b = nodePos[j]
      const d = Math.hypot(a.x - b.x, a.y - b.y)
      if (d < Math.min(cw, ch) * 0.4) {
        cctx.strokeStyle = `rgba(239,234,226,${0.09 * (1 - d / (Math.min(cw, ch) * 0.4))})`
        cctx.lineWidth = 1
        cctx.beginPath()
        cctx.moveTo(a.x, a.y)
        cctx.lineTo(b.x, b.y)
        cctx.stroke()
      }
    }
    if (consMouse.active) {
      const d = Math.hypot(nodePos[i].x - consMouse.x, nodePos[i].y - consMouse.y)
      if (d < 200) {
        cctx.strokeStyle = `rgba(217,178,108,${0.5 * (1 - d / 200)})`
        cctx.lineWidth = 1.4
        cctx.beginPath()
        cctx.moveTo(nodePos[i].x, nodePos[i].y)
        cctx.lineTo(consMouse.x, consMouse.y)
        cctx.stroke()
      }
    }
  }
  rafId = requestAnimationFrame(drawConstellation)
}

function splitChars(el) {
  const text = el.textContent
  el.innerHTML = ''
  ;[...text].forEach((ch) => {
    const s = document.createElement('span')
    s.className = 'char'
    s.textContent = ch === ' ' ? '\u00A0' : ch
    el.appendChild(s)
  })
  return el.querySelectorAll('.char')
}

onMounted(async () => {
  const chars = splitChars(titleEl.value)
  gsap.to(chars, {
    opacity: 1,
    y: 0,
    duration: 0.6,
    stagger: 0.008,
    ease: 'power3.out',
    scrollTrigger: { trigger: titleEl.value, start: 'top 85%' },
  })

  cctx = canvasEl.value.getContext('2d')
  layoutConstellation()
  await nextTick()
  layoutConstellation()

  window.addEventListener('resize', layoutConstellation)
  consWrapEl.value.addEventListener('mousemove', onMouseMove)
  consWrapEl.value.addEventListener('mouseleave', onMouseLeave)
  drawConstellation()
})

onUnmounted(() => {
  window.removeEventListener('resize', layoutConstellation)
  if (consWrapEl.value) {
    consWrapEl.value.removeEventListener('mousemove', onMouseMove)
    consWrapEl.value.removeEventListener('mouseleave', onMouseLeave)
  }
  if (rafId) cancelAnimationFrame(rafId)
})
</script>
