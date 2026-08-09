<template>
  <!-- Hero Section -->
  <section class="hero">
    <div class="container">
      <div class="hero-inner">
        <div class="hero-animation" ref="heroAnimation">
          <!-- Accent lines -->
          <div class="accent-line left" ref="lineLeft"></div>
          <div class="accent-line right" ref="lineRight"></div>
          <!-- Logo -->
          <div class="logo-wrap" ref="logoWrap">
            <svg viewBox="0 0 524 586">
              <path ref="logoFrame" class="logo-frame" fill="#ffffff" fill-rule="evenodd" d="M262,22 L492.8,155.25 L492.8,421.75 L262,555 L31.2,421.75 L31.2,155.25 Z M262,84 L439.1,186.25 L439.1,390.75 L262,493 L84.9,390.75 L84.9,186.25 Z"/>
              <g ref="logoHexes">
                <polygon class="logo-hex" points="211.11,146.74 255.28,172.24 255.28,223.24 211.11,248.74 166.94,223.24 166.94,172.24" fill="#ffffff"/>
                <polygon class="logo-hex" points="315.85,146.72 360.01,172.22 360.01,223.22 315.85,248.72 271.68,223.22 271.68,172.22" fill="#ffffff"/>
                <polygon class="logo-hex" points="159.06,237.57 203.23,263.07 203.23,314.07 159.06,339.57 114.9,314.07 114.9,263.07" fill="#ffffff"/>
                <polygon class="logo-hex" points="263.14,237.57 307.31,263.07 307.31,314.07 263.14,339.57 218.98,314.07 218.98,263.07" fill="#ffffff"/>
                <polygon class="logo-hex" points="367.56,237.57 411.73,263.07 411.73,314.07 367.56,339.57 323.39,314.07 323.39,263.07" fill="#ffffff"/>
                <polygon class="logo-hex" points="211.13,327.96 255.3,353.46 255.3,404.46 211.13,429.96 166.96,404.46 166.96,353.46" fill="#ffffff"/>
                <polygon class="logo-hex" points="315.84,327.95 360.01,353.45 360.01,404.45 315.84,429.95 271.67,404.45 271.67,353.45" fill="#ffffff"/>
              </g>
            </svg>
          </div>
          <!-- Text -->
          <div class="text-wrap">
            <div class="brand-text" ref="brandText">devEco™</div>
            <div class="tagline" ref="tagline">Community</div>
          </div>
        </div>

        <div class="hero-content">
          <p class="label">Developer Community</p>
          <h1>Where makers actually make things.</h1>
          <p class="hero-subtitle">
            Not a Slack that goes quiet after launch. A real community of hardware hackers, DevRel practitioners, and builders.
          </p>
          <div class="hero-cta">
            <a href="https://discord.gg/deveco" target="_blank" class="btn btn-primary">Join Discord</a>
            <router-link to="/team" class="btn btn-secondary">View Products</router-link>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Values Section -->
  <section class="values-section">
    <div class="container">
      <div class="section-header">
        <h2>Community Values</h2>
      </div>

      <div class="values-grid">
        <div class="card value-card">
          <span class="stamp stamp-teal">Safety</span>
          <h3>Moderated Spaces</h3>
          <p>Active moderation with clear guidelines. We've run hackerspaces and know what keeps communities healthy.</p>
        </div>

        <div class="card value-card">
          <span class="stamp stamp-pink">Authentic</span>
          <h3>Real Connections</h3>
          <p>No spam, no bad actors. Just developers and makers building genuine relationships.</p>
        </div>

        <div class="card value-card">
          <span class="stamp stamp-navy">Global</span>
          <h3>Inclusive Community</h3>
          <p>Developers from all backgrounds, skill levels, and locations. Diversity makes us stronger.</p>
        </div>

        <div class="card value-card">
          <span class="stamp stamp-teal">Open</span>
          <h3>Knowledge Sharing</h3>
          <p>Open collaboration. What helps one developer succeed helps the entire ecosystem.</p>
        </div>

        <div class="card value-card">
          <span class="stamp stamp-pink">Creative</span>
          <h3>Innovation Focus</h3>
          <p>Experimentation encouraged. Cutting-edge technology exploration in a supportive environment.</p>
        </div>

        <div class="card value-card">
          <span class="stamp stamp-navy">Active</span>
          <h3>24/7 Presence</h3>
          <p>Global timezones mean someone's always around. Get help when you need it.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- Events Section -->
  <section class="events-section">
    <div class="container">
      <div class="section-header">
        <h2>Community Events</h2>
      </div>

      <div class="events-grid">
        <div class="card event-card">
          <span class="badge badge-teal">Live Streams</span>
          <h3>PROCEPTION Live</h3>
          <p>Live-streamed sessions with industry experts and hands-on tech demos. Cutting-edge tech with guided walkthroughs.</p>
          <p class="event-schedule">Monthly on YouTube & Discord</p>
        </div>

        <div class="card event-card">
          <span class="badge badge-pink">Workshops</span>
          <h3>Developer Skills Sessions</h3>
          <p>Hands-on workshops covering technical skills to community building. Learn from practitioners.</p>
          <p class="event-schedule">Bi-weekly, various topics</p>
        </div>

        <div class="card event-card">
          <span class="badge badge-navy">Collaboration</span>
          <h3>Cross-Community Projects</h3>
          <p>Members from different communities work together on innovative projects and challenges.</p>
          <p class="event-schedule">Ongoing, project-based</p>
        </div>

        <div class="card event-card">
          <span class="badge badge-teal">Networking</span>
          <h3>Developer Meetups</h3>
          <p>Virtual and in-person networking events. Connect, share experiences, build relationships.</p>
          <p class="event-schedule">Weekly, global timezones</p>
        </div>
      </div>
    </div>
  </section>

  <!-- CTA Section -->
  <section class="cta-section">
    <div class="container">
      <h2>Ready to connect?</h2>
      <p>Join developers from around the world. No gatekeeping.</p>
      <div class="cta-buttons">
        <a href="https://discord.gg/deveco" target="_blank" class="btn btn-primary">Join Our Discord</a>
        <router-link to="/consultancy" class="btn btn-secondary">Explore Consulting</router-link>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'

const heroAnimation = ref<HTMLElement | null>(null)
const logoWrap = ref<HTMLElement | null>(null)
const logoFrame = ref<SVGPathElement | null>(null)
const logoHexes = ref<SVGGElement | null>(null)
const lineLeft = ref<HTMLElement | null>(null)
const lineRight = ref<HTMLElement | null>(null)
const brandText = ref<HTMLElement | null>(null)
const tagline = ref<HTMLElement | null>(null)

let colorFlashInterval: number | null = null
const colors = ['#4db3a8', '#e85a85', '#f4c84b', '#5bc5e8', '#6ab4a8']

const createParticles = () => {
  if (!heroAnimation.value) return
  // Remove old particles
  heroAnimation.value.querySelectorAll('.particle').forEach(p => p.remove())
  // Create new particles directly in heroAnimation (like the original)
  for (let i = 0; i < 16; i++) {
    const particle = document.createElement('div')
    particle.className = 'particle'
    particle.style.top = '35%'
    particle.style.left = '50%'
    const angle = (i / 16) * Math.PI * 2
    const distance = 80 + Math.random() * 120
    const x = Math.cos(angle) * distance
    const y = Math.sin(angle) * distance
    particle.style.setProperty('--end-pos', `translate(${x}px, ${y}px)`)
    particle.style.background = colors[Math.floor(Math.random() * colors.length)] || '#4db3a8'
    heroAnimation.value.appendChild(particle)
  }
}

const burstParticles = () => {
  heroAnimation.value?.querySelectorAll('.particle').forEach((p, i) => {
    setTimeout(() => p.classList.add('burst'), i * 15)
  })
}

const startColorFlash = () => {
  const hexes = logoHexes.value?.querySelectorAll('.logo-hex') || []
  colorFlashInterval = window.setInterval(() => {
    const hex = hexes[Math.floor(Math.random() * hexes.length)] as SVGPolygonElement
    const color = colors[Math.floor(Math.random() * colors.length)]
    if (hex && color) {
      hex.style.fill = color
      hex.classList.add('color-flash')
      setTimeout(() => {
        hex.style.fill = ''
        hex.classList.remove('color-flash')
      }, 250)
    }
  }, 700)
}

const stopColorFlash = () => {
  if (colorFlashInterval) {
    clearInterval(colorFlashInterval)
    colorFlashInterval = null
  }
}

const runAnimation = () => {
  createParticles()
  const hexes = logoHexes.value?.querySelectorAll('.logo-hex') || []

  // T=100ms: Frame slam in
  setTimeout(() => {
    logoFrame.value?.classList.add('slam')
  }, 100)

  // T=400ms: Hexes pop in rapid fire
  setTimeout(() => {
    hexes.forEach((hex, i) => {
      const el = hex as SVGPolygonElement
      setTimeout(() => {
        el.classList.add('pop')
        // Add visible class after animation completes (300ms)
        setTimeout(() => {
          el.classList.add('visible')
        }, 300)
      }, i * 50)
    })
  }, 400)

  // T=800ms: Glitch + particles
  setTimeout(() => {
    logoWrap.value?.classList.add('glitch')
    burstParticles()
  }, 800)

  // T=1000ms: Lines shoot
  setTimeout(() => {
    lineLeft.value?.classList.add('shoot')
    lineRight.value?.classList.add('shoot')
  }, 1000)

  // T=1100ms: Brand text slides
  setTimeout(() => {
    brandText.value?.classList.add('slide')
  }, 1100)

  // T=1300ms: Tagline slides
  setTimeout(() => {
    tagline.value?.classList.add('slide')
  }, 1300)

  // T=1600ms: Start color shift
  setTimeout(() => {
    hexes.forEach((hex, i) => {
      const el = hex as SVGPolygonElement
      el.style.animationDelay = `${i * 0.12}s`
      el.classList.add('shift')
    })
  }, 1600)

  // T=2200ms: Start random color flashes
  setTimeout(() => {
    startColorFlash()
  }, 2200)
}

onMounted(() => {
  runAnimation()
})

onUnmounted(() => {
  stopColorFlash()
})
</script>

<style>
/* Animation styles (unscoped for SVG elements) */

/* Frame animation */
.logo-frame {
  opacity: 0;
  transform-origin: center;
}

.logo-frame.slam {
  animation: communityFrameSlamIn 0.4s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
}

@keyframes communityFrameSlamIn {
  0% { opacity: 0; transform: scale(2) rotate(30deg); }
  100% { opacity: 1; transform: scale(1) rotate(0deg); }
}

/* Hex animations */
.logo-hex {
  transform-origin: center;
  opacity: 0;
}

.logo-hex.pop {
  animation: communityHexPop 0.3s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
}

@keyframes communityHexPop {
  0% { opacity: 0; transform: scale(0) rotate(-180deg); }
  100% { opacity: 1; transform: scale(1) rotate(0deg); }
}

/* Keep hexes visible after pop */
.logo-hex.visible {
  opacity: 1;
}

/* Color shift */
.logo-hex.shift {
  opacity: 1;
  animation: communityColorShift 2s ease-in-out infinite;
}

@keyframes communityColorShift {
  0%, 100% { fill: #ffffff; }
  50% { fill: #4db3a8; }
}

/* Color flash on hexes */
.logo-hex.color-flash {
  opacity: 1;
  animation: communityHexColorFlash 0.25s ease-out forwards !important;
}

@keyframes communityHexColorFlash {
  0% { transform: scale(1); }
  50% { transform: scale(1.15); }
  100% { transform: scale(1); }
}

/* Glitch effect */
.logo-wrap.glitch {
  animation: communityGlitch 0.3s ease-out;
}

@keyframes communityGlitch {
  0% { transform: translate(0, 0); }
  20% { transform: translate(2px, 0); filter: hue-rotate(90deg); }
  40% { transform: translate(-2px, 1px); filter: hue-rotate(-90deg); }
  60% { transform: translate(1px, -1px); filter: hue-rotate(45deg); }
  80% { transform: translate(-1px, 0); filter: hue-rotate(0deg); }
  100% { transform: translate(0, 0); filter: hue-rotate(0deg); }
}

/* Particle styles (unscoped) */
.particle {
  position: absolute;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  opacity: 0;
}

.particle.burst {
  animation: communityParticleBurst 0.8s ease-out forwards;
}

@keyframes communityParticleBurst {
  0% { opacity: 1; transform: translate(0, 0) scale(1); }
  100% { opacity: 0; transform: var(--end-pos) scale(0); }
}
</style>

<style scoped>
.hero {
  background: var(--teal);
  color: white;
  padding: var(--space-2xl) 0;
  overflow: hidden;
}

.hero-inner {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--space-xl);
  align-items: center;
}

.hero-animation {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 320px;
  overflow: hidden;
}

.logo-wrap {
  position: relative;
  width: 180px;
  height: 200px;
  z-index: 2;
}

.logo-wrap svg {
  width: 100%;
  height: 100%;
}

.text-wrap {
  text-align: center;
  margin-top: var(--space-md);
  z-index: 2;
}

.brand-text {
  font-family: 'JetBrains Mono', monospace;
  font-size: 42px;
  font-weight: 700;
  color: #ffffff;
  letter-spacing: -1px;
  opacity: 0;
  transform: translateY(30px);
}

.brand-text.slide {
  animation: textSlide 0.5s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
}

@keyframes textSlide {
  0% { opacity: 0; transform: translateY(30px); }
  100% { opacity: 1; transform: translateY(0); }
}

.tagline {
  font-family: 'JetBrains Mono', monospace;
  font-size: 14px;
  font-weight: 600;
  color: #ffffff;
  opacity: 0;
  transform: translateY(20px);
  margin-top: -2px;
  letter-spacing: 0.1em;
}

.tagline.slide {
  animation: tagSlide 0.4s ease-out forwards;
}

@keyframes tagSlide {
  0% { opacity: 0; transform: translateY(20px); }
  100% { opacity: 1; transform: translateY(0); }
}

.accent-line {
  position: absolute;
  height: 3px;
  width: 150px;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.8), transparent);
  opacity: 0;
}

.accent-line.left {
  top: 40%;
  left: 0;
}

.accent-line.right {
  top: 40%;
  right: 0;
}

.accent-line.left.shoot {
  animation: lineShootLeft 0.6s ease-out forwards;
}

.accent-line.right.shoot {
  animation: lineShootRight 0.6s ease-out forwards;
}

@keyframes lineShootLeft {
  0% { opacity: 1; transform: translateX(-100%); }
  50% { opacity: 1; transform: translateX(100%); }
  100% { opacity: 0; transform: translateX(200%); }
}

@keyframes lineShootRight {
  0% { opacity: 1; transform: translateX(100%); }
  50% { opacity: 1; transform: translateX(-100%); }
  100% { opacity: 0; transform: translateX(-200%); }
}

.hero-content {
  text-align: left;
}

.hero .label {
  color: white;
  opacity: 0.8;
  margin-bottom: var(--space-md);
}

.hero h1 {
  color: white;
  margin-bottom: var(--space-lg);
}

.hero-subtitle {
  font-size: 1.125rem;
  color: white;
  opacity: 0.9;
  margin-bottom: var(--space-xl);
  max-width: 500px;
}

.hero-cta {
  display: flex;
  gap: var(--space-md);
  flex-wrap: wrap;
}

.hero .btn-primary {
  background: var(--navy);
  box-shadow: 4px 4px 0 var(--teal-dark);
}

.hero .btn-primary:hover {
  background: var(--text-dark);
}

.hero .btn-secondary {
  border-color: white;
  color: white;
}

.hero .btn-secondary:hover {
  background: white;
  color: var(--teal);
}

/* Values */
.values-section {
  background: var(--bg-white);
  border-bottom: 2px solid var(--border);
}

.values-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: var(--space-lg);
}

.value-card .stamp {
  margin-bottom: var(--space-md);
}

.value-card h3 {
  margin-bottom: var(--space-sm);
}

.value-card p {
  font-size: 0.875rem;
  color: var(--text-mid);
}

/* Events */
.events-section {
  background: var(--bg-light);
}

.events-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-lg);
}

.event-card .badge {
  margin-bottom: var(--space-md);
}

.event-card h3 {
  margin-bottom: var(--space-sm);
}

.event-card p {
  font-size: 0.875rem;
  color: var(--text-mid);
  margin-bottom: var(--space-sm);
}

.event-schedule {
  color: var(--teal);
  font-weight: 600;
  font-size: 0.875rem;
}

/* CTA */
.cta-section {
  background: var(--navy);
  color: white;
  text-align: center;
}

.cta-section h2 {
  color: white;
  margin-bottom: var(--space-md);
}

.cta-section > .container > p {
  color: var(--text-light);
  margin-bottom: var(--space-xl);
}

.cta-buttons {
  display: flex;
  gap: var(--space-md);
  justify-content: center;
  flex-wrap: wrap;
}

.cta-section .btn-secondary {
  border-color: white;
  color: white;
}

.cta-section .btn-secondary:hover {
  background: white;
  color: var(--navy);
}

@media (max-width: 900px) {
  .hero-inner {
    grid-template-columns: 1fr;
    text-align: center;
  }

  .hero-animation {
    order: -1;
    height: 280px;
  }

  .hero-content {
    text-align: center;
  }

  .hero-subtitle {
    margin-left: auto;
    margin-right: auto;
  }

  .hero-cta {
    justify-content: center;
  }
}

@media (max-width: 768px) {
  .hero-cta {
    flex-direction: column;
    align-items: center;
  }

  .values-grid {
    grid-template-columns: 1fr;
  }

  .events-grid {
    grid-template-columns: 1fr;
  }

  .cta-buttons {
    flex-direction: column;
    align-items: center;
  }
}
</style>
