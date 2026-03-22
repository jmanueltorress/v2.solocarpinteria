<template>
  <section class="contact-section" id="contacto" ref="sectionRef">

    <!-- COLUMNA IZQUIERDA — Info + Redes + Mapa -->
    <div class="contact-left" :class="{ visible: isVisible }">

      <div class="contact-eyebrow">Contacto</div>
      <h2 class="contact-title">Hablemos de<br />tu proyecto.</h2>
      <p class="contact-sub">
        Cuéntanos qué tienes en mente. Te respondemos rápido y sin compromiso.
      </p>

      <!-- Redes / datos -->
      <div class="contact-links">
        <a href="https://www.facebook.com/share/15ZsJWHdT1/?mibextid=wwXIfr" target="_blank" class="contact-link">
          <span class="link-icon"><i class="fab fa-facebook"></i></span>
          <span class="link-text">Facebook</span>
          <span class="link-arrow">→</span>
        </a>
        <a href="tel:+524826900837" class="contact-link">
          <span class="link-icon"><i class="fas fa-phone"></i></span>
          <span class="link-text">482 690 0837</span>
          <span class="link-arrow">→</span>
        </a>
        <a href="mailto:carpinteriasolo@gmail.com" class="contact-link">
          <span class="link-icon"><i class="fas fa-envelope"></i></span>
          <span class="link-text">carpinteriasolo@gmail.com</span>
          <span class="link-arrow">→</span>
        </a>
      </div>

      <!-- Mapa -->
      <div class="map-wrap">
        <div class="map-label">
          <i class="fas fa-map-marker-alt"></i>
          <span>Visítanos en San Felipe, Gto.</span>
        </div>
        <iframe
          :src="mapURL"
          width="100%"
          height="220"
          style="border:0;"
          allowfullscreen=""
          loading="lazy"
          referrerpolicy="no-referrer-when-downgrade"
        ></iframe>
      </div>

    </div>

    <!-- DIVISOR VERTICAL -->
    <div class="contact-divider" :class="{ visible: isVisible }"></div>

    <!-- COLUMNA DERECHA — Formulario + Video -->
    <div class="contact-right" :class="{ visible: isVisible }">

      <!-- Formulario -->
      <form class="contact-form" @submit.prevent="enviarFormulario" novalidate>

        <div class="form-row">
          <div class="form-field" :class="{ filled: nombre }">
            <input
              type="text"
              v-model="nombre"
              name="Nombre"
              id="nombre"
              placeholder=" "
              required
            />
            <label for="nombre">Nombre *</label>
          </div>

          <div class="form-field" :class="{ filled: empresa }">
            <input
              type="text"
              v-model="empresa"
              name="Empresa"
              id="empresa"
              placeholder=" "
            />
            <label for="empresa">Empresa</label>
          </div>
        </div>

        <div class="form-field" :class="{ filled: telefono }">
          <input
            type="tel"
            v-model="telefono"
            name="Telefono"
            id="telefono"
            placeholder=" "
            required
          />
          <label for="telefono">Número de contacto *</label>
        </div>

        <div class="form-field form-field--textarea" :class="{ filled: mensaje }">
          <textarea
            v-model="mensaje"
            name="Mensaje"
            id="mensaje"
            placeholder=" "
            rows="5"
            required
          ></textarea>
          <label for="mensaje">¿En qué podemos ayudarte? *</label>
        </div>

        <label class="aviso-check">
          <input
            type="checkbox"
            v-model="avisoPrivacidad"
            name="AvisoPrivacidad"
            required
          />
          <span>
            He leído y acepto el
            <router-link to="/avisoPrivacy">Aviso de Privacidad</router-link>.
          </span>
        </label>

        <button type="submit" class="form-submit" :class="{ sending: enviando }">
          <span v-if="!enviando">Enviar mensaje →</span>
          <span v-else>Enviando…</span>
        </button>

        <p v-if="enviado" class="form-success">
          ✓ Mensaje enviado. Te contactaremos pronto.
        </p>
        <p v-if="error" class="form-error">
          ✕ Hubo un problema. Intenta de nuevo.
        </p>

      </form>

      <!-- Video -->
      <div class="video-wrap">
        <div class="video-label">Conoce nuestro taller</div>
        <div class="video-frame">
          <iframe
            width="100%"
            height="100%"
            :src="youtubeURL"
            title="YouTube video player"
            frameborder="0"
            allowfullscreen
          ></iframe>
        </div>
      </div>

    </div>

  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const sectionRef      = ref(null)
const isVisible       = ref(false)
const nombre          = ref('')
const empresa         = ref('')
const telefono        = ref('')
const mensaje         = ref('')
const avisoPrivacidad = ref(false)
const enviando        = ref(false)
const enviado         = ref(false)
const error           = ref(false)

const youtubeURL = "https://www.youtube.com/embed/YOUTUBE_ID"
const mapURL     = "https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3734.427208524124!2d-101.2187871!3d21.4734973!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x842b03682df8d70f%3A0x11fc3f6fddec48a5!2sCarpinteria%20SOLO!5e0!3m2!1ses-419!2smx!4v1719513580000!5m2!1ses-419!2smx"

const enviarFormulario = async () => {
  enviando.value = true
  enviado.value  = false
  error.value    = false

  const formData = new FormData()
  formData.append('Nombre',          nombre.value)
  formData.append('Empresa',         empresa.value)
  formData.append('Telefono',        telefono.value)
  formData.append('Mensaje',         mensaje.value)
  formData.append('AvisoPrivacidad', avisoPrivacidad.value ? 'Aceptado' : 'No aceptado')

  try {
    const response = await fetch('https://formspree.io/f/xblybggg', {
      method: 'POST',
      body: formData,
      headers: { Accept: 'application/json' },
    })

    if (response.ok) {
      enviado.value         = true
      nombre.value          = ''
      empresa.value         = ''
      telefono.value        = ''
      mensaje.value         = ''
      avisoPrivacidad.value = false
    } else {
      error.value = true
    }
  } catch (e) {
    console.error(e)
    error.value = true
  } finally {
    enviando.value = false
  }
}

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
.contact-section {
  display: grid;
  grid-template-columns: 1fr 1px 1fr;
  min-height: 100vh;
  background: #111;
  color: #fff;
}

/* ========================
   COLUMNA IZQUIERDA
   ======================== */
.contact-left {
  padding: 5rem 4rem 5rem 5rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  opacity: 0;
  transform: translateX(-28px);
  transition: opacity 0.7s ease, transform 0.7s ease;
}

.contact-left.visible {
  opacity: 1;
  transform: translateX(0);
}

.contact-eyebrow {
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.35);
}

.contact-title {
  font-size: 3rem;
  font-weight: 900;
  line-height: 1.05;
  color: #fff;
  margin: 0;
  letter-spacing: -0.02em;
}

.contact-sub {
  font-size: 0.95rem;
  color: rgba(255, 255, 255, 0.5);
  line-height: 1.7;
  margin: 0;
  max-width: 340px;
}

/* Links de contacto */
.contact-links {
  display: flex;
  flex-direction: column;
  gap: 0;
  border-top: 1px solid rgba(255, 255, 255, 0.08);
}

.contact-link {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1rem 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  text-decoration: none;
  color: rgba(255, 255, 255, 0.65);
  transition: color 0.25s ease, padding-left 0.25s ease;
}

.contact-link:hover {
  color: #fff;
  padding-left: 6px;
}

.link-icon {
  font-size: 0.95rem;
  width: 20px;
  text-align: center;
  flex-shrink: 0;
  color: rgba(255, 255, 255, 0.35);
}

.link-text {
  flex: 1;
  font-size: 0.87rem;
  font-weight: 500;
}

.link-arrow {
  font-size: 0.85rem;
  opacity: 0;
  transition: opacity 0.2s ease, transform 0.2s ease;
  transform: translateX(-4px);
}

.contact-link:hover .link-arrow {
  opacity: 1;
  transform: translateX(0);
}

/* Mapa */
.map-wrap {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  margin-top: auto;
}

.map-label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.35);
}

.map-wrap iframe {
  display: block;
  filter: grayscale(1) invert(1) contrast(0.85);
  opacity: 0.75;
  transition: opacity 0.3s ease;
}

.map-wrap iframe:hover {
  opacity: 1;
}

/* ========================
   DIVISOR VERTICAL
   ======================== */
.contact-divider {
  background: rgba(255, 255, 255, 0.08);
  transform: scaleY(0);
  transform-origin: top;
  transition: transform 0.8s cubic-bezier(0.77, 0, 0.18, 1) 0.2s;
}

.contact-divider.visible {
  transform: scaleY(1);
}

/* ========================
   COLUMNA DERECHA
   ======================== */
.contact-right {
  padding: 5rem 5rem 5rem 4rem;
  display: flex;
  flex-direction: column;
  gap: 3rem;
  opacity: 0;
  transform: translateX(28px);
  transition: opacity 0.7s ease 0.15s, transform 0.7s ease 0.15s;
}

.contact-right.visible {
  opacity: 1;
  transform: translateX(0);
}

/* ========================
   FORMULARIO
   ======================== */
.contact-form {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.25rem;
}

/* Campo flotante */
.form-field {
  position: relative;
}

.form-field input,
.form-field textarea {
  width: 100%;
  background: transparent;
  border: none;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  color: #fff;
  font-size: 0.95rem;
  padding: 1.4rem 0 0.5rem;
  outline: none;
  transition: border-color 0.25s ease;
  box-sizing: border-box;
  resize: none;
 
}

.form-field input::placeholder,
.form-field textarea::placeholder {
  color: transparent;
}

.form-field input:focus,
.form-field textarea:focus {
  border-bottom-color: #fff;
}

.form-field label {
  position: absolute;
  top: 1.3rem;
  left: 0;
  font-size: 0.82rem;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.35);
  pointer-events: none;
  transition: top 0.25s ease, font-size 0.25s ease, color 0.25s ease;
  letter-spacing: 0.04em;
}

/* Label flota al escribir o enfocar */
.form-field input:focus ~ label,
.form-field input:not(:placeholder-shown) ~ label,
.form-field textarea:focus ~ label,
.form-field textarea:not(:placeholder-shown) ~ label,
.form-field.filled label {
  top: 0.2rem;
  font-size: 0.65rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.45);
}

.form-field--textarea textarea {
  padding-top: 1.6rem;
  line-height: 1.6;
}

/* Aviso de privacidad */
.aviso-check {
  display: flex;
  align-items: flex-start;
  gap: 0.65rem;
  font-size: 0.78rem;
  color: rgba(255, 255, 255, 0.4);
  cursor: pointer;
  line-height: 1.5;
}

.aviso-check input[type="checkbox"] {
  margin-top: 2px;
  accent-color: #fff;
  flex-shrink: 0;
  cursor: pointer;
}

.aviso-check a {
  color: rgba(255, 255, 255, 0.65);
  text-decoration: underline;
  text-underline-offset: 3px;
}

.aviso-check a:hover {
  color: #fff;
}

/* Botón submit */
.form-submit {
  align-self: flex-start;
  padding: 0.8rem 2rem;
  background: #fff;
  color: #111;
  border: none;
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  cursor: pointer;
  border-radius: 2px;
  transition: background 0.2s ease, color 0.2s ease, opacity 0.2s ease;
}

.form-submit:hover {
  background: #e0e0e0;
}

.form-submit.sending {
  opacity: 0.55;
  cursor: not-allowed;
}

.form-success {
  font-size: 0.82rem;
  color: rgba(120, 220, 140, 0.85);
  margin: 0;
}

.form-error {
  font-size: 0.82rem;
  color: rgba(255, 100, 100, 0.85);
  margin: 0;
}

/* ========================
   VIDEO
   ======================== */
.video-wrap {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.video-label {
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.3);
}

.video-frame {
  position: relative;
  width: 100%;
  aspect-ratio: 16 / 9;
  background: #1a1a1a;
  overflow: hidden;
}

.video-frame iframe {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
}

/* ========================
   RESPONSIVE
   ======================== */
@media (max-width: 1024px) {
  .contact-left  { padding: 4rem 3rem 4rem 4rem; }
  .contact-right { padding: 4rem 4rem 4rem 3rem; }
  .contact-title { font-size: 2.4rem; }
}

@media (max-width: 768px) {
  .contact-section {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto auto;
  }

  .contact-divider {
    height: 1px;
    width: 100%;
    transform: scaleX(0);
    transform-origin: left;
  }

  .contact-divider.visible {
    transform: scaleX(1);
  }

  .contact-left  { padding: 4rem 2rem 3rem; }
  .contact-right { padding: 3rem 2rem 4rem; }

  .contact-title { font-size: 2.2rem; }
  .form-row { grid-template-columns: 1fr; }
}

@media (max-width: 480px) {
  .contact-title { font-size: 1.8rem; }
  .contact-left  { padding: 3rem 1.5rem 2.5rem; }
  .contact-right { padding: 2.5rem 1.5rem 3.5rem; }
}

@media (max-width: 340px) {
  .contact-title { font-size: 1.5rem; }
}
</style>

<!-- Safari / iOS fix -->
<style scoped>
@media not all and (min-resolution: 0.001dpcm) {
  @supports (-webkit-appearance: none) {
    .contact-section {
      grid-template-columns: 1fr;
    }

    .contact-divider {
      height: 1px;
      transform: scaleX(0);
      transform-origin: left;
    }

    .contact-divider.visible {
      transform: scaleX(1);
    }

    .contact-left,
    .contact-right {
      padding: 3rem 1.5rem;
    }

    .form-field input,
    .form-field textarea {
      font-size: 1rem;
    }
  }
}
</style>