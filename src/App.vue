<script setup lang="ts">
import { onBeforeUnmount, onMounted } from 'vue'

let frame = 0
let pointerX = 0
let pointerY = 0
let rootEl: HTMLElement | null = null

const commitPointer = () => {
  if (!rootEl) return
  rootEl.style.setProperty('--mouse-x', `${pointerX}px`)
  rootEl.style.setProperty('--mouse-y', `${pointerY}px`)
  frame = 0
}

const handlePointerMove = (event: PointerEvent) => {
  pointerX = event.clientX
  pointerY = event.clientY
  if (!frame) {
    frame = window.requestAnimationFrame(commitPointer)
  }
}

const hidePointer = () => {
  if (!rootEl) return
  rootEl.style.setProperty('--mouse-x', `-9999px`)
  rootEl.style.setProperty('--mouse-y', `-9999px`)
}

onMounted(() => {
  rootEl = document.getElementById('app')
  hidePointer()
  window.addEventListener('pointermove', handlePointerMove, { passive: true })
  window.addEventListener('pointerdown', handlePointerMove, { passive: true })
  window.addEventListener('blur', hidePointer)
})

onBeforeUnmount(() => {
  window.removeEventListener('pointermove', handlePointerMove)
  window.removeEventListener('pointerdown', handlePointerMove)
  window.removeEventListener('blur', hidePointer)
  if (frame) {
    window.cancelAnimationFrame(frame)
  }
})
</script>

<template>
  <div></div>
</template>

<style scoped>
.page {
  position: relative;
  z-index: 1;
  width: min(1080px, 90vw);
  margin: 0 auto;
  padding: clamp(3rem, 8vh, 5rem) 0;
  display: grid;
  gap: clamp(2rem, 6vw, 3rem);
}

.hero {
  max-width: 640px;
  display: grid;
  gap: 1.2rem;
}

.eyebrow {
  text-transform: uppercase;
  letter-spacing: 0.3em;
  font-size: 0.7rem;
  color: var(--text-soft);
}

h1 {
  font-family: var(--font-display);
  font-size: clamp(2.5rem, 6vw, 4.4rem);
  line-height: 1.02;
  color: var(--text-strong);
}

.subhead {
  font-size: clamp(1rem, 2.2vw, 1.2rem);
  color: var(--text-soft);
  max-width: 34ch;
}

.actions {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
}

.button {
  border-radius: 999px;
  padding: 0.75rem 1.8rem;
  font-size: 0.95rem;
  letter-spacing: 0.02em;
  border: 1px solid transparent;
  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease,
    border-color 0.2s ease,
    background 0.2s ease;
  cursor: pointer;
}

.button.primary {
  background: linear-gradient(130deg, #7cc0ff 0%, #a887ff 100%);
  color: #0a0d12;
  box-shadow: 0 12px 30px rgba(124, 192, 255, 0.25);
}

.button.ghost {
  background: transparent;
  color: var(--text-strong);
  border-color: rgba(160, 190, 255, 0.4);
}

.button:hover {
  transform: translateY(-2px);
}

.details {
  display: grid;
  gap: 1rem;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
}

.detail-card {
  padding: 1.2rem 1.4rem;
  border-radius: 20px;
  background: rgba(12, 16, 24, 0.65);
  border: 1px solid rgba(160, 190, 255, 0.2);
  backdrop-filter: blur(8px);
}

.detail-label {
  font-size: 0.75rem;
  letter-spacing: 0.24em;
  text-transform: uppercase;
  color: var(--text-soft);
}

.detail-value {
  font-size: 1.05rem;
  margin-top: 0.4rem;
  color: var(--text-strong);
}

@media (min-width: 960px) {
  .page {
    grid-template-columns: minmax(0, 1fr) minmax(0, 320px);
    align-items: end;
  }
}
</style>
