# Nilesh Rai — Portfolio (Vue 3 + Tailwind CSS + GSAP)

A pixel-faithful conversion of the original single-file HTML portfolio into a
Vue 3 / Vite project, styled with Tailwind CSS v4 and animated with GSAP +
ScrollTrigger + Lenis smooth scroll.

## Stack

- **Vue 3** (Composition API, `<script setup>`)
- **Vite** — build tool
- **Tailwind CSS v4** — base reset and design tokens (`@theme`) live in `src/style.css`; the original design system's bespoke CSS (gradients, masks, keyframes) is kept alongside as plain CSS since it isn't expressible as utility classes without losing pixel accuracy
- **GSAP + ScrollTrigger** — headline/preloader/scroll reveals, sticky stack scale effect, stat counters, bio text-fill scrub, title reveals
- **Lenis** — smooth scrolling, wired into GSAP's ticker and `ScrollTrigger.update`

## Project structure

```
src/
  App.vue               layout shell: grain bg, noise, progress bar, header, hero, preloader orchestration, lenis setup
  gsapSetup.js           single place gsap + ScrollTrigger are registered
  style.css              Tailwind import + design tokens + ported component CSS
  components/
    CustomCursor.vue      cursor dot + contextual label (event-delegated on [data-cursor])
    Preloader.vue         SVG monogram draw-on + iris wipe intro
    About.vue             split-title reveal, bio text-fill scrub, stat counters, timeline reveal
    Work.vue               sticky stacked project cards w/ scroll-driven scale/brightness
    Skills.vue              canvas constellation of skill nodes
    Contact.vue              split-title reveal + contact links
    SiteFooter.vue
```

## Run locally

```bash
npm install
npm run dev       # dev server
npm run build     # production build -> dist/
npm run preview   # preview the production build
```
