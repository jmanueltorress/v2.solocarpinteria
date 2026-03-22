<template>
  <!-- ==============================
       DESKTOP: efecto scroll sticky
       ============================== -->
  <section v-if="!isMobile" class="hero-container" ref="containerRef">

    <div class="hero-sticky">

      <!-- INTRO -->
      <div class="hero-intro" :class="{ 'slide-out': scrolled }">
        <h2 class="hero-title">DISEÑAMOS Y FABRICAMOS TUS IDEAS</h2>
        <p class="hero-tagline">Explora nuestros servicios y encuentra la solución ideal para ti.</p>
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
          :class="`cell-${i}`"
          @click="i !== 0 && openModal(i)"
        >
          <img :src="item.img" :alt="item.name" class="cell-img" />
          <div class="cell-overlay">
            <div class="overlay-content">
              <p class="overlay-category">{{ item.category }}</p>
              <h3 class="overlay-name">{{ item.name }}</h3>
              <a
                v-if="i === 0"
                href="tel:+524826900837"
                class="overlay-cta overlay-cta--call"
                @click.stop
              >
                <i class="fas fa-phone"></i> Contactar
              </a>
              <span v-else class="overlay-cta">Ver detalle →</span>
            </div>
          </div>
        </div>
      </div>

    </div>

    <div class="scroll-spacer" aria-hidden="true"></div>

    <!-- MODAL desktop -->
    <Transition name="modal-fade">
      <div v-if="activeIndex !== null" class="modal-backdrop" @click.self="closeModal">
        <div class="modal-card">
          <button class="modal-close" @click="closeModal" aria-label="Cerrar">✕</button>
          <div class="modal-img-wrap">
            <img :src="products[activeIndex].img" :alt="products[activeIndex].name" />
          </div>
          <div class="modal-info">
            <p class="modal-category">{{ products[activeIndex].category }}</p>
            <h2 class="modal-name">{{ products[activeIndex].name }}</h2>
            <p class="modal-desc">{{ products[activeIndex].desc }}</p>
            <a href="#catalogo" class="modal-btn" @click="closeModal">Ver catálogo completo</a>
          </div>
        </div>
      </div>
    </Transition>

  </section>

  <!-- ==============================
       MÓVIL: tarjetas estáticas
       ============================== -->
  <section v-else class="mobile-section">

    <div class="mobile-header-block">
      <h2 class="mobile-title">DISEÑAMOS Y FABRICAMOS TUS IDEAS</h2>
      <p class="mobile-tagline">Explora nuestros servicios y encuentra la solución ideal para ti.</p>
    </div>

    <div class="mobile-cards">
      <div
        v-for="(item, i) in products"
        :key="i"
        class="mobile-card"
        @click="i !== 0 && openModal(i)"
      >
        <img :src="item.img" :alt="item.name" class="mobile-card-img" />
        <div class="mobile-card-overlay">
          <p class="mobile-card-category">{{ item.category }}</p>
          <h3 class="mobile-card-name">{{ item.name }}</h3>
          <a
            v-if="i === 0"
            href="tel:+524826900837"
            class="mobile-card-cta mobile-card-cta--call"
            @click.stop
          >
            <i class="fas fa-phone"></i> Contactar
          </a>
          <span v-else class="mobile-card-cta">Ver detalle →</span>
        </div>
      </div>
    </div>

    <!-- MODAL móvil -->
    <Transition name="modal-fade">
      <div v-if="activeIndex !== null" class="modal-backdrop" @click.self="closeModal">
        <div class="modal-card">
          <button class="modal-close" @click="closeModal" aria-label="Cerrar">✕</button>
          <div class="modal-img-wrap">
            <img :src="products[activeIndex].img" :alt="products[activeIndex].name" />
          </div>
          <div class="modal-info">
            <p class="modal-category">{{ products[activeIndex].category }}</p>
            <h2 class="modal-name">{{ products[activeIndex].name }}</h2>
            <p class="modal-desc">{{ products[activeIndex].desc }}</p>
            <a href="#catalogo" class="modal-btn" @click="closeModal">Ver catálogo completo</a>
          </div>
        </div>
      </div>
    </Transition>

  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import panel1 from '@/assets/hero-panel-2-services.jpeg'
import panel2 from '@/assets/hero-panel-1.jpeg'
import panel3 from '@/assets/hero-panel-3-services.jpeg'

const scrolled     = ref(false)
const containerRef = ref(null)
const activeIndex  = ref(null)
const isMobile     = ref(window.innerWidth < 780)

const products = [
  {
    img: panel1,
    name: 'Asesoria Personalizada',
    category: 'Diseño & Fabricación',
    desc: 'Cocinas, closets, muebles de baño y más. Soluciones personalizadas para cada espacio y estilo.',
  },
  {
    img: panel2,
    name: 'Fabricamos a Medida',
    category: 'Muebles ideales',
    desc: 'Cada pieza nace en nuestro taller. Materiales de primera calidad y precisión milimétrica, desde el diseño hasta la entrega.',
  },
  {
    img: panel3,
    name: 'Corte Láser MDF',
    category: 'Decoración & Publicidad',
    desc: 'Diseños personalizados en MDF y acrílico con corte láser. Ideal para señalización, decoración y branding.',
  },
]

const openModal = (i) => {
  activeIndex.value = i
  document.body.style.overflow = 'hidden'
}
const closeModal = () => {
  activeIndex.value = null
  document.body.style.overflow = ''
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
  document.body.style.overflow = ''
})
</script>

<style scoped>
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
}

.cell-0 {
  clip-path: polygon(0 0, 92% 0, 100% 100%, 0 100%);
  margin-right: -4%;
  z-index: 3;
  cursor: default;
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

.cell-1 .cell-overlay {
  padding-left: calc(8% + 2.5rem);
}
.cell-2 .cell-overlay {
  padding-left: calc(8% + 2.5rem);
}

.cell-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center top;
  display: block;
  transition: transform 0.5s ease, filter 0.4s ease;
  filter: brightness(0.75);
}

.collage-cell:hover .cell-img {
  transform: scale(1.05);
  filter: brightness(0.45);
}

/* HOVER OVERLAY */
.cell-overlay {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: flex-end;
  justify-content: flex-start;
  padding: 2rem 2.5rem;
  z-index: 5;
  pointer-events: none;
}

.overlay-content {
  transform: translateY(12px);
  opacity: 0;
  transition: transform 0.35s ease, opacity 0.35s ease;
}

.collage-cell:hover .overlay-content {
  transform: translateY(0);
  opacity: 1;
}

.overlay-category {
  font-size: 0.7rem;
  font-weight: 600;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(255,255,255,0.6);
  margin: 0 0 0.4rem;
}

.overlay-name {
  font-size: 1.5rem;
  font-weight: 800;
  color: #fff;
  margin: 0 0 0.75rem;
  line-height: 1.1;
}

.overlay-cta {
  font-size: 0.8rem;
  font-weight: 600;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: rgba(255,255,255,0.75);
  border-bottom: 1px solid rgba(255,255,255,0.4);
  padding-bottom: 2px;
}

.overlay-cta--call {
  display: inline-flex;
  align-items: center;
  gap: 0.45rem;
  text-decoration: none;
  pointer-events: all;
  transition: color 0.2s ease, border-color 0.2s ease;
}

.overlay-cta--call:hover {
  color: #fff;
  border-color: rgba(255,255,255,0.85);
}

.overlay-cta--call i {
  font-size: 0.75rem;
}

/* ================================
   MODAL (compartido desktop/móvil)
   ================================ */
.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.82);
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1.5rem;
}

.modal-card {
  background: #111;
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 4px;
  max-width: 860px;
  width: 100%;
  display: grid;
  grid-template-columns: 1fr 1fr;
  overflow: hidden;
  position: relative;
}

.modal-close {
  position: absolute;
  top: 1rem;
  right: 1rem;
  background: rgba(255,255,255,0.08);
  border: none;
  color: #fff;
  font-size: 1rem;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
  transition: background 0.2s;
}

.modal-close:hover { background: rgba(255,255,255,0.18); }

.modal-img-wrap {
  aspect-ratio: 3/4;
  overflow: hidden;
}

.modal-img-wrap img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center top;
  display: block;
}

.modal-info {
  padding: 2.5rem 2rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 0.75rem;
}

.modal-category {
  font-size: 0.7rem;
  font-weight: 600;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(255,255,255,0.4);
  margin: 0;
}

.modal-name {
  font-size: 2rem;
  font-weight: 800;
  color: #fff;
  margin: 0;
  line-height: 1.1;
}

.modal-desc {
  font-size: 0.95rem;
  color: rgba(255,255,255,0.6);
  line-height: 1.65;
  margin: 0;
}

.modal-btn {
  display: inline-block;
  margin-top: 0.5rem;
  padding: 0.65rem 1.5rem;
  background: #fff;
  color: #000;
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  text-decoration: none;
  border-radius: 2px;
  align-self: flex-start;
  transition: background 0.2s, color 0.2s;
}

.modal-btn:hover { background: #e0e0e0; }

.modal-fade-enter-active,
.modal-fade-leave-active { transition: opacity 0.3s ease; }
.modal-fade-enter-from,
.modal-fade-leave-to     { opacity: 0; }

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

.mobile-cards {
  display: flex;
  flex-direction: column;
}

.mobile-card {
  position: relative;
  width: 100%;
  height: 220px;
  overflow: hidden;
  cursor: pointer;
}

.mobile-card:first-child {
  cursor: default;
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

.mobile-card:active .mobile-card-img {
  transform: scale(1.03);
  filter: brightness(0.4);
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
  font-size: 1.3rem;
  font-weight: 800;
  color: #fff;
  margin: 0 0 0.5rem;
  line-height: 1.1;
}

.mobile-card-cta {
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: rgba(255,255,255,0.7);
  border-bottom: 1px solid rgba(255,255,255,0.35);
  padding-bottom: 2px;
  align-self: flex-start;
}

.mobile-card-cta--call {
  display: inline-flex;
  align-items: center;
  gap: 0.45rem;
  text-decoration: none;
  transition: color 0.2s ease, border-color 0.2s ease;
}

.mobile-card-cta--call:hover {
  color: #fff;
  border-color: rgba(255,255,255,0.8);
}

.mobile-card-cta--call i {
  font-size: 0.72rem;
}
</style>

<!-- Fixes Safari / iOS -->
<style scoped>
@media not all and (min-resolution: 0.001dpcm) {
  @supports (-webkit-appearance: none) {
    .hero-title   { font-size: 2.2rem; line-height: 1.15; }
    .hero-tagline { font-size: 1rem; }
    .cell-0, .cell-1, .cell-2 { clip-path: none; margin: 0; }
    .cell-1 .cell-overlay,
    .cell-2 .cell-overlay { padding-left: 2.5rem; }
    .hero-collage { gap: 2px; }
    .mobile-card  { -webkit-overflow-scrolling: touch; }
  }
}
</style>