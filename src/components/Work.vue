<template>
  <section class="stack-wrap" id="work">
    <div class="stack-head">
      <span class="eyebrow">Selected Work</span>
      <h2 class="split-title" ref="titleEl">Four builds, stacked.</h2>
      <p class="sec-lede">Keep scrolling — each project stacks over the last.</p>
    </div>

    <div
      class="stack-card"
      v-for="(p, i) in projects"
      :key="i"
      :style="{ zIndex: 2 + i }"
      ref="cardRefs"
    >
      <div class="stack-inner">
        <div class="stack-info">
          <span class="idx mono">{{ String(i + 1).padStart(2, '0') }} / {{ String(projects.length).padStart(2, '0') }}</span>
          <h3>{{ p.title }}</h3>
          <p>{{ p.desc }}</p>
          <div class="stack-tags"><span v-for="(tag, ti) in p.tags" :key="ti">{{ tag }}</span></div>
          <!-- <a
            v-if="p.liveUrl"
            class="stack-link pt-6"
            :href="p.liveUrl"
            target="_blank"
            rel="noopener noreferrer"
            data-cursor="Visit"
          >Visit site ↗</a> -->
        </div>

        <component
          :is="p.liveUrl ? 'a' : 'div'"
          v-bind="p.liveUrl ? { href: p.liveUrl, target: '_blank', rel: 'noopener noreferrer' } : {}"
          class="stack-mock"
          data-cursor="View"
        >
          <div class="bar"><i></i><i></i><i></i></div>
          <div class="body">{{ p.mockLabel }}</div>
        </component>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { gsap, ScrollTrigger } from '../gsapSetup'

const titleEl = ref(null)
const cardRefs = ref([])

// Fill in `liveUrl` for any project that's public-facing. Leave it empty
// ('') for internal/client-owned portals (banking, enterprise admin, etc.)
// that shouldn't be linked publicly — the "Visit site" pill and clickable
// mock only render when a liveUrl is present.
const projects = [
  {
    title: 'Union Bank of India',
    desc: 'Government-compliant banking portal developed with ASP.NET MVC, following GIGW 2.1 accessibility and usability standards for a secure, responsive experience.',
    tags: ['ASP.NET MVC', 'HTML', 'CSS', 'JavaScript', 'GIGW 2.1'],
    mockLabel: 'Government Banking Portal',
    liveUrl: 'https://www.unionbankofindia.bank.in/en/home',
  },
  {
    title: 'Bharat Petroleum Corporation Limited',
    desc: 'Enterprise web platform built with ASP.NET Core, adhering to WCAG and GIGW 2.1 standards to deliver an accessible and high-performance digital experience.',
    tags: ['ASP.NET Core', 'HTML', 'CSS', 'JavaScript', 'WCAG', 'GIGW 2.1'],
    mockLabel: 'Enterprise Web Platform',
    liveUrl: 'https://www.bharatpetroleum.in/',
  },
  {
    title: 'Bharat PetroResources Limited',
    desc: 'Responsive corporate website designed with modern frontend technologies, focusing on performance, clean design, and seamless user experience.',
    tags: ['HTML', 'CSS', 'JavaScript'],
    mockLabel: 'Corporate Website',
    liveUrl: 'https://bharatpetroresources.in/',
  },
  {
    title: 'BalAsha Trust',
    desc: 'Modern NGO management platform built with Vue.js and Laravel, featuring responsive UI, smooth GSAP animations, API integration, and a Tailwind CSS-powered design.',
    tags: ['Vue.js', 'Laravel', 'Tailwind CSS', 'GSAP', 'Axios'],
    mockLabel: 'NGO Management Platform',
    liveUrl: 'https://uat.balashatrust.org/',
  },
  {
    title: 'Goldmine Advertising Pvt. Ltd.',
    desc: 'Interactive corporate website developed using React and Next.js with immersive GSAP animations and a responsive Tailwind CSS interface.',
    tags: ['React.js', 'Next.js', 'Tailwind CSS', 'GSAP'],
    mockLabel: 'Creative Agency Website',
    liveUrl: 'https://goldmineltd.com/',
  },
  {
    title: 'Sama Birthing and Beyond',
    desc: 'Custom WordPress website designed to provide an engaging and informative experience with an intuitive content management system.',
    tags: ['WordPress'],
    mockLabel: 'Healthcare Website',
    liveUrl: 'https://www.samabirthingandbeyond.com/',
  },
  {
    title: 'Yana Climb',
    desc: 'Responsive landing website built using HTML, CSS, Bootstrap, and JavaScript with a focus on performance and mobile-first design.',
    tags: ['HTML', 'CSS', 'Bootstrap', 'JavaScript'],
    mockLabel: 'Business Landing Page',
    liveUrl: 'https://yanaclimb.com/',
  },
  {
    title: 'Neelam House',
    desc: 'Elegant WordPress website developed with a clean layout, responsive design, and easy content management for business operations.',
    tags: ['WordPress'],
    mockLabel: 'Business Website',
    liveUrl: 'https://www.neelamhouse.com/',
  },
  {
    title: 'Rangeen Akshar',
    desc: 'WordPress-powered website crafted with a vibrant and user-friendly interface, enabling seamless content publishing and management.',
    tags: ['WordPress'],
    mockLabel: 'Creative Website',
    liveUrl: 'https://rangeenakshar.com/',
  },
  {
    title: 'Reset',
    desc: 'Modern no-code website developed in Webflow with responsive layouts, smooth interactions, and visually engaging user experiences.',
    tags: ['Webflow'],
    mockLabel: 'Webflow Website',
    liveUrl: '',
  },
  {
    title: 'BPCL Digital Products',
    desc: 'Developed and maintained digital platforms including My Smart Fleet, PetroBonus, and Convenience, delivering scalable solutions for customer engagement and business operations.',
    tags: ['ASP.NET Core', 'HTML', 'CSS', 'JavaScript', 'Enterprise Solutions'],
    mockLabel: 'Digital Product Suite',
    liveUrl: '',
  },
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

  const cards = cardRefs.value
  cards.forEach((card, i) => {
    if (i === cards.length - 1) return
    const next = cards[i + 1]
    gsap.to(card.querySelector('.stack-inner'), {
      scale: 0.94,
      ease: 'none',
      scrollTrigger: { trigger: next, start: 'top bottom', end: 'top top', scrub: true },
    })
  })
})
</script>
