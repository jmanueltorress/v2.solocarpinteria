<template>
  <section class="cta-section" ref="sectionRef">

    <div class="cta-inner" :class="{ visible: isVisible }">

      <p class="cta-eyebrow">¿Tienes un proyecto en mente?</p>

      <h2 class="cta-title">Hagámoslo<br />realidad.</h2>

      <router-link to="/contact" class="cta-btn">
        <span>Contáctanos</span>
        <span class="btn-arrow">→</span>
      </router-link>

    </div>

    <!-- Línea decorativa animada -->
    <div class="cta-line cta-line--left"  :class="{ visible: isVisible }"></div>
    <div class="cta-line cta-line--right" :class="{ visible: isVisible }"></div>

  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const sectionRef = ref(null)
const isVisible  = ref(false)

let observer = null

onMounted(() => {
  observer = new IntersectionObserver(
    ([entry]) => { if (entry.isIntersecting) { isVisible.value = true; observer.disconnect() } },
    { threshold: 0.2 }
  )
  if (sectionRef.value) observer.observe(sectionRef.value)
})

onUnmounted(() => observer?.disconnect())
</script>

<style scoped>
/* ========================
   SECCIÓN
   ======================== */
.cta-section {
  background: #1a1a1a;
  padding: 7rem 5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  box-sizing: border-box;
}

/* ========================
   CONTENIDO
   ======================== */
.cta-inner {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: 2rem;
  opacity: 0;
  transform: translateY(28px);
  transition: opacity 0.75s ease, transform 0.75s ease;
}

.cta-inner.visible {
  opacity: 1;
  transform: translateY(0);
}

.cta-eyebrow {
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.3);
  margin: 0;
}

.cta-title {
  font-size: 5rem;
  font-weight: 900;
  line-height: 0.92;
  letter-spacing: -0.04em;
  color: #fff;
  margin: 0;
}

/* ========================
   BOTÓN
   ======================== */
.cta-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  margin-top: 0.5rem;
  padding: 0.9rem 2.4rem;
  background: #fff;
  color: #111;
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  text-decoration: none;
  border-radius: 2px;
  transition: background 0.25s ease, color 0.25s ease, gap 0.25s ease;
}

.cta-btn:hover {
  background: #e8e8e8;
  gap: 1.2rem;
}

.btn-arrow {
  display: inline-block;
  transition: transform 0.25s ease;
}

.cta-btn:hover .btn-arrow {
  transform: translateX(4px);
}

/* ========================
   LÍNEAS DECORATIVAS
   ======================== */
.cta-line {
  position: absolute;
  top: 50%;
  height: 1px;
  width: 12%;
  background: rgba(255, 255, 255, 0.1);
  transform: scaleX(0);
  transition: transform 0.8s cubic-bezier(0.77, 0, 0.18, 1) 0.35s;
}

.cta-line--left {
  left: 5%;
  transform-origin: left center;
}

.cta-line--right {
  right: 5%;
  transform-origin: right center;
}

.cta-line.visible {
  transform: scaleX(1);
}

/* ========================
   RESPONSIVE
   ======================== */
@media (max-width: 768px) {
  .cta-section { padding: 5rem 2rem; }
  .cta-title   { font-size: 3.5rem; }
  .cta-line    { display: none; }
}

@media (max-width: 480px) {
  .cta-section { padding: 4rem 1.5rem; }
  .cta-title   { font-size: 2.6rem; }
}

@media (max-width: 340px) {
  .cta-title { font-size: 2rem; }
}
</style>

<!-- Safari / iOS fix -->
<style scoped>
@media not all and (min-resolution: 0.001dpcm) {
  @supports (-webkit-appearance: none) {
    .cta-title { font-size: 3.2rem; line-height: 1.0; }
    .cta-line  { display: none; }
  }
}
</style>