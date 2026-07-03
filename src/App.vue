<template>
  <div class="grain-bg"></div>
  <div class="noise"></div>
  <div id="progressBar" ref="progressBarEl"></div>

  <CustomCursor />

  <Preloader v-if="showPreloader" @done="onPreloaderDone" />

  <header id="siteHeader" ref="headerEl">
    <div class="logo">Nilesh<span class="dot">.</span>Dev</div>
    <nav>
      <ul>
        <li><a href="#work">Work</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#skills">Skills</a></li>
        <li><a href="#contact">Contact</a></li>
      </ul>
    </nav>
    <a class="hdr-cta magnetic" href="#contact" data-cursor="Say hi" ref="magneticEl">Let's talk</a>
  </header>

  <section class="hero" id="heroSection" ref="heroSectionEl">
    <div id="dotgrid" ref="dotgridEl"></div>
    <p class="hero-eyebrow" ref="eyebrowEl">Frontend Developer — Mumbai, India</p>
    <h1 id="heroHeadline" ref="headlineEl"></h1>
    <p class="sub" id="heroSub" ref="subEl">4+ years turning <b>design files into production</b> — accessible, fast, pixel-true, for banking and enterprise teams.</p>
    <div class="scroll-cue" id="scrollCue" ref="scrollCueEl"><div class="line"></div><span>SCROLL</span></div>
  </section>

  <About />
  <Work />
  <Skills />
  <Contact />
  <SiteFooter />
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Lenis from '@studio-freight/lenis'
import { gsap, ScrollTrigger } from './gsapSetup'
import CustomCursor from './components/CustomCursor.vue'
import Preloader from './components/Preloader.vue'
import About from './components/About.vue'
import Work from './components/Work.vue'
import Skills from './components/Skills.vue'
import Contact from './components/Contact.vue'
import SiteFooter from './components/SiteFooter.vue'

const showPreloader = ref(true)
const progressBarEl = ref(null)
const headerEl = ref(null)
const magneticEl = ref(null)
const heroSectionEl = ref(null)
const dotgridEl = ref(null)
const eyebrowEl = ref(null)
const headlineEl = ref(null)
const subEl = ref(null)
const scrollCueEl = ref(null)

function splitWords(el, text) {
  el.innerHTML = ''
  const arr = text.trim().split(' ')
  arr.forEach((w, i) => {
    const word = document.createElement('span')
    word.className = 'word'
    if (w.includes('*')) {
      const em = document.createElement('em')
      em.textContent = w.replace(/\*/g, '')
      word.appendChild(em)
    } else {
      const inner = document.createElement('span')
      inner.textContent = w
      word.appendChild(inner)
    }
    el.appendChild(word)
    if (i < arr.length - 1) el.appendChild(document.createTextNode(' '))
  })
  return el.querySelectorAll('.word > span, .word > em')
}

function revealHero() {
  gsap.to(headerEl.value, { opacity: 1, duration: 0.8 })
  const words = splitWords(headlineEl.value, 'Crafting interfaces that feel *alive*.')
  gsap
    .timeline()
    .to(eyebrowEl.value, { opacity: 1, duration: 0.5 })
    .to(words, { opacity: 1, y: 0, duration: 0.9, stagger: 0.05, ease: 'power4.out' }, '-=0.2')
    .to(subEl.value, { opacity: 1, duration: 0.7 }, '-=0.4')
    .to(scrollCueEl.value, { opacity: 1, duration: 0.5 }, '-=0.3')
}

function onPreloaderDone() {
  showPreloader.value = false
  revealHero()
}

onMounted(() => {
  // hero spotlight follows cursor
  heroSectionEl.value.addEventListener('mousemove', (e) => {
    const r = heroSectionEl.value.getBoundingClientRect()
    dotgridEl.value.style.setProperty('--mx', e.clientX - r.left + 'px')
    dotgridEl.value.style.setProperty('--my', e.clientY - r.top + 'px')
  })

  // magnetic button
  const btn = magneticEl.value
  btn.addEventListener('mousemove', (e) => {
    const r = btn.getBoundingClientRect()
    btn.style.transform = `translate(${(e.clientX - r.left - r.width / 2) * 0.25}px, ${(e.clientY - r.top - r.height / 2) * 0.35}px)`
  })
  btn.addEventListener('mouseleave', () => (btn.style.transform = 'translate(0,0)'))

  // lenis smooth scroll
  const lenis = new Lenis({ lerp: 0.09 })
  gsap.ticker.add((t) => lenis.raf(t * 1000))
  gsap.ticker.lagSmoothing(0)
  lenis.on('scroll', ScrollTrigger.update)
  lenis.on('scroll', ({ scroll, limit }) => {
    progressBarEl.value.style.width = (scroll / limit) * 100 + '%'
  })
})
</script>
