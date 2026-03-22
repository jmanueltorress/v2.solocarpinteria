<template>
  <!-- ==============================
       DESKTOP: efecto scroll sticky
       ============================== -->
  <section v-if="!isMobile" class="hero-container" ref="containerRef">

    <div class="hero-sticky">

      <!-- INTRO -->
      <div class="hero-intro" :class="{ 'slide-out': scrolled }">
        <h1 class="hero-title">COCINAS Y MUEBLES DE DISEÑO</h1>
        <p class="hero-tagline">Del taller a tu hogar, con precisión y carácter.</p>
        <div class="scroll-hint" aria-hidden="true">
          <span class="scroll-arrow">↓</span>
        </div>
      </div>

      <!-- COLLAGE -->
      <div class="hero-collage" :class="{ 'slide-in': scrolled }">
        <div
          v-for="(item, i) in products"
          :key="i"
          class="collage-cell"
          :class="[`cell-${i}`, { 'cell-open': activeIndex === i }]"
          @click="toggleAccordion(i)"
        >
          <img :src="item.img" :alt="item.name" class="cell-img" />
          <div class="cell-content">
            <p class="overlay-category">{{ item.category }}</p>
            <h3 class="overlay-name">
              {{ item.name }}
              <span class="toggle-icon" :class="{ rotated: activeIndex === i }">↑</span>
            </h3>
            <div class="accordion-divider"></div>
            <div class="accordion-body" :class="{ open: activeIndex === i }">
              <p>{{ item.desc }}</p>
              <ul>
                <li v-for="bullet in item.bullets" :key="bullet">{{ bullet }}</li>
              </ul>
            </div>
          </div>
        </div>
      </div>

    </div>

    <div class="scroll-spacer" aria-hidden="true"></div>
  </section>

  <!-- ==============================
       MÓVIL: tarjetas estáticas
       ============================== -->
  <section v-else class="mobile-section">

    <div class="mobile-header-block">
      <h1 class="mobile-title">COCINAS Y MUEBLES DE DISEÑO</h1>
      <p class="mobile-tagline">Del taller a tu hogar, con precisión y carácter.</p>
    </div>

    <div class="mobile-cards">
      <div
        v-for="(item, i) in products"
        :key="i"
        class="mobile-card"
        :class="{ 'card-open': activeIndex === i }"
        @click="toggleAccordion(i)"
      >
        <img :src="item.img" :alt="item.name" class="mobile-card-img" />
        <div class="mobile-card-overlay">
          <p class="mobile-card-category">{{ item.category }}</p>
          <h3 class="mobile-card-name">
            {{ item.name }}
            <span class="mobile-toggle-icon" :class="{ rotated: activeIndex === i }">↑</span>
          </h3>
          <div class="mobile-accordion-divider"></div>
          <div class="mobile-accordion-body" :class="{ open: activeIndex === i }">
            <p>{{ item.desc }}</p>
            <ul>
              <li v-for="bullet in item.bullets" :key="bullet">{{ bullet }}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>

  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import panel1 from '@/assets/mision.jpg'
import panel2 from '@/assets/vision.jpg'
import panel3 from '@/assets/valores.jpg'

const scrolled     = ref(false)
const containerRef = ref(null)
const activeIndex  = ref(null)
const isMobile     = ref(window.innerWidth < 780)

const products = [
  {
    img: panel1,
    name: 'Misión',
    category: 'Nuestra misión',
    desc: 'Diseñar y fabricar cocinas y muebles de alta calidad que transformen cada hogar, combinando funcionalidad, estética y durabilidad.',
    bullets: [
      'Materiales seleccionados con rigor',
      'Procesos artesanales e industriales',
      'Entrega puntual y servicio cercano',
    ],
  },
  {
    img: panel2,
    name: 'Visión',
    category: 'Nuestra visión',
    desc: 'Ser la empresa de diseño de interiores más reconocida de la región por nuestra innovación, calidez humana y compromiso con el cliente.',
    bullets: [
      'Presencia en todo el país para 2030',
      'Referente en diseño sustentable',
      'Equipo humano orgulloso y comprometido',
    ],
  },
  {
    img: panel3,
    name: 'Valores',
    category: 'Nuestros valores',
    desc: 'Los principios que guían cada decisión, desde el taller hasta la entrega en tu hogar.',
    bullets: [
      'Honestidad y transparencia',
      'Excelencia artesanal',
      'Respeto al cliente y al equipo',
      'Innovación con propósito',
    ],
  },
]

const toggleAccordion = (i) => {
  activeIndex.value = activeIndex.value === i ? null : i
}

const onScroll = () => {
  if (!containerRef.value) return
  const { top, height } = containerRef.value.getBoundingClientRect()
  const progress = -top / (height - window.innerHeight)
  scrolled.value = progress > 0.15
}

const onResize = () => {
  isMobile.value = window.innerWidth < 780
}

onMounted(() => {
  window.addEventListener('scroll', onScroll, { passive: true })
  window.addEventListener('resize', onResize)
})

onUnmounted(() => {
  window.removeEventListener('scroll', onScroll)
  window.removeEventListener('resize', onResize)
})
</script>

<style scoped>
/* ================================
   DESKTOP — estructura sticky
   ================================ */
.hero-container {
  position: relative;
  height: 200vh;
}

.hero-sticky {
  position: sticky;
  top: 0;
  width: 100%;
  height: 100vh;
  overflow: hidden;
}

.scroll-spacer {
  height: 100vh;
}

/* INTRO */
.hero-intro {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 2rem;
  background-color: #fff;
  z-index: 2;
  transform: translateX(0);
  opacity: 1;
  transition: transform 0.9s cubic-bezier(0.77, 0, 0.18, 1), opacity 0.7s ease;
}

.hero-intro.slide-out {
  transform: translateX(-100%);
  opacity: 0;
  pointer-events: none;
}

.hero-title {
  font-size: 3.5rem;
  font-weight: 900;
  color: #1a1a1a;
  line-height: 1.05;
  margin: 0 0 1rem;
  letter-spacing: -0.01em;
}

.hero-tagline {
  font-size: 1.25rem;
  color: #888;
  font-style: italic;
  margin: 0;
}

.scroll-hint {
  position: absolute;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
}

.scroll-arrow {
  font-size: 1.5rem;
  color: #aaa;
  animation: bounce 1.8s ease-in-out infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0);   opacity: 0.4; }
  50%       { transform: translateY(8px); opacity: 1;   }
}

/* COLLAGE */
.hero-collage {
  position: absolute;
  inset: 0;
  z-index: 1;
  background: #000;
  display: flex;
  flex-direction: row;
  transform: translateX(100%);
  opacity: 0;
  transition: transform 0.9s cubic-bezier(0.77, 0, 0.18, 1), opacity 0.7s ease;
}

.hero-collage.slide-in {
  transform: translateX(0);
  opacity: 1;
}

/* CELDAS */
.collage-cell {
  position: relative;
  flex: 1;
  overflow: hidden;
  cursor: pointer;
  transition: flex 0.55s cubic-bezier(0.77, 0, 0.18, 1);
}

.collage-cell.cell-open { flex: 2.2; }

.cell-0 {
  clip-path: polygon(0 0, 92% 0, 100% 100%, 0 100%);
  margin-right: -4%;
  z-index: 3;
}
.cell-1 {
  clip-path: polygon(0 0, 92% 0, 100% 100%, 8% 100%);
  margin-right: -4%;
  z-index: 2;
}
.cell-2 {
  clip-path: polygon(8% 0, 100% 0, 100% 100%, 0 100%);
  z-index: 1;
}

.cell-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center top;
  display: block;
  transition: transform 0.5s ease, filter 0.4s ease;
  filter: brightness(0.6);
}

.collage-cell:hover .cell-img,
.collage-cell.cell-open .cell-img {
  transform: scale(1.05);
  filter: brightness(0.4);
}

.cell-content {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding: 2rem 2.5rem;
  z-index: 5;
  pointer-events: none;
}
.cell-0 .cell-content {
  padding-right: 3.5rem;
}

.cell-1 .cell-content {
  padding-left: 4.5rem;
  padding-right: 3.5rem;
}

.cell-2 .cell-content {
  padding-left: 4.5rem;
}
.overlay-category {
  font-size: 0.7rem;
  font-weight: 600;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.55);
  margin: 0 0 0.4rem;
}

.overlay-name {
  font-size: 1.5rem;
  font-weight: 800;
  color: #fff;
  margin: 0;
  line-height: 1.1;
  display: flex;
  align-items: center;
  gap: 0.4rem;
}

.toggle-icon {
  font-size: 0.9rem;
  color: rgba(255, 255, 255, 0.45);
  display: inline-block;
  transition: transform 0.4s cubic-bezier(0.77, 0, 0.18, 1);
}

.toggle-icon.rotated { transform: rotate(180deg); }

.accordion-divider {
  height: 1px;
  background: rgba(255, 255, 255, 0.15);
  margin: 0.65rem 0 0.8rem;
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.4s ease 0.1s;
}

.collage-cell.cell-open .accordion-divider { transform: scaleX(1); }

.accordion-body {
  max-height: 0;
  overflow: hidden;
  opacity: 0;
  transition: max-height 0.55s cubic-bezier(0.77, 0, 0.18, 1), opacity 0.4s ease;
}

.accordion-body.open {
  max-height: 260px;
  opacity: 1;
}

.accordion-body p {
  font-size: 0.9rem;
  color: rgba(255, 255, 255, 0.72);
  line-height: 1.65;
  margin: 0 0 0.65rem;
}

.accordion-body ul {
  margin: 0;
  padding-left: 1.1rem;
  list-style: disc;
}

.accordion-body ul li {
  font-size: 0.82rem;
  color: rgba(255, 255, 255, 0.58);
  line-height: 1.85;
}

/* ================================
   MÓVIL — tarjetas estáticas
   ================================ */
.mobile-section {
  width: 100%;
  background: #fff;
}

/* Bloque de encabezado */
.mobile-header-block {
  padding: 3rem 1.5rem 2rem;
  text-align: center;
  background: #fff;
}

.mobile-title {
  font-size: 1.75rem;
  font-weight: 900;
  color: #1a1a1a;
  line-height: 1.1;
  margin: 0 0 0.75rem;
  letter-spacing: -0.01em;
}

.mobile-tagline {
  font-size: 1rem;
  color: #888;
  font-style: italic;
  margin: 0;
}

/* Tarjetas */
.mobile-cards {
  display: flex;
  flex-direction: column;
  gap: 0;
}

.mobile-card {
  position: relative;
  width: 100%;
  height: 220px;
  overflow: hidden;
  cursor: pointer;
  transition: height 0.5s cubic-bezier(0.77, 0, 0.18, 1);
}

.mobile-card.card-open {
  height: 380px;
}

.mobile-card-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  display: block;
  transition: transform 0.5s ease, filter 0.4s ease;
  filter: brightness(0.55);
}

.mobile-card.card-open .mobile-card-img {
  transform: scale(1.04);
  filter: brightness(0.35);
}

.mobile-card-overlay {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding: 1.5rem;
}

.mobile-card-category {
  font-size: 0.65rem;
  font-weight: 600;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.55);
  margin: 0 0 0.3rem;
}

.mobile-card-name {
  font-size: 1.3rem;
  font-weight: 800;
  color: #fff;
  margin: 0;
  display: flex;
  align-items: center;
  gap: 0.4rem;
}

.mobile-toggle-icon {
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.45);
  display: inline-block;
  transition: transform 0.4s cubic-bezier(0.77, 0, 0.18, 1);
}

.mobile-toggle-icon.rotated { transform: rotate(180deg); }

.mobile-accordion-divider {
  height: 1px;
  background: rgba(255, 255, 255, 0.2);
  margin: 0.6rem 0 0.75rem;
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.4s ease 0.1s;
}

.mobile-card.card-open .mobile-accordion-divider { transform: scaleX(1); }

.mobile-accordion-body {
  max-height: 0;
  overflow: hidden;
  opacity: 0;
  transition: max-height 0.5s cubic-bezier(0.77, 0, 0.18, 1), opacity 0.35s ease;
}

.mobile-accordion-body.open {
  max-height: 200px;
  opacity: 1;
}

.mobile-accordion-body p {
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.75);
  line-height: 1.6;
  margin: 0 0 0.5rem;
}

.mobile-accordion-body ul {
  margin: 0;
  padding-left: 1rem;
  list-style: disc;
}

.mobile-accordion-body ul li {
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.6);
  line-height: 1.8;
}
</style>

<!-- Fixes Safari / iOS -->
<style scoped>
@media not all and (min-resolution: 0.001dpcm) {
  @supports (-webkit-appearance: none) {
    .hero-title   { font-size: 2.2rem; line-height: 1.15; }
    .hero-tagline { font-size: 1rem; }
    .cell-0, .cell-1, .cell-2 { clip-path: none; margin: 0; }
    .hero-collage { gap: 2px; }
    .mobile-card  { -webkit-overflow-scrolling: touch; }
  }
}
</style>