<template>
  <!-- Hero Section -->
  <section class="hero">
    <div class="container">
      <div class="hero-inner">
        <div class="hero-content">
          <p class="label">Developer Relations Consulting</p>
          <h1>DevRel from the workshop floor.</h1>
          <p class="hero-subtitle">
            Not another agency with slide decks. Practitioners who've built communities from hackerspaces to enterprise scale.
          </p>
          <div class="hero-cta">
            <router-link to="/contact" class="btn btn-primary">Start Your Project</router-link>
            <a href="#services" class="btn btn-secondary">Our Services</a>
          </div>
        </div>
        <div class="hero-logo">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 380 130" width="380" height="130">
            <g transform="translate(10, 10)">
              <circle class="ring" cx="49.3" cy="44.5" r="35.3" fill="none" stroke="#4db3a8" stroke-width="3"/>
              <line class="connector c1" x1="49.29" y1="44.5" x2="49.15" y2="20.63" stroke="#4db3a8" stroke-width="2.5" stroke-linecap="round"/>
              <line class="connector c2" x1="49.29" y1="44.5" x2="60.69" y2="32.78" stroke="#4db3a8" stroke-width="2.5" stroke-linecap="round"/>
              <line class="connector c3" x1="49.29" y1="44.5" x2="71.84" y2="44.5" stroke="#4db3a8" stroke-width="2.5" stroke-linecap="round"/>
              <line class="connector c4" x1="49.29" y1="44.5" x2="60.69" y2="56.22" stroke="#4db3a8" stroke-width="2.5" stroke-linecap="round"/>
              <line class="connector c5" x1="49.29" y1="44.5" x2="49.15" y2="68.37" stroke="#4db3a8" stroke-width="2.5" stroke-linecap="round"/>
              <line class="connector c6" x1="49.29" y1="44.5" x2="37.31" y2="56.22" stroke="#4db3a8" stroke-width="2.5" stroke-linecap="round"/>
              <line class="connector c7" x1="49.29" y1="44.5" x2="26.16" y2="44.5" stroke="#4db3a8" stroke-width="2.5" stroke-linecap="round"/>
              <line class="connector c8" x1="49.29" y1="44.5" x2="37.31" y2="32.78" stroke="#4db3a8" stroke-width="2.5" stroke-linecap="round"/>
              <circle class="node n1" cx="49.15" cy="20.63" r="4.2" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.5"/>
              <circle class="node n3" cx="71.84" cy="44.5" r="4.2" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.5"/>
              <circle class="node n5" cx="49.15" cy="68.37" r="4.2" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.5"/>
              <circle class="node n7" cx="26.16" cy="44.5" r="4.2" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.5"/>
              <circle class="node n2" cx="60.69" cy="32.78" r="4.2" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.5"/>
              <circle class="node n4" cx="60.69" cy="56.22" r="4.2" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.5"/>
              <circle class="node n6" cx="37.31" cy="56.22" r="4.2" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.5"/>
              <circle class="node n8" cx="37.31" cy="32.78" r="4.2" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.5"/>
              <circle class="center-node" cx="49.29" cy="44.5" r="2.2" fill="#3d9991" stroke="#1e3a3a" stroke-width="1.5"/>
            </g>
            <text class="text-dev" x="125" y="58" font-family="'JetBrains Mono', monospace" font-size="46" font-weight="bold" fill="#ffffff">DEV</text>
            <text class="text-eco" x="205" y="58" font-family="'JetBrains Mono', monospace" font-size="46" font-weight="bold" fill="#4db3a8">ECO</text>
            <text class="text-consulting" x="127" y="82" font-family="'JetBrains Mono', monospace" font-size="15" letter-spacing="0.25em" fill="#ffffff">CONSULTING LLC</text>
          </svg>
        </div>
      </div>
    </div>
  </section>

  <!-- Services Section -->
  <section id="services">
    <div class="container">
      <div class="section-header">
        <h2>Our Services</h2>
      </div>

      <div class="services-grid">
        <div class="card service-card">
          <span class="stamp stamp-teal">Community</span>
          <h3>Community Building</h3>
          <p>Real communities, not vanity metrics.</p>
          <ul class="service-list">
            <li>Community strategy</li>
            <li>Platform setup</li>
            <li>Engagement programs</li>
          </ul>
        </div>

        <div class="card service-card">
          <span class="stamp stamp-pink">Content</span>
          <h3>Technical Content</h3>
          <p>Content from people who build.</p>
          <ul class="service-list">
            <li>Content strategy</li>
            <li>Technical docs</li>
            <li>Video tutorials</li>
          </ul>
        </div>

        <div class="card service-card">
          <span class="stamp stamp-navy">Adoption</span>
          <h3>Product Adoption</h3>
          <p>Get developers using your product.</p>
          <ul class="service-list">
            <li>Onboarding design</li>
            <li>Integration guides</li>
            <li>Hackathons</li>
          </ul>
        </div>

        <div class="card service-card">
          <span class="stamp stamp-teal">Training</span>
          <h3>Training & Workshops</h3>
          <p>Hands-on learning for teams.</p>
          <ul class="service-list">
            <li>Team training</li>
            <li>DevRel practices</li>
            <li>Strategic planning</li>
          </ul>
        </div>

        <div class="card service-card">
          <span class="stamp stamp-pink">Strategy</span>
          <h3>DevRel Strategy</h3>
          <p>Clear roadmaps. Real results.</p>
          <ul class="service-list">
            <li>Program design</li>
            <li>Metrics framework</li>
            <li>Team structure</li>
          </ul>
        </div>

        <div class="card service-card">
          <span class="stamp stamp-navy">Partnerships</span>
          <h3>Developer Partnerships</h3>
          <p>Build authentic advocacy.</p>
          <ul class="service-list">
            <li>Partner programs</li>
            <li>Ambassador networks</li>
            <li>Community campaigns</li>
          </ul>
        </div>
      </div>
    </div>
  </section>

  <!-- Process Section -->
  <section id="process" class="process-section">
    <div class="container">
      <div class="section-header">
        <h2>Our Process</h2>
        <p class="process-subtitle">From discovery to deployment. We're your embedded partner.</p>
      </div>

      <div class="process-grid">
        <div class="process-step" data-step="1">
          <h4>Discovery</h4>
          <p>We analyze your goals, audience, and current state to understand what you actually need.</p>
        </div>

        <div class="process-step" data-step="2">
          <h4>Strategy</h4>
          <p>Clear roadmaps using methods that work. No jargon. Just a plan you can execute.</p>
        </div>

        <div class="process-step" data-step="3">
          <h4>Implementation</h4>
          <p>We build alongside you. Communities, content, programs. Hands-on work.</p>
        </div>

        <div class="process-step" data-step="4">
          <h4>Growth</h4>
          <p>Monitor, optimize, scale. Systems that grow, not dependencies that require us forever.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- Team Section -->
  <section id="team" class="team-section">
    <div class="container">
      <div class="section-header">
        <h2>Meet Our Team</h2>
      </div>

      <div class="team-grid">
        <div class="team-member" :class="{ expanded: expanded.robert }">
          <div class="member-photo">
            <img src="/team/robert.jpg" alt="Robert Wolff" />
          </div>
          <h3>Robert Wolff</h3>
          <p class="member-title">Founder & Partner</p>
          <p class="member-bio-preview">Developer advocate and community strategist with a decade of experience building and scaling developer ecosystems.</p>
          <p class="member-bio">
            Robert is a developer advocate and community strategist with a decade of experience building and scaling developer ecosystems.
            As the founder of DevEco Consulting, he's helped companies connect with developers through authentic storytelling, content, and hands-on engagement.
            Previously at Arm, he led developer programs, community growth, and global advocacy initiatives.
            Robert is passionate about open source, collaboration, and helping technology teams connect with the people who build the future.
          </p>
          <button class="read-more-btn" @click="expanded.robert = !expanded.robert">
            {{ expanded.robert ? 'Show less' : 'Read more' }}
          </button>
          <div class="member-tags">
            <span class="tag">Developer Advocacy</span>
            <span class="tag">Community Strategy</span>
            <span class="tag">Open Source</span>
          </div>
        </div>

        <div class="team-member" :class="{ expanded: expanded.moheeb }">
          <div class="member-photo">
            <img src="/team/moheeb.jpg" alt="Moheeb Zara" />
          </div>
          <h3>Moheeb Zara</h3>
          <p class="member-title">Partner</p>
          <p class="member-bio-preview">Developer relations engineer, creative technologist, and founder of LumenCanvas, a browser-native projection mapping platform.</p>
          <p class="member-bio">
            Moheeb is a developer relations engineer, creative technologist, and founder of LumenCanvas, a browser-native projection mapping platform.
            With over a decade of experience spanning art, robotics, and open-source software, he's worked with companies like AWS, SignalWire, and SORACOM
            to bring complex technologies to life through content, demos, and community.
            As a partner at DevEco Consulting, Moheeb leads development on creative tools and developer-focused projects that blend art and engineering.
          </p>
          <button class="read-more-btn" @click="expanded.moheeb = !expanded.moheeb">
            {{ expanded.moheeb ? 'Show less' : 'Read more' }}
          </button>
          <div class="member-tags">
            <span class="tag">Creative Technology</span>
            <span class="tag">Developer Relations</span>
            <span class="tag">Open Source</span>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- CTA Section -->
  <section class="cta-section">
    <div class="container">
      <h2>Ready to build something real?</h2>
      <p>No pitch decks. Just a conversation about what you need.</p>
      <div class="cta-buttons">
        <router-link to="/contact" class="btn btn-primary">Start a Conversation</router-link>
        <router-link to="/community" class="btn btn-secondary">Join Our Community</router-link>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { reactive } from 'vue'

const expanded = reactive({
  robert: false,
  moheeb: false
})
</script>

<style>
/* Animation keyframes for SVG logo */
@keyframes hub-emerge {
  0% { transform: scale(0); opacity: 0; }
  60% { transform: scale(1.2); opacity: 1; }
  100% { transform: scale(1); opacity: 1; }
}
@keyframes node-join {
  0% { transform: scale(0) translateY(10px); opacity: 0; }
  70% { transform: scale(1.15) translateY(0); opacity: 1; }
  100% { transform: scale(1) translateY(0); opacity: 1; }
}
@keyframes connection-form {
  0% { stroke-dashoffset: 30; opacity: 0; }
  30% { opacity: 1; }
  100% { stroke-dashoffset: 0; opacity: 1; }
}
@keyframes ring-complete {
  0% { stroke-dashoffset: 222; opacity: 0.3; }
  100% { stroke-dashoffset: 0; opacity: 1; }
}
@keyframes text-arrive {
  0% { opacity: 0; transform: translateX(-8px); }
  100% { opacity: 1; transform: translateX(0); }
}
@keyframes consulting-settle {
  0% { opacity: 0; letter-spacing: 0.5em; }
  100% { opacity: 1; letter-spacing: 0.3em; }
}

.center-node { animation: hub-emerge 0.5s cubic-bezier(0.34, 1.56, 0.64, 1) 0.2s both; }
.connector { stroke-dasharray: 30; stroke-dashoffset: 30; }
.c1 { animation: connection-form 0.4s ease-out 0.5s both; }
.c2 { animation: connection-form 0.4s ease-out 0.65s both; }
.c3 { animation: connection-form 0.4s ease-out 0.8s both; }
.c4 { animation: connection-form 0.4s ease-out 0.95s both; }
.c5 { animation: connection-form 0.4s ease-out 1.1s both; }
.c6 { animation: connection-form 0.4s ease-out 1.25s both; }
.c7 { animation: connection-form 0.4s ease-out 1.4s both; }
.c8 { animation: connection-form 0.4s ease-out 1.55s both; }
.n1 { animation: node-join 0.4s cubic-bezier(0.34, 1.56, 0.64, 1) 0.6s both; }
.n2 { animation: node-join 0.4s cubic-bezier(0.34, 1.56, 0.64, 1) 0.75s both; }
.n3 { animation: node-join 0.4s cubic-bezier(0.34, 1.56, 0.64, 1) 0.9s both; }
.n4 { animation: node-join 0.4s cubic-bezier(0.34, 1.56, 0.64, 1) 1.05s both; }
.n5 { animation: node-join 0.4s cubic-bezier(0.34, 1.56, 0.64, 1) 1.2s both; }
.n6 { animation: node-join 0.4s cubic-bezier(0.34, 1.56, 0.64, 1) 1.35s both; }
.n7 { animation: node-join 0.4s cubic-bezier(0.34, 1.56, 0.64, 1) 1.5s both; }
.n8 { animation: node-join 0.4s cubic-bezier(0.34, 1.56, 0.64, 1) 1.65s both; }
.ring { stroke-dasharray: 222; animation: ring-complete 0.8s ease-out 1.8s both; }
.text-dev { animation: text-arrive 0.5s ease-out 2.0s both; }
.text-eco { animation: text-arrive 0.5s ease-out 2.1s both; }
.text-consulting { animation: consulting-settle 0.6s ease-out 2.2s both; }
</style>

<style scoped>
.hero {
  background: var(--navy);
  color: white;
  padding: var(--space-2xl) 0;
}

.hero-inner {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: var(--space-xl);
  align-items: center;
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
  margin-bottom: var(--space-xl);
  max-width: 500px;
}

.hero-cta {
  display: flex;
  gap: var(--space-md);
  flex-wrap: wrap;
}

.hero .btn-secondary {
  border-color: white;
  color: white;
}

.hero .btn-secondary:hover {
  background: white;
  color: var(--navy);
}

.hero-logo {
  display: flex;
  align-items: center;
  justify-content: center;
}

.hero-logo svg {
  max-width: 100%;
  height: auto;
}

/* Services */
.services-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: var(--space-lg);
}

.service-card .stamp {
  margin-bottom: var(--space-md);
}

.service-card h3 {
  margin-bottom: var(--space-sm);
}

.service-card > p {
  font-size: 0.875rem;
  color: var(--text-mid);
  margin-bottom: var(--space-md);
}

.service-list {
  list-style: none;
}

.service-list li {
  padding: var(--space-sm) 0;
  border-bottom: 1px solid var(--border);
  font-size: 0.875rem;
  color: var(--text-mid);
  position: relative;
  padding-left: 1.25rem;
}

.service-list li::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  width: 6px;
  height: 6px;
  background: var(--teal);
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
}

/* Process */
.process-section {
  background: var(--bg-white);
  border-top: 2px solid var(--border);
  border-bottom: 2px solid var(--border);
}

.process-subtitle {
  color: var(--text-mid);
  margin-top: var(--space-sm);
}

.process-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: var(--space-lg);
}

.process-step {
  position: relative;
  padding-left: var(--space-xl);
}

.process-step::before {
  content: attr(data-step);
  position: absolute;
  left: 0;
  top: 0;
  width: 36px;
  height: 36px;
  background: var(--teal);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
}

.process-step h4 {
  margin-bottom: var(--space-sm);
}

.process-step p {
  font-size: 0.875rem;
  color: var(--text-mid);
}

/* Team */
.team-section {
  background: linear-gradient(180deg, var(--bg-light) 0%, var(--bg-white) 100%);
}

.team-grid {
  display: flex;
  flex-direction: column;
  gap: var(--space-xl);
}

.team-member {
  background: white;
  border: 2px solid var(--border);
  padding: var(--space-xl);
}

.member-photo {
  width: 100px;
  height: 100px;
  margin-bottom: var(--space-md);
  background: var(--navy);
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
  overflow: hidden;
}

.member-photo img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.team-member h3 {
  margin-bottom: var(--space-xs);
}

.member-title {
  color: var(--teal);
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  margin-bottom: var(--space-md);
}

.member-bio-preview {
  display: none;
  font-size: 0.875rem;
  color: var(--text-mid);
  line-height: 1.7;
  margin-bottom: var(--space-sm);
}

.member-bio {
  font-size: 0.875rem;
  color: var(--text-mid);
  line-height: 1.7;
  margin-bottom: var(--space-md);
}

.read-more-btn {
  display: none;
  background: none;
  border: none;
  color: var(--teal);
  font-size: 0.875rem;
  font-weight: 600;
  cursor: pointer;
  padding: 0;
  margin-bottom: var(--space-md);
}

.read-more-btn:hover {
  text-decoration: underline;
}

.member-tags {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-sm);
}

.tag {
  background: var(--bg-light);
  color: var(--teal);
  padding: 0.25rem 0.5rem;
  font-size: 0.75rem;
  font-weight: 500;
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

  .hero-logo {
    order: -1;
    margin-bottom: var(--space-lg);
  }

  .hero-logo svg {
    width: 280px;
    height: auto;
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

  .services-grid {
    grid-template-columns: 1fr;
  }

  .process-grid {
    grid-template-columns: 1fr;
  }

  .team-grid {
    grid-template-columns: 1fr;
  }

  .member-bio-preview {
    display: block;
  }

  .member-bio {
    display: none;
  }

  .read-more-btn {
    display: block;
  }

  .team-member.expanded .member-bio-preview {
    display: none;
  }

  .team-member.expanded .member-bio {
    display: block;
  }

  .cta-buttons {
    flex-direction: column;
    align-items: center;
  }
}
</style>
