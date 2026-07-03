<template>
  <section id="contact">
    <div class="contact-hero">
      <span class="eyebrow" style="justify-content:center;">Contact</span>
      <h2 class="split-title" ref="titleEl" style="margin:0 auto;text-align:center;">Have a role — or a problem worth solving?</h2>
      <a class="big-email" href="mailto:nileshrai454@gmail.com" data-cursor="Copy">nileshrai454@gmail.com</a>
      <div class="contact-links">
        <a href="https://www.linkedin.com/in/nilesh-rai-48041a155/" target="_blank" data-cursor="Open">LinkedIn</a>
        <a href="https://github.com/webdevil-new" target="_blank" data-cursor="Open">GitHub</a>
        <a href="tel:9892180608" target="_blank" data-cursor="Call">+91 98921 80608</a>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from '../gsapSetup'

const titleEl = ref(null)

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

onMounted(() => {
  const chars = splitChars(titleEl.value)
  gsap.to(chars, {
    opacity: 1,
    y: 0,
    duration: 0.6,
    stagger: 0.008,
    ease: 'power3.out',
    scrollTrigger: { trigger: titleEl.value, start: 'top 85%' },
  })
})
</script>
