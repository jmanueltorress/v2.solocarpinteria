<template>
  <section class="servicios-section" id="servicios" ref="sectionRef">

    <!-- ENCABEZADO -->
    <div class="servicios-header" :class="{ visible: isVisible }">
      <p class="servicios-eyebrow">Lo que hacemos</p>
      <h2 class="servicios-title">Nuestros<br />Servicios</h2>
    </div>

    <!-- LISTA DE SERVICIOS -->
    <div class="servicios-list">
      <div
        v-for="(servicio, i) in servicios"
        :key="i"
        class="servicio-row"
        :class="{ visible: isVisible, active: activeIndex === i }"
        :style="{ transitionDelay: `${i * 0.09}s` }"
        @mouseenter="activeIndex = i"
        @mouseleave="activeIndex = null"
      >
        <!-- Número -->
        <span class="srv-num">0{{ i + 1 }}</span>

        <!-- Nombre -->
        <h3 class="srv-name">{{ servicio.nombre }}</h3>

        <!-- Tags / características -->
        <div class="srv-tags">
          <span
            v-for="tag in servicio.tags"
            :key="tag"
            class="srv-tag"
          >{{ tag }}</span>
        </div>

        <!-- Descripción -->
        <p class="srv-desc">{{ servicio.descripcion }}</p>

        <!-- CTA -->
        <a :href="servicio.link" class="srv-cta" @click.stop>Ver →</a>

      </div>
    </div>

  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const sectionRef  = ref(null)
const isVisible   = ref(false)
const activeIndex = ref(null)

const servicios = [
  {
    nombre:      'Baños',
    descripcion: 'Muebles de lavabo en variantes monomando, placa y lavabo integrado, fabricados en MDF de alta calidad con acabado en melamina. Disponibles en distintos modelos y colores.',
    tags:        ['Greg', 'Dany', 'Nano', 'Boris', 'Lía', 'Fito', 'Reni'],
    link:        '#bano',
  },
  {
    nombre:      'Cocinas',
    descripcion: 'Cocinas integrales en L, U o con isla central fabricadas en melamina. La cubierta puede ser de ... según tu preferencia.',
    tags:        ['Integral', 'En L', 'En U', 'Isla central'],
    link:        '#cocinas',
  },
  {
    nombre:      'Recámaras',
    descripcion: 'Closets, vestidores y cabeceras fabricados en melamina que aprovechan cada centímetro y se adaptan a tu estilo de vida.',
    tags:        ['Closets', 'Vestidores', 'Cabeceras'],
    link:        '#recamaras',
  },
  {
    nombre:      'Decoración & Publicidad',
    descripcion: 'Corte láser en MDF y acrílico para señalización, displays, letras corporativas y piezas decorativas únicas.',
    tags:        ['Corte láser', 'Señalización', 'MDF', 'Acrílico'],
    link:        '#publicidad',
  },
]

let observer = null

onMounted(() => {
  observer = new IntersectionObserver(
    ([entry]) => { if (entry.isIntersecting) { isVisible.value = true; observer.disconnect() } },
    { threshold: 0.08 }
  )
  if (sectionRef.value) observer.observe(sectionRef.value)
})

onUnmounted(() => observer?.disconnect())
</script>

<style scoped>
/* ========================
   SECCIÓN
   ======================== */
.servicios-section {
  background: #f7f6f3;
  padding: 6rem 5rem;
  box-sizing: border-box;
}

/* ========================
   ENCABEZADO
   ======================== */
.servicios-header {
  margin-bottom: 3.5rem;
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.7s ease, transform 0.7s ease;
}

.servicios-header.visible {
  opacity: 1;
  transform: translateY(0);
}

.servicios-eyebrow {
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: #aaa;
  margin: 0 0 0.85rem;
}

.servicios-title {
  font-size: 3.5rem;
  font-weight: 900;
  line-height: 0.95;
  letter-spacing: -0.03em;
  color: #1a1a1a;
  margin: 0;
}

/* ========================
   FILAS DE SERVICIOS
   ======================== */
.servicios-list {
  display: flex;
  flex-direction: column;
  border-top: 1px solid rgba(0, 0, 0, 0.1);
}

.servicio-row {
  display: grid;
  grid-template-columns: 48px 220px 1fr 1fr 80px;
  align-items: center;
  gap: 2rem;
  padding: 1.75rem 1.5rem;
  border-bottom: 1px solid rgba(0, 0, 0, 0.08);
  cursor: default;
  position: relative;
  opacity: 0;
  transform: translateX(-20px);
  transition:
    opacity 0.55s ease,
    transform 0.55s ease,
    border-color 0.4s ease;
}

/* Capa de fondo animada — se expande con scaleX */
.servicio-row::before {
  content: '';
  position: absolute;
  inset: 0;
  background: #1a1a1a;
  transform: scaleX(0);
  transform-origin: left center;
  transition: transform 0.45s cubic-bezier(0.77, 0, 0.18, 1);
  z-index: 0;
  pointer-events: none;
}

.servicio-row.visible {
  opacity: 1;
  transform: translateX(0);
}

.servicio-row.active {
  border-color: transparent;
}

.servicio-row.active::before {
  transform: scaleX(1);
}

/* Todo el contenido sobre la capa */
.servicio-row > * {
  position: relative;
  z-index: 1;
}

/* Número */
.srv-num {
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  color: #ccc;
  transition: color 0.45s ease;
}

.servicio-row.active .srv-num {
  color: rgba(255, 255, 255, 0.25);
}

/* Nombre */
.srv-name {
  font-size: 1.25rem;
  font-weight: 900;
  color: #1a1a1a;
  margin: 0;
  letter-spacing: -0.02em;
  white-space: nowrap;
  transition: color 0.45s ease;
}

.servicio-row.active .srv-name {
  color: #fff;
}

/* Tags */
.srv-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
}

.srv-tag {
  font-size: 0.62rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: #888;
  background: rgba(0, 0, 0, 0.06);
  padding: 3px 9px;
  border-radius: 2px;
  transition: background 0.45s ease, color 0.45s ease;
}

.servicio-row.active .srv-tag {
  background: rgba(255, 255, 255, 0.1);
  color: rgba(255, 255, 255, 0.55);
}

/* Descripción */
.srv-desc {
  font-size: 0.85rem;
  color: #888;
  line-height: 1.6;
  margin: 0;
  transition: color 0.45s ease;
}

.servicio-row.active .srv-desc {
  color: rgba(255, 255, 255, 0.5);
}

/* CTA */
.srv-cta {
  justify-self: end;
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: #aaa;
  text-decoration: none;
  border-bottom: 1px solid rgba(0, 0, 0, 0.15);
  padding-bottom: 2px;
  white-space: nowrap;
  transition: color 0.45s ease, border-color 0.45s ease;
}

.servicio-row.active .srv-cta {
  color: #fff;
  border-bottom-color: rgba(255, 255, 255, 0.35);
}

.srv-cta:hover {
  color: #333;
}

.servicio-row.active .srv-cta:hover {
  color: rgba(255, 255, 255, 0.75);
}

/* ========================
   RESPONSIVE
   ======================== */
@media (max-width: 1100px) {
  .servicios-section { padding: 5rem 3.5rem; }
  .servicio-row {
    grid-template-columns: 40px 180px 1fr 80px;
    grid-template-rows: auto auto;
  }
  .srv-tags  { grid-column: 2 / 4; grid-row: 2; margin-top: -0.5rem; }
  .srv-desc  { display: none; }
}

@media (max-width: 768px) {
  .servicios-section { padding: 4rem 2rem; }
  .servicios-title   { font-size: 2.6rem; }

  .servicio-row {
    grid-template-columns: 36px 1fr auto;
    grid-template-rows: auto auto;
    gap: 0.5rem 1rem;
    padding: 1.4rem 1rem;
  }

  .srv-num  { grid-row: 1; grid-column: 1; align-self: start; padding-top: 3px; }
  .srv-name { grid-row: 1; grid-column: 2; font-size: 1.1rem; white-space: normal; }
  .srv-cta  { grid-row: 1; grid-column: 3; justify-self: end; }
  .srv-tags { grid-row: 2; grid-column: 2 / 4; }
  .srv-desc { display: none; }
}

@media (max-width: 480px) {
  .servicios-section { padding: 3.5rem 1.5rem; }
  .servicios-title   { font-size: 2rem; }
  .servicio-row      { padding: 1.25rem 0.75rem; }
}

@media (max-width: 340px) {
  .servicios-title { font-size: 1.7rem; }
}
</style>

<!-- Safari / iOS fix -->
<style scoped>
@media not all and (min-resolution: 0.001dpcm) {
  @supports (-webkit-appearance: none) {
    .servicios-title { font-size: 2.4rem; line-height: 1.05; }
    .servicio-row {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      gap: 0.5rem;
    }
    .srv-tags { flex-wrap: wrap; }
    .srv-desc { display: block; }
    .srv-cta  { align-self: flex-start; }
  }
}
</style>