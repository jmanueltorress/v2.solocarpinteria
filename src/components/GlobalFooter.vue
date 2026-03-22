<template>
  <footer class="footer" ref="footerRef">

    <!-- CUERPO PRINCIPAL -->
    <div class="footer-body" :class="{ visible: isVisible }">

      <!-- COLUMNA BRAND — siempre visible -->
      <div class="footer-brand">
        <RouterLink to="/">
          <img src="@/assets/logo-dark.png" alt="Solo Carpintería" class="footer-logo" />
        </RouterLink>
        <p class="footer-tagline">
          Cocinas y Muebles de diseño.<br />San Felipe, Guanajuato.
        </p>
      </div>

      <!-- DIVISOR -->
      <div class="footer-vline"></div>

      <!-- ACORDEONES -->
      <div class="footer-accordions">

        <!-- Redes Sociales -->
        <div class="accordion" :class="{ open: openSection === 'social' }">
          <button class="accordion-header" @click="toggle('social')">
            <span class="accordion-eyebrow">Redes Sociales</span>
            <span class="accordion-icon">
              <i class="fas fa-chevron-down"></i>
            </span>
          </button>
          <div class="accordion-body">
            <div class="accordion-content">
              <a
                href="https://www.facebook.com/share/15ZsJWHdT1/?mibextid=wwXIfr"
                target="_blank"
                class="acc-link"
              >
                <span class="ac-icon"><i class="fab fa-facebook-f"></i></span>
                <span class="ac-text">Facebook</span>
                <span class="ac-arrow">→</span>
              </a>
              <a
                href="https://www.instagram.com/solo_carpinteriasf?igsh=OGF5M25ueTB5djBl"
                target="_blank"
                class="acc-link"
              >
                <span class="ac-icon"><i class="fab fa-instagram"></i></span>
                <span class="ac-text">@solo_carpinteriasf</span>
                <span class="ac-arrow">→</span>
              </a>
            </div>
          </div>
        </div>

        <!-- Navegación -->
        <div class="accordion" :class="{ open: openSection === 'nav' }">
          <button class="accordion-header" @click="toggle('nav')">
            <span class="accordion-eyebrow">Navegación</span>
            <span class="accordion-icon">
              <i class="fas fa-chevron-down"></i>
            </span>
          </button>
          <div class="accordion-body">
            <div class="accordion-content">
              <RouterLink to="/" class="acc-link">
                <span class="ac-icon"><i class="fas fa-home"></i></span>
                <span class="ac-text">Inicio</span>
                <span class="ac-arrow">→</span>
              </RouterLink>
              <RouterLink to="/about" class="acc-link">
                <span class="ac-icon"><i class="fas fa-users"></i></span>
                <span class="ac-text">Nosotros</span>
                <span class="ac-arrow">→</span>
              </RouterLink>
              <RouterLink to="/services" class="acc-link">
                <span class="ac-icon"><i class="fas fa-tools"></i></span>
                <span class="ac-text">Servicios</span>
                <span class="ac-arrow">→</span>
              </RouterLink>
              <RouterLink to="/Products" class="acc-link">
                <span class="ac-icon"><i class="fas fa-box"></i></span>
                <span class="ac-text">Productos</span>
                <span class="ac-arrow">→</span>
              </RouterLink>
              <RouterLink to="/Projects" class="acc-link">
                <span class="ac-icon"><i class="fas fa-layer-group"></i></span>
                <span class="ac-text">Proyectos</span>
                <span class="ac-arrow">→</span>
              </RouterLink>
             
              <RouterLink to="/contact" class="acc-link">
                <span class="ac-icon"><i class="fas fa-paper-plane"></i></span>
                <span class="ac-text">Contacto</span>
                <span class="ac-arrow">→</span>
              </RouterLink>
               <RouterLink to="/store" class="acc-link">
                <span class="ac-icon"><i class="fas fa-store"></i></span>
                <span class="ac-text">Tienda</span>
                <span class="ac-arrow">→</span>
              </RouterLink>
            </div>
          </div>
        </div>

        <!-- Contacto -->
        <div class="accordion" :class="{ open: openSection === 'contact' }">
          <button class="accordion-header" @click="toggle('contact')">
            <span class="accordion-eyebrow">Contacto</span>
            <span class="accordion-icon">
              <i class="fas fa-chevron-down"></i>
            </span>
          </button>
          <div class="accordion-body">
            <div class="accordion-content">
              <a href="tel:+524826900837" class="acc-link">
                <span class="ac-icon"><i class="fas fa-phone"></i></span>
                <span class="ac-text">482 690 0837</span>
                <span class="ac-arrow">→</span>
              </a>
              <a href="mailto:carpinteriasolo@gmail.com" class="acc-link">
                <span class="ac-icon"><i class="fas fa-envelope"></i></span>
                <span class="ac-text">carpinteriasolo@gmail.com</span>
                <span class="ac-arrow">→</span>
              </a>
              <span class="acc-link acc-link--static">
                <span class="ac-icon"><i class="fas fa-map-marker-alt"></i></span>
                <span class="ac-text">San Felipe, Guanajuato</span>
              </span>
            </div>
          </div>
        </div>

      </div>
    </div>

    <!-- LÍNEA DIVISORA -->
    <div class="footer-hline" :class="{ visible: isVisible }"></div>

    <!-- BARRA INFERIOR -->
    <div class="footer-bottom" :class="{ visible: isVisible }">
      <span class="footer-copy">© Solo Carpintería 2026. Todos los derechos reservados.</span>
      <RouterLink to="/avisoPrivacy" class="footer-privacy">Aviso de Privacidad</RouterLink>
      <span class="footer-dev">
        Developed by
        <a href="https://github.com/jmanueltorress" target="_blank" rel="noopener">
          <img src="/images/footer/logo-dev.png" alt="Developer" class="logo-dev" />
        </a>
      </span>
    </div>

  </footer>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const footerRef  = ref(null)
const isVisible  = ref(false)
const openSection = ref(null)

const toggle = (section) => {
  openSection.value = openSection.value === section ? null : section
}

let observer = null

onMounted(() => {
  observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) {
        isVisible.value = true
        observer.disconnect()
      }
    },
    { threshold: 0.08 }
  )
  if (footerRef.value) observer.observe(footerRef.value)
})

onUnmounted(() => observer?.disconnect())
</script>

<style scoped>
/* ========================
   BASE
   ======================== */
.footer {
  background: #111;
  color: #fff;
  overflow: hidden;
}

/* ========================
   CUERPO
   ======================== */
.footer-body {
  display: grid;
  grid-template-columns: 1fr 1px 1.4fr;
  align-items: start;
  padding: 4rem 5rem 3.5rem;
  gap: 0;

  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.7s ease, transform 0.7s ease;
}

.footer-body.visible {
  opacity: 1;
  transform: translateY(0);
}

/* ========================
   BRAND
   ======================== */
.footer-brand {
  padding-right: 3rem;
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.footer-logo {
  height: 52px;
  width: auto;
  display: block;
  /* filter: brightness(0) invert(1); */
  transition: opacity 0.25s ease;
}

.footer-logo:hover { opacity: 0.7; }

.footer-tagline {
  font-size: 0.82rem;
  color: rgba(255, 255, 255, 0.4);
  line-height: 1.8;
  margin: 0;
  max-width: 280px;
}

/* ========================
   DIVISOR VERTICAL
   ======================== */
.footer-vline {
  background: rgba(255, 255, 255, 0.08);
  align-self: stretch;
}

/* ========================
   ACORDEONES
   ======================== */
.footer-accordions {
  padding-left: 3rem;
  display: flex;
  flex-direction: column;
}

.accordion {
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
}

.accordion:first-child {
  border-top: 1px solid rgba(255, 255, 255, 0.08);
}

/* Header del acordeón */
.accordion-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  padding: 1.1rem 0;
  background: none;
  border: none;
  cursor: pointer;
  color: rgba(255, 255, 255, 0.5);
  transition: color 0.25s ease;
}

.accordion-header:hover {
  color: #fff;
}

.accordion.open .accordion-header {
  color: #fff;
}

.accordion-eyebrow {
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.22em;
  text-transform: uppercase;
}

.accordion-icon {
  font-size: 0.7rem;
  color: rgba(255, 255, 255, 0.25);
  transition: transform 0.35s cubic-bezier(0.77, 0, 0.18, 1), color 0.25s ease;
}

.accordion.open .accordion-icon {
  transform: rotate(180deg);
  color: rgba(255, 255, 255, 0.55);
}

/* Cuerpo colapsable */
.accordion-body {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows 0.4s cubic-bezier(0.77, 0, 0.18, 1);
}

.accordion.open .accordion-body {
  grid-template-rows: 1fr;
}

.accordion-content {
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

/* Links dentro del acordeón */
.acc-link {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 0.7rem 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
  text-decoration: none;
  color: rgba(255, 255, 255, 0.45);
  font-size: 0.85rem;
  font-weight: 500;
  transition: color 0.2s ease, padding-left 0.2s ease;
}

.acc-link:last-child {
  border-bottom: none;
  margin-bottom: 0.5rem;
}

.acc-link--static { cursor: default; }

.acc-link:not(.acc-link--static):hover {
  color: #fff;
  padding-left: 5px;
}

.acc-link.router-link-active {
  color: rgba(255, 255, 255, 0.85);
}

.ac-icon {
  font-size: 0.78rem;
  width: 16px;
  text-align: center;
  flex-shrink: 0;
  color: rgba(255, 255, 255, 0.2);
}

.ac-text { flex: 1; }

.ac-arrow {
  font-size: 0.8rem;
  opacity: 0;
  transform: translateX(-4px);
  transition: opacity 0.2s ease, transform 0.2s ease;
}

.acc-link:not(.acc-link--static):hover .ac-arrow {
  opacity: 1;
  transform: translateX(0);
}

/* ========================
   LÍNEA HORIZONTAL
   ======================== */
.footer-hline {
  height: 1px;
  background: rgba(255, 255, 255, 0.08);
  margin: 0 5rem;
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.8s cubic-bezier(0.77, 0, 0.18, 1) 0.3s;
}

.footer-hline.visible { transform: scaleX(1); }

/* ========================
   BARRA INFERIOR
   ======================== */
.footer-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 0.75rem;
  padding: 1.25rem 5rem;

  opacity: 0;
  transition: opacity 0.6s ease 0.5s;
}

.footer-bottom.visible { opacity: 1; }

.footer-copy {
  font-size: 0.72rem;
  color: rgba(255, 255, 255, 0.25);
  letter-spacing: 0.03em;
}

.footer-privacy {
  font-size: 0.72rem;
  color: rgba(255, 255, 255, 0.25);
  text-decoration: none;
  border-bottom: 1px solid transparent;
  transition: color 0.2s ease, border-color 0.2s ease;
  letter-spacing: 0.03em;
}

.footer-privacy:hover {
  color: rgba(255, 255, 255, 0.65);
  border-color: rgba(255, 255, 255, 0.3);
}

.footer-dev {
  display: flex;
  align-items: center;
  gap: 0.45rem;
  font-size: 0.7rem;
  color: rgba(255, 255, 255, 0.2);
}

.logo-dev {
  height: 18px;
  width: auto;
  opacity: 0.4;
  transition: opacity 0.25s ease;
  vertical-align: middle;
}

.logo-dev:hover { opacity: 0.85; }

/* ========================
   RESPONSIVE — 1024px
   ======================== */
@media (max-width: 1024px) {
  .footer-body {
    padding: 3.5rem 3rem 3rem;
  }

  .footer-hline  { margin: 0 3rem; }
  .footer-bottom { padding: 1.25rem 3rem; }
}

/* ========================
   RESPONSIVE — 768px
   ======================== */
@media (max-width: 768px) {
  .footer-body {
    grid-template-columns: 1fr;
    padding: 3rem 2rem 2.5rem;
    gap: 2.5rem;
  }

  .footer-vline { display: none; }

  .footer-brand    { padding-right: 0; }
  .footer-accordions { padding-left: 0; }

  .footer-hline  { margin: 0 2rem; }
  .footer-bottom { padding: 1.25rem 2rem; }
}

/* ========================
   RESPONSIVE — 480px
   ======================== */
@media (max-width: 480px) {
  .footer-body   { padding: 2.5rem 1.5rem 2rem; }
  .footer-hline  { margin: 0 1.5rem; }
  .footer-bottom {
    flex-direction: column;
    align-items: flex-start;
    padding: 1.25rem 1.5rem;
  }
}

/* ========================
   SAFARI FIX
   ======================== */
@media not all and (min-resolution: 0.001dpcm) {
  @supports (-webkit-appearance: none) {
    .footer-body {
      grid-template-columns: 1fr;
      padding: 3rem 1.5rem 2.5rem;
      gap: 2.5rem;
    }

    .footer-vline      { display: none; }
    .footer-brand      { padding-right: 0; }
    .footer-accordions { padding-left: 0; }

    .footer-hline  { margin: 0 1.5rem; }
    .footer-bottom {
      flex-direction: column;
      align-items: flex-start;
      padding: 1rem 1.5rem;
    }
  }
}
</style>