<template>
  <!-- ==============================
       DESKTOP: efecto scroll sticky
       ============================== -->
  <section v-if="!isMobile" class="hero-container" ref="containerRef">

    <div class="hero-sticky">

      <!-- INTRO -->
      <div class="hero-intro" :class="{ 'slide-out': scrolled }">
        <h1 class="hero-title">
          <span class="title-top">NUESTRO</span>
          <span class="title-bottom">CATÁLOGO</span>
        </h1>
        <p class="hero-tagline">Diseño y fabricación a tu medida, para cada espacio.</p>
        <div class="scroll-hint" aria-hidden="true">
          <span class="scroll-arrow">↓</span>
        </div>
      </div>

      <!-- COLLAGE -->
      <div class="hero-collage" :class="{ 'slide-in': scrolled }">
        <div
          v-for="(item, i) in categories"
          :key="i"
          class="collage-cell"
          :class="[`cell-${i}`, { 'cell-open': activeIndex === i }]"
          @click="toggleAccordion(i)"
        >
          <img :src="item.img" :alt="item.name" class="cell-img" />
          <div class="cell-content">
            <p class="overlay-category">{{ item.tag }}</p>
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
              <div class="catalog-actions">
                <a :href="item.link" class="catalog-btn" @click.stop>Ver catálogo →</a>
                <a
                  v-if="i === 0"
                  href="#"
                  target="_blank"
                  class="catalog-btn-3d"
                  @click.stop
                >Ver diseños 3D →</a>
              </div>
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
      <h1 class="mobile-title">
        <span class="title-top">NUESTRO</span>
        <span class="title-bottom">CATÁLOGO</span>
      </h1>
      <p class="mobile-tagline">Diseño y fabricación a tu medida, para cada espacio.</p>
    </div>

    <div class="mobile-cards">
      <div
        v-for="(item, i) in categories"
        :key="i"
        class="mobile-card"
        :class="{ 'card-open': activeIndex === i }"
        @click="toggleAccordion(i)"
      >
        <img :src="item.img" :alt="item.name" class="mobile-card-img" />
        <div class="mobile-card-overlay">
          <p class="mobile-card-category">{{ item.tag }}</p>
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
            <div class="mobile-catalog-actions">
              <a :href="item.link" class="mobile-catalog-btn" @click.stop>Ver catálogo →</a>
              <a
                v-if="i === 0"
                href="#"
                target="_blank"
                class="mobile-catalog-btn-3d"
                @click.stop
              >Ver diseños 3D →</a>
            </div>
          </div>
        </div>
      </div>
    </div>

  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import panel1 from '@/assets/hero-panel-catalogo-1.jpeg'
import panel2 from '@/assets/hero-panel-catalogo-2.jpeg'
import panel3 from '@/assets/hero-panel-catalogo-3.jpeg'
import panel4 from '@/assets/hero-panel-catalogo-4.jpeg'

const scrolled     = ref(false)
const containerRef = ref(null)
const activeIndex  = ref(null)
const isMobile     = ref(window.innerWidth < 780)

const categories = [
  {
    img: panel1,
    name: 'Muebles para Lavabo',
    tag: 'Baño',
    desc: 'Muebles de lavabo fabricados en MDF de alta calidad con acabado en melamina, disponibles en variantes monomando, placa y lavabo integrado.',
    bullets: [
      'Modelos: Greg, Dany, Nano, Boris, Lía, Fito y Reni',
      'Variantes monomando, placa y lavabo integrado',
      'MDF con acabado en melamina',
    ],
    link: '#bano',
  },
  {
    img: panel2,
    name: 'Closets y Recámaras',
    tag: 'Recámara',
    desc: 'Muebles de dormitorio diseñados para maximizar el espacio y reflejar tu estilo personal.',
    bullets: [
      'Closets y vestidores a medida',
      'Cabeceras tapizadas o en madera',
      'Mesas de noche',
    ],
    link: '#recamaras',
  },
  {
    img: panel3,
    name: 'Cocinas Integrales',
    tag: 'Cocina',
    desc: 'Diseñadas desde cero, con materiales de primera calidad y acabados que duran años.',
    bullets: [
      'Diseño integral personalizado',
      'Isla, barra o en L según tu espacio',
      'Cubierta de cuarzo o granito',
    ],
    link: '#cocinas',
  },
  {
    img: panel4,
    name: 'Publicidad & Decoración',
    tag: 'Corte Láser',
    desc: 'Piezas decorativas y publicitarias en MDF y acrílico con corte láser de precisión para tu negocio u hogar.',
    bullets: [
      'Señalización y letras corporativas',
      'Decoración de interiores en MDF',
      'Letreros led personalizados',
    ],
    link: '#publicidad',
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
   ANIMACIONES DE ENTRADA
   ================================ */

@keyframes fadeDown {
  from { opacity: 0; transform: translateY(-40px); }
  to   { opacity: 1; transform: translateY(0); }
}

@keyframes fadeUp {
  from { opacity: 0; transform: translateY(40px); }
  to   { opacity: 1; transform: translateY(0); }
}

@keyframes zoomInUp {
  from { opacity: 0; transform: scale(0.75) translateY(30px); }
  to   { opacity: 1; transform: scale(1) translateY(0); }
}

.hero-title {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  margin: 0 0 1rem;
  overflow: hidden;
}

.title-top {
  display: block;
  font-size: 3.5rem;
  font-weight: 900;
  color: #1a1a1a;
  line-height: 1.05;
  letter-spacing: -0.01em;
  animation: fadeDown 0.8s cubic-bezier(0.22, 1, 0.36, 1) both;
}

.title-bottom {
  display: block;
  font-size: 3.5rem;
  font-weight: 900;
  color: #1a1a1a;
  line-height: 1.05;
  letter-spacing: -0.01em;
  animation: fadeUp 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.1s both;
}

.hero-tagline {
  font-size: 1.25rem;
  color: #888;
  font-style: italic;
  margin: 0;
  animation: zoomInUp 0.75s cubic-bezier(0.22, 1, 0.36, 1) 0.35s both;
}

.mobile-title {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  margin: 0 0 0.75rem;
  overflow: hidden;
}

.mobile-title .title-top {
  font-size: 1.75rem;
  font-weight: 900;
  color: #1a1a1a;
  line-height: 1.1;
  letter-spacing: -0.01em;
  animation: fadeDown 0.8s cubic-bezier(0.22, 1, 0.36, 1) both;
}

.mobile-title .title-bottom {
  font-size: 1.75rem;
  font-weight: 900;
  color: #1a1a1a;
  line-height: 1.1;
  letter-spacing: -0.01em;
  animation: fadeUp 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.1s both;
}

.mobile-tagline {
  font-size: 1rem;
  color: #888;
  font-style: italic;
  margin: 0;
  animation: zoomInUp 0.75s cubic-bezier(0.22, 1, 0.36, 1) 0.35s both;
}

/* ================================
   DESKTOP — estructura sticky
   ================================ */
.hero-container {
  position: relative;
  height: 220vh;
}

.hero-sticky {
  position: sticky;
  top: 0;
  width: 100%;
  height: 100vh;
  overflow: hidden;
}

.scroll-spacer { height: 120vh; }

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

.collage-cell {
  position: relative;
  flex: 1;
  overflow: hidden;
  cursor: pointer;
  transition: flex 0.55s cubic-bezier(0.77, 0, 0.18, 1);
}

.collage-cell.cell-open { flex: 2.4; }

.cell-0 { clip-path: polygon(0 0, 93% 0, 100% 100%, 0 100%);   margin-right: -3.5%; z-index: 4; }
.cell-1 { clip-path: polygon(0 0, 93% 0, 100% 100%, 7% 100%);  margin-right: -3.5%; z-index: 3; }
.cell-2 { clip-path: polygon(0 0, 93% 0, 100% 100%, 7% 100%);  margin-right: -3.5%; z-index: 2; }
.cell-3 { clip-path: polygon(7% 0, 100% 0, 100% 100%, 0 100%); z-index: 1; }

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
  filter: brightness(0.38);
}

.cell-content {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding: 2rem;
  z-index: 5;
  pointer-events: none;
}

.cell-0 .cell-content { padding-right: 3.5rem; }
.cell-1 .cell-content { padding-left: 4.5rem; padding-right: 3.5rem; }
.cell-2 .cell-content { padding-left: 4.5rem; }

.overlay-category {
  font-size: 0.65rem;
  font-weight: 600;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: rgba(255,255,255,0.5);
  margin: 0 0 0.35rem;
}

.overlay-name {
  font-size: 1.3rem;
  font-weight: 800;
  color: #fff;
  margin: 0;
  line-height: 1.1;
  display: flex;
  align-items: center;
  gap: 0.35rem;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.toggle-icon {
  font-size: 0.85rem;
  color: rgba(255,255,255,0.4);
  display: inline-block;
  flex-shrink: 0;
  transition: transform 0.4s cubic-bezier(0.77, 0, 0.18, 1);
}

.toggle-icon.rotated { transform: rotate(180deg); }

.accordion-divider {
  height: 1px;
  background: rgba(255,255,255,0.15);
  margin: 0.6rem 0 0.75rem;
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
  max-height: 320px;
  opacity: 1;
}

.accordion-body p {
  font-size: 0.85rem;
  color: rgba(255,255,255,0.7);
  line-height: 1.65;
  margin: 0 0 0.6rem;
}

.accordion-body ul {
  margin: 0 0 1rem;
  padding-left: 1.1rem;
  list-style: disc;
}

.accordion-body ul li {
  font-size: 0.8rem;
  color: rgba(255,255,255,0.55);
  line-height: 1.85;
}

/* BOTONES DESKTOP */
.catalog-actions {
  pointer-events: all;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  align-items: flex-start;
}

.catalog-btn {
  display: inline-block;
  padding: 0.5rem 1.2rem;
  background: #fff;
  color: #000;
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  text-decoration: none;
  border-radius: 2px;
  transition: background 0.2s, color 0.2s;
}

.catalog-btn:hover { background: #e0e0e0; }

.catalog-btn-3d {
  display: inline-block;
  padding: 0.5rem 1.2rem;
  background: transparent;
  color: #fff;
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  text-decoration: none;
  border-radius: 2px;
  border: 1px solid rgba(255,255,255,0.5);
  transition: background 0.2s, border-color 0.2s;
}

.catalog-btn-3d:hover {
  background: rgba(255,255,255,0.1);
  border-color: #fff;
}

/* ================================
   MÓVIL — tarjetas estáticas
   ================================ */
.mobile-section {
  width: 100%;
  background: #fff;
}

.mobile-header-block {
  padding: 3rem 1.5rem 2rem;
  text-align: center;
}

.mobile-cards {
  display: flex;
  flex-direction: column;
}

.mobile-card {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
  cursor: pointer;
  transition: height 0.5s cubic-bezier(0.77, 0, 0.18, 1);
}

.mobile-card.card-open {
  height: 430px;
}

.mobile-card-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  display: block;
  filter: brightness(0.55);
  transition: transform 0.4s ease, filter 0.4s ease;
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
  color: rgba(255,255,255,0.55);
  margin: 0 0 0.3rem;
}

.mobile-card-name {
  font-size: 1.25rem;
  font-weight: 800;
  color: #fff;
  margin: 0;
  display: flex;
  align-items: center;
  gap: 0.4rem;
  line-height: 1.1;
}

.mobile-toggle-icon {
  font-size: 0.85rem;
  color: rgba(255,255,255,0.45);
  display: inline-block;
  transition: transform 0.4s cubic-bezier(0.77, 0, 0.18, 1);
}

.mobile-toggle-icon.rotated { transform: rotate(180deg); }

.mobile-accordion-divider {
  height: 1px;
  background: rgba(255,255,255,0.2);
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
  max-height: 290px;
  opacity: 1;
}

.mobile-accordion-body p {
  font-size: 0.85rem;
  color: rgba(255,255,255,0.75);
  line-height: 1.6;
  margin: 0 0 0.5rem;
}

.mobile-accordion-body ul {
  margin: 0 0 0.85rem;
  padding-left: 1rem;
  list-style: disc;
}

.mobile-accordion-body ul li {
  font-size: 0.8rem;
  color: rgba(255,255,255,0.6);
  line-height: 1.8;
}

/* BOTONES MÓVIL */
.mobile-catalog-actions {
  pointer-events: all;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  align-items: flex-start;
}

.mobile-catalog-btn {
  display: inline-block;
  padding: 0.5rem 1.2rem;
  background: #fff;
  color: #000;
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  text-decoration: none;
  border-radius: 2px;
  transition: background 0.2s;
}

.mobile-catalog-btn:hover { background: #e0e0e0; }

.mobile-catalog-btn-3d {
  display: inline-block;
  padding: 0.5rem 1.2rem;
  background: transparent;
  color: #fff;
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  text-decoration: none;
  border-radius: 2px;
  border: 1px solid rgba(255,255,255,0.5);
  transition: background 0.2s, border-color 0.2s;
}

.mobile-catalog-btn-3d:hover {
  background: rgba(255,255,255,0.1);
  border-color: #fff;
}

/* ================================
   DESKTOP responsive
   ================================ */
@media (max-width: 900px) {
  .overlay-name { font-size: 1.1rem; }
}
</style>

<!-- Fixes Safari / iOS -->
<style scoped>
@media not all and (min-resolution: 0.001dpcm) {
  @supports (-webkit-appearance: none) {
    .title-top,
    .title-bottom { font-size: 2.2rem; line-height: 1.15; }
    .hero-tagline  { font-size: 1rem; }
    .cell-0, .cell-1, .cell-2, .cell-3 { clip-path: none; margin: 0; }
    .hero-collage  { gap: 2px; }
    .mobile-card   { -webkit-overflow-scrolling: touch; }
  }
}
</style>