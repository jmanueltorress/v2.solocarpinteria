<template>
  <section class="proyectos-section" id="proyectos" ref="sectionRef">

    <!-- ENCABEZADO -->
    <div class="proyectos-header" :class="{ visible: headerVisible }">
      <p class="header-label">Portafolio</p>
      <h2 class="header-title">Ideas que destacan</h2>
      <p class="header-sub">Cada espacio tiene una historia. Aquí van las nuestras.</p>
    </div>

    <!-- GRID DE PROYECTOS -->
    <div class="proyectos-grid">

      <div
        v-for="(project, i) in projects"
        :key="i"
        class="proyecto-card"
        :class="{ visible: visibleCards.includes(i), 'card-open': activeCard === i }"
        :style="{ transitionDelay: `${(i % 3) * 0.1}s` }"
        @click="toggleCard(i)"
      >
        <div class="card-img-wrap">
          <img :src="project.img" :alt="project.name" class="card-img" />
          <div class="card-img-overlay"></div>
        </div>

        <!-- Info siempre visible -->
        <div class="card-base">
          <span class="card-tag">{{ project.category }}</span>
          <h3 class="card-title">{{ project.name }}</h3>
          <span class="card-toggle" :class="{ rotated: activeCard === i }">↓</span>
        </div>

        <!-- Acordeón -->
        <div class="card-accordion" :class="{ open: activeCard === i }">
          <div class="card-accordion-inner">
            <p class="card-desc">{{ project.desc }}</p>
            <div class="card-meta">
              <div v-for="meta in project.meta" :key="meta.label" class="meta-item">
                <span class="meta-label">{{ meta.label }}</span>
                <span class="meta-value">{{ meta.value }}</span>
              </div>
            </div>
            <a :href="project.link" class="card-cta" @click.stop>Ver proyecto →</a>
          </div>
        </div>

      </div>

    </div>

    <!-- CTA FINAL -->
    <div class="proyectos-footer" :class="{ visible: headerVisible }">
      <router-link to="/contact" class="footer-cta">¿Tu proyecto es el siguiente? Contáctanos</router-link>
    </div>

  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const sectionRef    = ref(null)
const headerVisible = ref(false)
const visibleCards  = ref([])
const activeCard    = ref(null)

const PLACEHOLDER = 'https://placehold.net/600x600.png'

const projects = [
  {
    img:      PLACEHOLDER,
    name:     'Mueble Greg — Monomando',
    category: 'Baño',
    desc:     'Mueble para lavabo modelo Greg en variante monomando. Fabricado en MDF con acabado en melamina blanca y herrajes de cierre suave.',
    meta: [
      { label: 'Modelo',   value: 'Greg' },
      { label: 'Variante', value: 'Monomando' },
      { label: 'Material', value: 'MDF + Melamina' },
    ],
    link: '#bano',
  },
  {
    img:      PLACEHOLDER,
    name:     'Cocina Integral en L',
    category: 'Cocina',
    desc:     'Cocina en L fabricada en melamina con cubierta de ... . Diseño personalizado con alacena hasta el techo y herrajes ocultos.',
    meta: [
      { label: 'Distribución', value: 'En L' },
      { label: 'Cubierta',     value: '...' },
      { label: 'Material',     value: 'Melamina' },
    ],
    link: '#cocinas',
  },
  {
    img:      PLACEHOLDER,
    name:     'Closet Vestidor',
    category: 'Recámara',
    desc:     'Closet a medida con cajones, zapatero y espacio para ropa colgada. Fabricado en melamina con acabado en wengué.',
    meta: [
      { label: 'Tipo',     value: 'Vestidor' },
      { label: 'Acabado',  value: 'Wengué' },
      { label: 'Material', value: 'Melamina' },
    ],
    link: '#recamaras',
  },
  {
    img:      PLACEHOLDER,
    name:     'Mueble Dany — Lavabo Integrado',
    category: 'Baño',
    desc:     'Modelo Dany con lavabo integrado. Líneas limpias, cajones de cierre amortiguado y acabado en melamina polar.',
    meta: [
      { label: 'Modelo',   value: 'Dany' },
      { label: 'Variante', value: 'Lavabo integrado' },
      { label: 'Material', value: 'MDF + Melamina' },
    ],
    link: '#bano',
  },
  {
    img:      PLACEHOLDER,
    name:     'Letrero Corporativo Láser',
    category: 'Publicidad',
    desc:     'Letras volumétricas en MDF de 5mm con corte láser de precisión y acabado pintado. Ideal para fachadas y recepciones.',
    meta: [
      { label: 'Material', value: 'MDF 5mm' },
      { label: 'Técnica',  value: 'Corte láser' },
      { label: 'Acabado',  value: 'Pintado' },
    ],
    link: '#publicidad',
  },
  {
    img:      PLACEHOLDER,
    name:     'Cocina con Isla Central',
    category: 'Cocina',
    desc:     'Cocina de concepto abierto con isla central y barra de desayuno. Melamina blanco mate con cubierta de ... .',
    meta: [
      { label: 'Distribución', value: 'Isla central' },
      { label: 'Cubierta',     value: '...' },
      { label: 'Material',     value: 'Melamina' },
    ],
    link: '#cocinas',
  },
]

const toggleCard = (i) => {
  activeCard.value = activeCard.value === i ? null : i
}

let observer = null

onMounted(() => {
  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          headerVisible.value = true
          projects.forEach((_, i) => {
            setTimeout(() => {
              if (!visibleCards.value.includes(i)) {
                visibleCards.value = [...visibleCards.value, i]
              }
            }, 120 * i)
          })
          observer.disconnect()
        }
      })
    },
    { threshold: 0.1 }
  )
  if (sectionRef.value) observer.observe(sectionRef.value)
})

onUnmounted(() => observer?.disconnect())
</script>

<style scoped>
/* ========================
   SECCIÓN
   ======================== */
.proyectos-section {
  background: #f7f6f3;
  padding: 6rem 4rem 5rem;
}

/* ========================
   ENCABEZADO
   ======================== */
.proyectos-header {
  text-align: center;
  margin-bottom: 4rem;
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.7s ease, transform 0.7s ease;
}

.proyectos-header.visible {
  opacity: 1;
  transform: translateY(0);
}

.header-label {
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: #aaa;
  margin: 0 0 0.75rem;
}

.header-title {
  font-size: 3rem;
  font-weight: 900;
  color: #1a1a1a;
  line-height: 1.05;
  margin: 0 0 0.75rem;
  letter-spacing: -0.02em;
}

.header-sub {
  font-size: 1.05rem;
  color: #888;
  font-style: italic;
  margin: 0;
}

/* ========================
   GRID
   ======================== */
.proyectos-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2px;
  background: #ddd;
}

/* ========================
   CARD
   ======================== */
.proyecto-card {
  background: #fff;
  cursor: pointer;
  overflow: hidden;
  opacity: 0;
  transform: translateY(32px);
  transition: opacity 0.55s ease, transform 0.55s ease, box-shadow 0.3s ease;
}

.proyecto-card.visible {
  opacity: 1;
  transform: translateY(0);
}

.proyecto-card:hover {
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  z-index: 2;
}

/* Imagen */
.card-img-wrap {
  position: relative;
  aspect-ratio: 4 / 3;
  overflow: hidden;
}

.card-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  display: block;
  transition: transform 0.55s ease, filter 0.4s ease;
  filter: brightness(0.92);
}

.proyecto-card:hover .card-img,
.proyecto-card.card-open .card-img {
  transform: scale(1.06);
  filter: brightness(0.75);
}

.card-img-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(to bottom, transparent 50%, rgba(0,0,0,0.25) 100%);
  pointer-events: none;
}

/* Info base */
.card-base {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  padding: 1.1rem 1.25rem 1rem;
  border-bottom: 1px solid #f0f0f0;
}

.card-tag {
  font-size: 0.62rem;
  font-weight: 700;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: #fff;
  background: #1a1a1a;
  padding: 3px 8px;
  border-radius: 2px;
  flex-shrink: 0;
}

.card-title {
  font-size: 0.95rem;
  font-weight: 800;
  color: #1a1a1a;
  margin: 0;
  line-height: 1.2;
  flex: 1;
}

.card-toggle {
  font-size: 0.9rem;
  color: #aaa;
  flex-shrink: 0;
  display: inline-block;
  transition: transform 0.4s cubic-bezier(0.77, 0, 0.18, 1), color 0.2s;
}

.card-toggle.rotated {
  transform: rotate(180deg);
  color: #333;
}

/* Acordeón */
.card-accordion {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.5s cubic-bezier(0.77, 0, 0.18, 1);
}

.card-accordion.open {
  max-height: 320px;
}

.card-accordion-inner {
  padding: 1.1rem 1.25rem 1.4rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.card-desc {
  font-size: 0.87rem;
  color: #555;
  line-height: 1.65;
  margin: 0;
}

/* Meta datos */
.card-meta {
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
  border-left: 2px solid #e8e8e8;
  padding-left: 0.85rem;
}

.meta-item {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  gap: 0.5rem;
}

.meta-label {
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: #bbb;
}

.meta-value {
  font-size: 0.8rem;
  font-weight: 600;
  color: #333;
}

/* CTA */
.card-cta {
  display: inline-block;
  align-self: flex-start;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: #1a1a1a;
  text-decoration: none;
  border-bottom: 1.5px solid #1a1a1a;
  padding-bottom: 2px;
  transition: color 0.2s, border-color 0.2s;
}

.card-cta:hover {
  color: #555;
  border-color: #555;
}

/* ========================
   FOOTER CTA
   ======================== */
.proyectos-footer {
  text-align: center;
  margin-top: 4rem;
  opacity: 0;
  transform: translateY(16px);
  transition: opacity 0.7s ease 0.4s, transform 0.7s ease 0.4s;
}

.proyectos-footer.visible {
  opacity: 1;
  transform: translateY(0);
}

.footer-cta {
  display: inline-block;
  padding: 0.9rem 2.5rem;
  background: #1a1a1a;
  color: #fff;
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  text-decoration: none;
  border-radius: 2px;
  transition: background 0.2s;
}

.footer-cta:hover {
  background: #333;
}

/* ========================
   RESPONSIVE
   ======================== */
@media (max-width: 1024px) {
  .proyectos-section { padding: 5rem 2.5rem 4rem; }
  .header-title      { font-size: 2.4rem; }
}

@media (max-width: 768px) {
  .proyectos-section { padding: 4rem 1.5rem 3rem; }
  .header-title      { font-size: 2rem; }
  .proyectos-grid    { grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 520px) {
  .proyectos-section     { padding: 3.5rem 1rem 3rem; }
  .header-title          { font-size: 1.7rem; }
  .proyectos-grid        { grid-template-columns: 1fr; }
  .card-accordion.open   { max-height: 360px; }
}

@media (max-width: 340px) {
  .header-title { font-size: 1.4rem; }
}
</style>