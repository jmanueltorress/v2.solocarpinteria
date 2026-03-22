<template>
  <main class="about-page" ref="pageRef">

    <!-- ==============================
         HERO — imagen izq / texto der
         ============================== -->
    <div class="about-hero" :class="{ visible: isVisible }">

      <div class="about-hero-img">
        <img :src="heroImg" alt="Taller Carpintería SOLO" class="hero-img" />
      </div>

      <div class="about-hero-text">
        <p class="about-eyebrow">Somos SOLO.</p>
        <h1 class="about-title">Muebles que<br />cuentan historias.</h1>
        <p class="about-lead">
          Empresa con expreriencia en diseño y fabricación
          de cocinas, closets, muebles de baño y soluciones a medida. Cada proyecto que
          sale de nuestro taller lleva precisión, carácter y el orgullo de hacerlo bien.
        </p>
      </div>

    </div>

    <!-- ==============================
         STATS — fila horizontal debajo
         ============================== -->
    <div class="about-stats-row" :class="{ visible: isVisible }">
      <div class="stat-item">
        <span class="stat-num">+6</span>
        <span class="stat-label">Años de experiencia</span>
      </div>
      <div class="stat-sep"></div>
      <div class="stat-item">
        <span class="stat-num">100%</span>
        <span class="stat-label">Fabricación propia</span>
      </div>
      <div class="stat-sep"></div>
      <div class="stat-item">
        <span class="stat-num">A medida</span>
        <span class="stat-label">Cada proyecto es único</span>
      </div>
    </div>

    <!-- DIVISOR -->
    <div class="about-divider" :class="{ visible: isVisible }"></div>

    <!-- ==============================
         TEXTO SECUNDARIO
         ============================== -->
    <div class="about-body" :class="{ visible: isVisible }">
      <p>
        Nacimos en San Felipe, Guanajuato, con la convicción de que un mueble bien
        hecho transforma un espacio y, con él, la vida de quienes lo habitan.
        No trabajamos en serie: escuchamos, diseñamos y fabricamos pensando en ti.
      </p>
      <p>
        Contamos con taller propio equipado con maquinaria de precisión y un equipo
        comprometido con la calidad en cada detalle, desde el corte hasta la
        instalación final.
      </p>
    </div>

    <!-- DIVISOR -->
    <div class="about-divider" :class="{ visible: isVisible }"></div>

    <!-- ==============================
         VALORES
         ============================== -->
    <div class="about-values" :class="{ visible: isVisible }">
      <p class="values-eyebrow">Lo que nos mueve</p>
      <div class="values-list">
        <div
          v-for="(v, i) in valores"
          :key="i"
          class="value-row"
          :style="{ transitionDelay: `${i * 0.08}s` }"
          :class="{ visible: isVisible }"
        >
          <span class="value-num">0{{ i + 1 }}</span>
          <span class="value-name">{{ v.name }}</span>
          <span class="value-desc">{{ v.desc }}</span>
        </div>
      </div>
    </div>

  </main>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import heroImg from '@/assets/hero-panel-2.jpeg'

const pageRef   = ref(null)
const isVisible = ref(false)

const valores = [
  { name: 'Precisión',  desc: 'Cada milímetro importa. Trabajamos con maquinaria de corte de alta exactitud.' },
  { name: 'Honestidad', desc: 'Precios claros, tiempos reales y comunicación directa desde el primer día.' },
  { name: 'Calidad',    desc: 'Usamos materiales de primera y herrajes que duran. Sin atajos.' },
  { name: 'Compromiso', desc: 'Tu proyecto no termina hasta que estés satisfecho con el resultado final.' },
]

let observer = null

onMounted(() => {
  observer = new IntersectionObserver(
    ([entry]) => { if (entry.isIntersecting) { isVisible.value = true; observer.disconnect() } },
    { threshold: 0.06 }
  )
  if (pageRef.value) observer.observe(pageRef.value)
})

onUnmounted(() => observer?.disconnect())
</script>

<style scoped>
/* ========================
   PÁGINA
   ======================== */
.about-page {
  background: #111;
  color: #fff;
  min-height: 100vh;
  padding: 6rem 5rem;
  max-width: 1200px;
  margin: 0 auto;
  box-sizing: border-box;
}

/* ========================
   HERO — imagen izq + texto der
   ======================== */
.about-hero {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 5rem;
  align-items: center;
  opacity: 0;
  transform: translateY(28px);
  transition: opacity 0.75s ease, transform 0.75s ease;
}

.about-hero.visible {
  opacity: 1;
  transform: translateY(0);
}

/* Imagen */
.about-hero-img {
  aspect-ratio: 4 / 3;
  max-height: 380px;
  overflow: hidden;
  border-radius: 12px;
}

.hero-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center 30%;
  display: block;
  filter: brightness(0.82);
  transition: transform 0.65s ease, filter 0.4s ease;
}

.about-hero-img:hover .hero-img {
  transform: scale(1.04);
  filter: brightness(0.65);
}

/* Texto */
.about-hero-text {
  display: flex;
  flex-direction: column;
  gap: 1.75rem;
}

.about-eyebrow {
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.3);
  margin: 0;
}

.about-title {
  font-size: 4.5rem;
  font-weight: 900;
  line-height: 0.95;
  letter-spacing: -0.03em;
  color: #fff;
  margin: 0;
}

.about-lead {
  font-size: 1rem;
  color: rgba(255, 255, 255, 0.55);
  line-height: 1.8;
  margin: 0;
}

/* ========================
   STATS — fila horizontal
   ======================== */
.about-stats-row {
  display: flex;
  align-items: stretch;
  margin-top: 3.5rem;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  opacity: 0;
  transform: translateY(16px);
  transition: opacity 0.7s ease 0.2s, transform 0.7s ease 0.2s;
}

.about-stats-row.visible {
  opacity: 1;
  transform: translateY(0);
}

.stat-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
  padding: 2rem 0;
}

.stat-sep {
  width: 1px;
  background: rgba(255, 255, 255, 0.1);
  margin: 1.25rem 3rem;
  flex-shrink: 0;
}

.stat-num {
  font-size: 2.4rem;
  font-weight: 900;
  letter-spacing: -0.03em;
  color: #fff;
  line-height: 1;
}

.stat-label {
  font-size: 0.68rem;
  font-weight: 600;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.28);
}

/* ========================
   DIVISOR
   ======================== */
.about-divider {
  height: 1px;
  background: rgba(255, 255, 255, 0.1);
  margin: 3.5rem 0;
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.8s cubic-bezier(0.77, 0, 0.18, 1) 0.2s;
}

.about-divider.visible {
  transform: scaleX(1);
}

/* ========================
   TEXTO SECUNDARIO
   ======================== */
.about-body {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  opacity: 0;
  transform: translateY(16px);
  transition: opacity 0.7s ease 0.15s, transform 0.7s ease 0.15s;
}

.about-body.visible {
  opacity: 1;
  transform: translateY(0);
}

.about-body p {
  font-size: 0.9rem;
  color: rgba(255, 255, 255, 0.42);
  line-height: 1.85;
  margin: 0;
}

/* ========================
   VALORES
   ======================== */
.about-values {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.7s ease 0.25s, transform 0.7s ease 0.25s;
}

.about-values.visible {
  opacity: 1;
  transform: translateY(0);
}

.values-eyebrow {
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.3);
  margin: 0 0 1.5rem;
}

.values-list {
  display: flex;
  flex-direction: column;
}

.value-row {
  display: grid;
  grid-template-columns: 40px 200px 1fr;
  align-items: baseline;
  gap: 2rem;
  padding: 1.25rem 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.07);
  opacity: 0;
  transform: translateX(-16px);
  transition: opacity 0.5s ease, transform 0.5s ease, border-color 0.2s ease;
}

.value-row:first-child {
  border-top: 1px solid rgba(255, 255, 255, 0.07);
}

.value-row.visible {
  opacity: 1;
  transform: translateX(0);
}

.value-row:hover {
  border-bottom-color: rgba(255, 255, 255, 0.18);
}

.value-num {
  font-size: 0.65rem;
  font-weight: 700;
  color: rgba(255, 255, 255, 0.2);
  letter-spacing: 0.08em;
}

.value-name {
  font-size: 0.95rem;
  font-weight: 800;
  color: #fff;
  letter-spacing: -0.01em;
}

.value-desc {
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.42);
  line-height: 1.65;
}

/* ========================
   RESPONSIVE
   ======================== */
@media (max-width: 1024px) {
  .about-page  { padding: 5rem 3.5rem; }
  .about-title { font-size: 3.5rem; }
  .about-hero  { gap: 3.5rem; }
  .stat-sep    { margin: 1.25rem 2rem; }
}

@media (max-width: 768px) {
  .about-page  { padding: 4rem 2rem; }
  .about-title { font-size: 2.8rem; }

  .about-hero {
    grid-template-columns: 1fr;
    gap: 2rem;
  }

  .about-hero-img {
    aspect-ratio: 16 / 9;
    max-height: 260px;
    border-radius: 8px;
  }

  .about-stats-row { flex-wrap: wrap; }
  .stat-item       { flex: 1 1 120px; }
  .stat-sep        { display: none; }

  .about-body {
    grid-template-columns: 1fr;
    gap: 1.25rem;
  }

  .value-row {
    grid-template-columns: 32px 1fr;
    grid-template-rows: auto auto;
    gap: 0.4rem 1rem;
  }

  .value-num  { grid-row: 1; grid-column: 1; }
  .value-name { grid-row: 1; grid-column: 2; }
  .value-desc { grid-row: 2; grid-column: 2; }
}

@media (max-width: 480px) {
  .about-page  { padding: 3.5rem 1.5rem; }
  .about-title { font-size: 2.2rem; }
  .about-lead  { font-size: 0.92rem; }
  .stat-num    { font-size: 1.8rem; }
}

@media (max-width: 340px) {
  .about-title { font-size: 1.9rem; }
}
</style>

<!-- Safari / iOS fix -->
<style scoped>
@media not all and (min-resolution: 0.001dpcm) {
  @supports (-webkit-appearance: none) {
    .about-title     { font-size: 2.6rem; line-height: 1.05; }
    .about-hero      { display: flex; flex-direction: column; }
    .about-hero-img  { width: 100%; aspect-ratio: 16 / 9; max-height: 260px; border-radius: 8px; }
    .about-stats-row { flex-wrap: wrap; }
    .stat-sep        { display: none; }
    .about-body      { display: flex; flex-direction: column; gap: 1.25rem; }
    .value-row       { display: flex; flex-direction: column; gap: 0.35rem; }
  }
}
</style>