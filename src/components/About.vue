<template>
  <section id="about">
    <span class="eyebrow">About</span>
    <h2 class="split-title" ref="titleEl">Precision as a habit, not a checklist.</h2>
    <div class="about-grid">
      <div>
        <p class="bio-fill" ref="bioEl">I build interfaces for institutions where getting it wrong isn't an option — banks, government portals, and enterprise platforms used by people who never chose to be there, and can't be excluded from it.</p>
        <div class="stat-row">
          <div class="stat"><b ref="c1" data-count="4">0</b><span>+ Years experience</span></div>
          <div class="stat"><b ref="c2" data-count="50">0</b><span>K+ Users shipped to</span></div>
          <div class="stat"><b ref="c3" data-count="45">0</b><span>% Faster, best case</span></div>
        </div>
      </div>
      <div class="timeline">
        <div class="t-item" v-for="(item, i) in timelineItems" :key="i" ref="timelineRefs">
          <span class="yr mono">{{ item.yr }}</span>
          <h4>{{ item.title }}</h4>
          <p>{{ item.desc }}</p>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { gsap, ScrollTrigger } from '../gsapSetup'

const titleEl = ref(null)
const bioEl = ref(null)
const c1 = ref(null)
const c2 = ref(null)
const c3 = ref(null)
const timelineRefs = ref([])

const timelineItems = [
  { yr: '2020', title: 'B.Sc. Computer Science', desc: 'University of Mumbai.' },
  { yr: 'JAN 2022 — SEP 2022', title: 'Trainee Software Developer, Ele Market Place', desc: 'PHP/CodeIgniter backend modules, jQuery + AJAX interfaces for listing and ticketing platforms.' },
  { yr: 'SEP 2022 — PRESENT', title: 'Frontend Developer, Goldmine Advertising', desc: 'Owning frontend delivery for banking, government, and enterprise clients — Union Bank of India, BPCL, and more.' },
]

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

  gsap.to(bioEl.value, {
    backgroundPosition: '0% 0',
    ease: 'none',
    scrollTrigger: { trigger: bioEl.value, start: 'top 75%', end: 'bottom 45%', scrub: 0.6 },
  })

  ;[c1.value, c2.value, c3.value].forEach((el) => {
    const target = +el.dataset.count
    ScrollTrigger.create({
      trigger: el,
      start: 'top 90%',
      once: true,
      onEnter() {
        gsap.to(
          { v: 0 },
          {
            v: target,
            duration: 1.6,
            ease: 'power2.out',
            onUpdate() {
              el.textContent = Math.round(this.targets()[0].v)
            },
          }
        )
      },
    })
  })

  timelineRefs.value.forEach((item) => {
    ScrollTrigger.create({
      trigger: item,
      start: 'top 85%',
      onEnter: () => item.classList.add('in'),
    })
  })
})
</script>
