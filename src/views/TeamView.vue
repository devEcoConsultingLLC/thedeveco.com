<script setup lang="ts">
import { ref } from 'vue'

interface EngineNode {
  id: string
  name: string
  role: string
  description: string
}

const activeNode = ref<string | null>(null)

const engineNodes: EngineNode[] = [
  { id: 'community', name: 'devEco Community', role: 'network', description: 'The living network. Discord servers, local meetups, Developers Anonymous, and the open source communities where builders connect and collaborate.' },
  { id: 'consulting', name: 'devEco Consulting LLC', role: 'strategy', description: 'Strategy and execution. Developer ecosystem consulting, community operations, and technical content — from Edge AI to embedded systems.' },
  { id: 'content', name: 'devEco Content Hub', role: 'publishing', description: 'The publishing engine. Blog posts, LinkedIn campaigns, certification courses, and the 52-principle Community Operating System knowledge base.' },
  { id: 'crm', name: 'devEco CRM', role: 'relationships', description: 'The relationship layer. Contact management, engagement tracking, and pipeline operations across the entire ecosystem.' },
  { id: 'estate', name: 'devEco Digital Estate', role: 'namespaces', description: 'Every namespace. Every platform. The full devEco presence — domains, repositories, social channels, and distribution endpoints across the web.' },
]

const toggleNode = (nodeId: string) => {
  activeNode.value = activeNode.value === nodeId ? null : nodeId
}

const activeNodeData = () => engineNodes.find(n => n.id === activeNode.value)
</script>

<template>
  <!-- Hero Section -->
  <section class="hero">
    <div class="container">
      <div class="hero-inner">
        <div class="hero-animation">
          <svg viewBox="0 0 400 400" xmlns="http://www.w3.org/2000/svg" class="rocket-portal-svg">
            <defs>
              <radialGradient id="portalGrad" cx="50%" cy="50%" r="50%">
                <stop offset="0%" stop-color="#0ff" stop-opacity="0"/>
                <stop offset="60%" stop-color="#0ff" stop-opacity="0.3"/>
                <stop offset="100%" stop-color="#8b5cf6" stop-opacity="0.8"/>
              </radialGradient>
            </defs>

            <!-- Portal -->
            <g class="portal">
              <ellipse cx="200" cy="200" rx="50" ry="50" fill="url(#portalGrad)"/>
              <g class="portal-ring">
                <ellipse cx="200" cy="200" rx="55" ry="55" fill="none" stroke="#0ff" stroke-width="3" stroke-dasharray="8 12"/>
              </g>
              <ellipse cx="200" cy="200" rx="35" ry="35" fill="none" stroke="#a78bfa" stroke-width="2" stroke-dasharray="4 8"/>
            </g>

            <!-- Rocket -->
            <g class="rocket">
              <g class="flame">
                <polygon points="0,25 -8,42 0,36 8,42" fill="#ff6b35"/>
                <polygon points="0,27 -5,38 0,33 5,38" fill="#ffd166"/>
              </g>
              <ellipse cx="0" cy="0" rx="12" ry="25" fill="#8b5cf6"/>
              <polygon points="0,-38 -10,-18 10,-18" fill="#a78bfa"/>
              <circle cx="0" cy="-5" r="6" fill="#0ff"/>
              <circle cx="0" cy="-5" r="3.5" fill="#1e3a3a"/>
              <polygon points="-12,12 -22,28 -12,22" fill="#ec4899"/>
              <polygon points="12,12 22,28 12,22" fill="#ec4899"/>
            </g>

            <!-- Gears -->
            <g class="gears-group">
              <!-- Main gear - Coral -->
              <g class="gear1">
                <circle cx="200" cy="200" r="45" fill="#ff6b6b"/>
                <circle cx="200" cy="200" r="15" fill="#1e3a3a"/>
                <g fill="#ff6b6b">
                  <rect x="192" y="148" width="16" height="14" rx="2"/>
                  <rect x="192" y="238" width="16" height="14" rx="2"/>
                  <rect x="238" y="192" width="14" height="16" rx="2"/>
                  <rect x="148" y="192" width="14" height="16" rx="2"/>
                  <rect x="226" y="160" width="14" height="14" rx="2" transform="rotate(45, 233, 167)"/>
                  <rect x="160" y="226" width="14" height="14" rx="2" transform="rotate(45, 167, 233)"/>
                  <rect x="226" y="226" width="14" height="14" rx="2" transform="rotate(45, 233, 233)"/>
                  <rect x="160" y="160" width="14" height="14" rx="2" transform="rotate(45, 167, 167)"/>
                </g>
              </g>

              <!-- Top right gear - Yellow -->
              <g class="gear2">
                <circle cx="295" cy="155" r="32" fill="#ffd166"/>
                <circle cx="295" cy="155" r="10" fill="#1e3a3a"/>
                <g fill="#ffd166">
                  <rect x="288" y="118" width="14" height="12" rx="2"/>
                  <rect x="288" y="180" width="14" height="12" rx="2"/>
                  <rect x="320" y="148" width="12" height="14" rx="2"/>
                  <rect x="258" y="148" width="12" height="14" rx="2"/>
                  <rect x="314" y="128" width="12" height="12" rx="2" transform="rotate(45, 320, 134)"/>
                  <rect x="264" y="170" width="12" height="12" rx="2" transform="rotate(45, 270, 176)"/>
                </g>
              </g>

              <!-- Bottom left gear - Teal -->
              <g class="gear3">
                <circle cx="120" cy="260" r="38" fill="#4db3a8"/>
                <circle cx="120" cy="260" r="12" fill="#1e3a3a"/>
                <g fill="#4db3a8">
                  <rect x="112" y="216" width="16" height="13" rx="2"/>
                  <rect x="112" y="291" width="16" height="13" rx="2"/>
                  <rect x="151" y="252" width="13" height="16" rx="2"/>
                  <rect x="76" y="252" width="13" height="16" rx="2"/>
                  <rect x="144" y="226" width="13" height="13" rx="2" transform="rotate(45, 150, 232)"/>
                  <rect x="83" y="281" width="13" height="13" rx="2" transform="rotate(45, 90, 288)"/>
                </g>
              </g>

              <!-- Bottom right gear - Pink -->
              <g class="gear4">
                <circle cx="280" cy="290" r="28" fill="#ec4899"/>
                <circle cx="280" cy="290" r="9" fill="#1e3a3a"/>
                <g fill="#ec4899">
                  <rect x="273" y="258" width="14" height="10" rx="2"/>
                  <rect x="273" y="312" width="14" height="10" rx="2"/>
                  <rect x="302" y="283" width="10" height="14" rx="2"/>
                  <rect x="248" y="283" width="10" height="14" rx="2"/>
                </g>
              </g>

              <!-- Top left gear - Purple -->
              <g class="gear5">
                <circle cx="100" cy="140" r="25" fill="#a78bfa"/>
                <circle cx="100" cy="140" r="8" fill="#1e3a3a"/>
                <g fill="#a78bfa">
                  <rect x="93" y="111" width="14" height="10" rx="2"/>
                  <rect x="93" y="159" width="14" height="10" rx="2"/>
                  <rect x="119" y="133" width="10" height="14" rx="2"/>
                  <rect x="71" y="133" width="10" height="14" rx="2"/>
                </g>
              </g>
            </g>
          </svg>
        </div>
        <div class="hero-content">
          <p class="label">About Us</p>
          <h1>The people behind devEco.</h1>
          <p class="hero-subtitle">
            Practitioners who've been building developer communities since before it was a job title.
          </p>
        </div>
      </div>
    </div>
  </section>

  <!-- Team Section -->
  <section id="team" class="team-section">
    <div class="container">
      <div class="section-header">
        <h2>The Collective</h2>
      </div>

      <div class="team-grid">
        <div class="team-member">
          <span class="badge badge-teal">Founder</span>
          <div class="team-photo">
            <img src="/team/robert.jpg" alt="Robert Wolff" />
          </div>
          <h3>Robert Wolff</h3>
          <p class="role">Founder & Partner</p>
          <p class="bio">Entrepreneur, developer advocate, community strategist, and systems orchestrator. Founder of the devEco Community Engine.</p>
          <a href="https://linktr.ee/robertwolff" target="_blank" class="member-link">Linktree</a>
        </div>

        <div class="team-member">
          <span class="badge badge-pink">Partner</span>
          <div class="team-photo">
            <img src="/team/moheeb.jpg" alt="Moheeb Zara" />
          </div>
          <h3>Moheeb Zara</h3>
          <p class="role">Partner</p>
          <p class="bio">Creative technologist. Hardware hacker. HeatSync Labs alum. Founder of LumenCanvas.</p>
          <a href="https://hack.build" target="_blank" class="member-link">Portfolio</a>
        </div>
      </div>
    </div>
  </section>

  <!-- Community Engine Explorer -->
  <section class="engine-section">
    <div class="container">
      <div class="engine-header">
        <h2>The devEco Community Engine</h2>
        <p>One ecosystem. Many surfaces. Five connected layers driving developer community at scale.</p>
      </div>

      <div class="engine-grid">
        <button
          class="engine-node"
          :class="{ active: activeNode === 'community' }"
          @click="toggleNode('community')"
        >
          <span class="engine-node__pulse" style="animation-delay: 0s"></span>
          <span class="engine-node__name">devEco Community</span>
          <span class="engine-node__role">network</span>
        </button>

        <div class="engine-hub">
          <span class="engine-hub__name">ENGINE</span>
          <span class="engine-hub__sub">devEco</span>
        </div>

        <button
          class="engine-node"
          :class="{ active: activeNode === 'consulting' }"
          @click="toggleNode('consulting')"
        >
          <span class="engine-node__pulse" style="animation-delay: 0.4s"></span>
          <span class="engine-node__name">devEco Consulting LLC</span>
          <span class="engine-node__role">strategy</span>
        </button>

        <button
          class="engine-node"
          :class="{ active: activeNode === 'content' }"
          @click="toggleNode('content')"
        >
          <span class="engine-node__pulse" style="animation-delay: 0.8s"></span>
          <span class="engine-node__name">devEco Content Hub</span>
          <span class="engine-node__role">publishing</span>
        </button>

        <button
          class="engine-node"
          :class="{ active: activeNode === 'crm' }"
          @click="toggleNode('crm')"
        >
          <span class="engine-node__pulse" style="animation-delay: 1.2s"></span>
          <span class="engine-node__name">devEco CRM</span>
          <span class="engine-node__role">relationships</span>
        </button>

        <button
          class="engine-node"
          :class="{ active: activeNode === 'estate' }"
          @click="toggleNode('estate')"
        >
          <span class="engine-node__pulse" style="animation-delay: 1.6s"></span>
          <span class="engine-node__name">devEco Digital Estate</span>
          <span class="engine-node__role">namespaces</span>
        </button>
      </div>

      <div class="engine-detail" :class="{ open: activeNode !== null }">
        <template v-if="activeNodeData()">
          <h4 class="engine-detail__name">{{ activeNodeData()!.name }}</h4>
          <p class="engine-detail__desc">{{ activeNodeData()!.description }}</p>
        </template>
      </div>

      <div class="engine-cta">
        <router-link to="/contact" class="btn btn-primary">Start a Conversation</router-link>
        <a href="https://discord.gg/deveco" target="_blank" class="btn btn-secondary">Join Our Community</a>
      </div>
    </div>
  </section>
</template>

<style>
/* SVG Animation styles (unscoped) */
.rocket-portal-svg .rocket {
  animation: rocketMove 6s ease-in-out infinite;
}

@keyframes rocketMove {
  0% { transform: translate(40px, 360px) rotate(45deg) scale(0.8); opacity: 0; }
  10% { transform: translate(60px, 340px) rotate(45deg) scale(1); opacity: 1; }
  40% { transform: translate(185px, 215px) rotate(45deg) scale(1); opacity: 1; }
  50% { transform: translate(200px, 200px) rotate(45deg) scale(0); opacity: 0; }
  100% { transform: translate(200px, 200px) rotate(45deg) scale(0); opacity: 0; }
}

.rocket-portal-svg .portal {
  transform-origin: 200px 200px;
  animation: portalPulse 6s ease-in-out infinite;
}

@keyframes portalPulse {
  0% { transform: scale(0.8); opacity: 0.6; }
  30% { transform: scale(1); opacity: 1; }
  45% { transform: scale(1.3); opacity: 1; }
  50% { transform: scale(3); opacity: 0; }
  100% { transform: scale(0.8); opacity: 0.6; }
}

.rocket-portal-svg .portal-ring {
  transform-origin: 200px 200px;
  animation: ringRotate 3s linear infinite;
}

@keyframes ringRotate {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.rocket-portal-svg .gears-group {
  animation: gearsAppear 6s ease-in-out infinite;
}

@keyframes gearsAppear {
  0%, 45% { opacity: 0; }
  55% { opacity: 1; }
  90% { opacity: 1; }
  100% { opacity: 0; }
}

.rocket-portal-svg .gear1 {
  transform-origin: 200px 200px;
  animation: spin1 4s linear infinite;
}

@keyframes spin1 {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.rocket-portal-svg .gear2 {
  transform-origin: 295px 155px;
  animation: spin2 3s linear infinite;
}

@keyframes spin2 {
  from { transform: rotate(0deg); }
  to { transform: rotate(-360deg); }
}

.rocket-portal-svg .gear3 {
  transform-origin: 120px 260px;
  animation: spin3 5s linear infinite;
}

@keyframes spin3 {
  from { transform: rotate(0deg); }
  to { transform: rotate(-360deg); }
}

.rocket-portal-svg .gear4 {
  transform-origin: 280px 290px;
  animation: spin4 2.5s linear infinite;
}

@keyframes spin4 {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.rocket-portal-svg .gear5 {
  transform-origin: 100px 140px;
  animation: spin5 3.5s linear infinite;
}

@keyframes spin5 {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.rocket-portal-svg .flame {
  transform-origin: 0 25px;
  animation: flicker 0.1s ease-in-out infinite alternate;
}

@keyframes flicker {
  from { opacity: 0.8; transform: scaleY(0.9); }
  to { opacity: 1; transform: scaleY(1.1); }
}
</style>

<style scoped>
.hero {
  background: var(--navy);
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
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.rocket-portal-svg {
  width: 100%;
  max-width: 320px;
  height: auto;
  overflow: hidden;
  border-radius: 50%;
  background: radial-gradient(circle, transparent 60%, var(--navy) 100%);
}

.hero-content {
  text-align: left;
}

.hero .label {
  color: var(--teal-light);
  margin-bottom: var(--space-md);
}

.hero h1 {
  color: white;
  margin-bottom: var(--space-lg);
}

.hero-subtitle {
  font-size: 1.125rem;
  color: var(--text-light);
  max-width: 500px;
}

/* Team */
.team-section {
  background: linear-gradient(180deg, var(--bg-light) 0%, var(--bg-white) 100%);
}

.team-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-xl);
}

.team-member {
  text-align: center;
}

.team-member > .badge {
  display: inline-block;
  margin-bottom: var(--space-sm);
  transform: rotate(-3deg);
}

.team-photo {
  width: 120px;
  height: 135px;
  margin: 0 auto var(--space-md);
  background: var(--navy);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 3rem;
  color: var(--teal);
  position: relative;
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
  overflow: hidden;
}

.team-photo img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.team-member h3 {
  margin-bottom: var(--space-xs);
}

.team-member .role {
  color: var(--teal);
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  margin-bottom: var(--space-sm);
}

.team-member .bio {
  font-size: 0.875rem;
  color: var(--text-mid);
  margin-bottom: var(--space-sm);
}

.member-link {
  display: inline-block;
  font-size: 0.75rem;
  color: var(--teal);
  text-decoration: none;
  border: 1px solid var(--teal);
  padding: 0.25rem 0.75rem;
  transition: all 0.2s;
}

.member-link:hover {
  background: var(--teal);
  color: white;
}

/* Engine Section */
.engine-section {
  background: #1c3535;
  padding: var(--space-2xl) 0;
}

.engine-header {
  text-align: center;
  margin-bottom: var(--space-xl);
}

.engine-header h2 {
  color: #f0ede4;
  margin-bottom: var(--space-md);
  position: relative;
  display: inline-block;
}

.engine-header h2::after {
  content: '';
  position: absolute;
  bottom: -8px;
  left: 0;
  right: 0;
  height: 3px;
  background: #4db3a8;
}

.engine-header p {
  color: rgba(240, 237, 228, 0.5);
  font-size: 0.875rem;
  margin-top: var(--space-md);
}

.engine-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}

.engine-hub {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 1.25rem;
  border: 1.5px solid #d4a843;
  background: rgba(212, 168, 67, 0.08);
  text-align: center;
}

.engine-hub__name {
  color: #d4a843;
  font-size: 0.875rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.2em;
}

.engine-hub__sub {
  color: #d4a843;
  font-size: 0.7rem;
  margin-top: 0.25rem;
  opacity: 0.7;
}

.engine-node {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 1.25rem;
  border: 1.5px dashed rgba(240, 237, 228, 0.15);
  background: transparent;
  cursor: pointer;
  text-align: center;
  font-family: inherit;
  transition: border-color 0.2s, background 0.2s;
}

.engine-node:hover {
  border: 1.5px dashed #4db3a8;
  background: rgba(77, 179, 168, 0.05);
}

.engine-node.active {
  border: 1.5px solid #4db3a8;
  background: rgba(77, 179, 168, 0.08);
}

.engine-node__name {
  color: #f0ede4;
  font-size: 0.8rem;
  font-weight: 600;
}

.engine-node__role {
  color: #4db3a8;
  font-size: 0.65rem;
  text-transform: uppercase;
  letter-spacing: 0.15em;
  margin-top: 0.35rem;
}

.engine-node__pulse {
  position: absolute;
  top: 8px;
  right: 8px;
  width: 6px;
  height: 6px;
  background: #4db3a8;
  animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 0.3; }
  50% { opacity: 1; }
}

.engine-detail {
  max-height: 0;
  overflow: hidden;
  opacity: 0;
  transition: max-height 0.35s ease, opacity 0.25s ease, margin 0.35s ease;
  margin-top: 0;
  background: rgba(77, 179, 168, 0.04);
  border-left: 3px solid #4db3a8;
  padding: 0 1.5rem;
}

.engine-detail.open {
  max-height: 200px;
  opacity: 1;
  margin-top: 1.5rem;
  padding: 1.25rem 1.5rem;
}

.engine-detail__name {
  color: #f0ede4;
  font-size: 0.8rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.engine-detail__desc {
  color: rgba(240, 237, 228, 0.7);
  font-size: 0.875rem;
  line-height: 1.6;
}

.engine-cta {
  display: flex;
  justify-content: center;
  gap: 1rem;
  flex-wrap: wrap;
  margin-top: 2.5rem;
}

.engine-cta .btn-secondary {
  border-color: rgba(240, 237, 228, 0.3);
  color: rgba(240, 237, 228, 0.6);
}

.engine-cta .btn-secondary:hover {
  border-color: #4db3a8;
  color: #4db3a8;
}

@media (max-width: 900px) {
  .hero-inner {
    grid-template-columns: 1fr;
    text-align: center;
  }

  .hero-animation {
    order: -1;
  }

  .rocket-portal-svg {
    max-width: 280px;
  }

  .hero-content {
    text-align: center;
  }

  .hero-subtitle {
    margin-left: auto;
    margin-right: auto;
  }

  .engine-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .team-grid {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 600px) {
  .engine-grid {
    grid-template-columns: 1fr;
  }

  .engine-cta {
    flex-direction: column;
    align-items: center;
  }
}
</style>
