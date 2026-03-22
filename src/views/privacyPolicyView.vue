<template>
  <div class="aviso-page">

    <div class="aviso-inner">

      <!-- Encabezado -->
      <div class="aviso-header">
        <p class="aviso-eyebrow">Legal</p>
        <h1 class="aviso-title">Aviso de<br />Privacidad</h1>
        <p class="aviso-intro">
          <strong>Carpintería SOLO</strong> valora y respeta la privacidad de quienes nos contactan.
          La información personal que usted nos proporciona será tratada con confidencialidad
          y conforme a la normativa aplicable.
        </p>
      </div>

      <!-- Divisor -->
      <div class="aviso-divider"></div>

      <!-- Contenido -->
      <div class="aviso-body">

        <div class="aviso-block">
          <p class="block-num">01</p>
          <div class="block-content">
            <h2>Datos personales que se recaban</h2>
            <p>Podemos recabar los siguientes datos a través de nuestros formularios o vía WhatsApp:</p>
            <ul>
              <li>Nombre completo</li>
              <li>Empresa (opcional)</li>
              <li>Número telefónico</li>
              <li>Correo electrónico</li>
              <li>Mensaje o comentarios</li>
            </ul>
          </div>
        </div>

        <div class="aviso-block">
          <p class="block-num">02</p>
          <div class="block-content">
            <h2>Finalidades del tratamiento</h2>
            <p>Los datos recabados serán utilizados para:</p>
            <ul>
              <li>Brindar atención personalizada</li>
              <li>Responder dudas, solicitudes o cotizaciones</li>
              <li>Fines administrativos y de calidad</li>
            </ul>
          </div>
        </div>

        <div class="aviso-block">
          <p class="block-num">03</p>
          <div class="block-content">
            <h2>Transferencia de datos</h2>
            <p>
              No compartimos sus datos personales con terceros, salvo obligación legal
              o requerimiento de una autoridad competente.
            </p>
          </div>
        </div>

        <div class="aviso-block">
          <p class="block-num">04</p>
          <div class="block-content">
            <h2>Uso de WhatsApp y formularios web</h2>
            <p>
              Al contactarnos por estos medios, usted autoriza el uso de sus datos
              conforme a este aviso de privacidad.
            </p>
          </div>
        </div>

        <div class="aviso-block">
          <p class="block-num">05</p>
          <div class="block-content">
            <h2>Modificación y eliminación de datos</h2>
            <p>
              Usted puede solicitar en cualquier momento la modificación o eliminación
              de sus datos personales escribiéndonos a:
              <a href="mailto:carpinteriasolo@gmail.com">carpinteriasolo@gmail.com</a>
            </p>
          </div>
        </div>

        <div class="aviso-block">
          <p class="block-num">06</p>
          <div class="block-content">
            <h2>Cambios al Aviso de Privacidad</h2>
            <p>
              Este aviso puede actualizarse sin previo aviso. Las versiones más recientes
              estarán disponibles en nuestro sitio web.
            </p>
          </div>
        </div>

      </div>

      <!-- Divisor -->
      <div class="aviso-divider"></div>

      <!-- Footer del aviso -->
      <div class="aviso-footer">
        <span class="fecha">Última actualización: Julio 2025</span>
        <router-link to="/" class="back-link">← Volver al inicio</router-link>
      </div>

    </div>

  </div>
</template>

<script setup>
// Componente estático
</script>

<style scoped>
/* ========================
   PÁGINA
   ======================== */
.aviso-page {
  background: #111;
  color: #fff;
  min-height: 100vh;
  padding: 6rem 2rem;
}

.aviso-inner {
  max-width: 780px;
  margin: 0 auto;
}

/* ========================
   ENCABEZADO
   ======================== */
.aviso-header {
  margin-bottom: 3rem;
}

.aviso-eyebrow {
  font-size: 0.65rem;
  font-weight: 700;
  letter-spacing: 0.25em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.3);
  margin: 0 0 1rem;
}

.aviso-title {
  font-size: 4rem;
  font-weight: 900;
  line-height: 1.0;
  letter-spacing: -0.03em;
  color: #fff;
  margin: 0 0 1.75rem;
}

.aviso-intro {
  font-size: 0.95rem;
  color: rgba(255, 255, 255, 0.55);
  line-height: 1.75;
  margin: 0;
  max-width: 560px;
}

.aviso-intro strong {
  color: rgba(255, 255, 255, 0.85);
  font-weight: 700;
}

/* ========================
   DIVISOR
   ======================== */
.aviso-divider {
  height: 1px;
  background: rgba(255, 255, 255, 0.1);
  margin: 2.5rem 0;
}

/* ========================
   BLOQUES
   ======================== */
.aviso-body {
  display: flex;
  flex-direction: column;
  gap: 0;
}

.aviso-block {
  display: grid;
  grid-template-columns: 56px 1fr;
  gap: 1.5rem;
  padding: 2rem 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.07);
  transition: background 0.2s ease;
}

.aviso-block:last-child {
  border-bottom: none;
}

.block-num {
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  color: rgba(255, 255, 255, 0.2);
  margin: 4px 0 0;
  font-variant-numeric: tabular-nums;
}

.block-content h2 {
  font-size: 1rem;
  font-weight: 800;
  color: #fff;
  margin: 0 0 0.65rem;
  letter-spacing: -0.01em;
}

.block-content p {
  font-size: 0.88rem;
  color: rgba(255, 255, 255, 0.55);
  line-height: 1.75;
  margin: 0 0 0.5rem;
}

.block-content p:last-child {
  margin-bottom: 0;
}

.block-content ul {
  margin: 0.5rem 0 0;
  padding-left: 1rem;
  list-style: none;
}

.block-content ul li {
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.45);
  line-height: 1.9;
  position: relative;
  padding-left: 0.75rem;
}

.block-content ul li::before {
  content: '—';
  position: absolute;
  left: -0.5rem;
  color: rgba(255, 255, 255, 0.2);
  font-size: 0.75rem;
}

.block-content a {
  color: rgba(255, 255, 255, 0.75);
  text-decoration: underline;
  text-underline-offset: 3px;
  transition: color 0.2s ease;
}

.block-content a:hover {
  color: #fff;
}

/* ========================
   FOOTER DEL AVISO
   ======================== */
.aviso-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 1rem;
}

.fecha {
  font-size: 0.75rem;
  font-style: italic;
  color: rgba(255, 255, 255, 0.25);
}

.back-link {
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.45);
  text-decoration: none;
  transition: color 0.2s ease;
}

.back-link:hover {
  color: #fff;
}

/* ========================
   RESPONSIVE
   ======================== */
@media (max-width: 768px) {
  .aviso-page  { padding: 4rem 1.5rem; }
  .aviso-title { font-size: 2.8rem; }
  .aviso-block { grid-template-columns: 40px 1fr; gap: 1rem; }
}

@media (max-width: 480px) {
  .aviso-title { font-size: 2.2rem; }
  .aviso-block {
    grid-template-columns: 1fr;
    gap: 0.4rem;
    padding: 1.5rem 0;
  }
  .block-num { margin: 0; }
}

@media (max-width: 340px) {
  .aviso-title { font-size: 1.8rem; }
}
</style>

<!-- Safari / iOS fix -->
<style scoped>
@media not all and (min-resolution: 0.001dpcm) {
  @supports (-webkit-appearance: none) {
    .aviso-title { font-size: 2.6rem; line-height: 1.1; }
    .aviso-block { grid-template-columns: 1fr; }
    .block-num   { display: none; }
  }
}
</style>