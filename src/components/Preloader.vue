<template>
  <div class="pl-root">
    <div class="curtain" id="curtainL" ref="curtainL"></div>
    <div class="curtain" id="curtainR" ref="curtainR"></div>

    <div class="preloader" ref="preloaderEl">
      <div class="terminal">
        <div class="term-bar"><i></i><i></i><i></i><span>zsh — portfolio-build</span></div>
        <div class="term-body" ref="termBodyEl"></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { gsap } from '../gsapSetup'

const emit = defineEmits(['done'])

const curtainL = ref(null)
const curtainR = ref(null)
const preloaderEl = ref(null)
const termBodyEl = ref(null)

const lines = [
  'initializing_portfolio.sh',
  'installing dependencies... <b>done</b>',
  'compiling components... <b>done</b>',
  'optimizing assets... <b>done</b>',
  'running a11y audit... <b>passed</b>',
  '<b>ready.</b>',
]

let typeInterval = null
let startTimeout = null
let curtainTimeout = null

function typeLine(li) {
  if (li >= lines.length) {
    curtainTimeout = setTimeout(openCurtain, 400)
    return
  }
  const div = document.createElement('div')
  div.className = 'l'
  const prefix = li === 0 ? '$ ' : '&gt; '
  termBodyEl.value.appendChild(div)
  const full = prefix + lines[li]
  let i = 0
  typeInterval = setInterval(() => {
    div.innerHTML = full.slice(0, i) + '<span class="term-caret">▌</span>'
    i++
    if (i > full.length) {
      clearInterval(typeInterval)
      div.innerHTML = full
      startTimeout = setTimeout(() => typeLine(li + 1), 220)
    }
  }, 14)
}

function openCurtain() {
  gsap.timeline({
    onComplete() {
      preloaderEl.value.style.display = 'none'
      emit('done')
    },
  })
    .to(curtainL.value, { xPercent: -100, duration: 0.9, ease: 'power4.inOut' })
    .to(curtainR.value, { xPercent: 100, duration: 0.9, ease: 'power4.inOut' }, '<')
}

onMounted(() => {
  startTimeout = setTimeout(() => typeLine(0), 400)
})

onBeforeUnmount(() => {
  clearInterval(typeInterval)
  clearTimeout(startTimeout)
  clearTimeout(curtainTimeout)
})
</script>

<style scoped>
.pl-root {
  --pl-black: #030304;
  --pl-panel-border: rgba(255, 255, 255, 0.10);
  --pl-dim: #8C8D9A;
  --pl-ember: #FF7A45;
}

.preloader {
  position: fixed;
  inset: 0;
  background: var(--pl-black);
  z-index: 10000;
  display: flex;
  align-items: center;
  justify-content: center;
}

.terminal {
  width: min(560px, 86vw);
  border: 1px solid var(--pl-panel-border);
  border-radius: 10px;
  background: #08090B;
  overflow: hidden;
  box-shadow: 0 40px 120px -30px rgba(0, 0, 0, .8);
}

.term-bar {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 12px 16px;
  border-bottom: 1px solid var(--pl-panel-border);
}
.term-bar i {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: rgba(255, 255, 255, .15);
}
.term-bar span {
  margin-left: 10px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 11px;
  color: var(--pl-dim);
}

.term-body {
  padding: 22px 24px;
  min-height: 180px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 13px;
  color: #9FE0B8;
  line-height: 1.9;
}
.term-body :deep(.l) { color: var(--pl-dim); }
.term-body :deep(.l b) { color: #fff; }
.term-body :deep(.term-caret) {
  display: inline-block;
  width: 7px;
  height: 14px;
  background: var(--pl-ember);
  vertical-align: middle;
  animation: pl-blink 1s step-end infinite;
}
@keyframes pl-blink { 50% { opacity: 0; } }

.curtain {
  position: fixed;
  top: 0;
  bottom: 0;
  width: 50%;
  background: var(--pl-black);
  z-index: 9995;
}
#curtainL { left: 0; }
#curtainR { right: 0; }
</style>
