<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-grid">
        <div class="footer-brand">
          <img src="/images/deveco-community-logo.png" alt="DevEco" class="footer-logo" />
          <p>DevRel from the workshop floor.</p>
        </div>

        <div class="footer-col">
          <h4>About</h4>
          <ul>
            <li><router-link to="/team">Team</router-link></li>
            <li><router-link to="/consultancy">Consultancy</router-link></li>
            <li><router-link to="/ecosystem">Ecosystem</router-link></li>
          </ul>
        </div>

        <div class="footer-col">
          <h4>Audits</h4>
          <ul>
            <li><a href="/devxrl" target="_blank" rel="noopener noreferrer">DevXRL Audit</a></li>
            <li><a href="/smrl" target="_blank" rel="noopener noreferrer">SMRL Audit</a></li>
            <li><a href="/g2mrl" target="_blank" rel="noopener noreferrer">G2MRL Audit</a></li>
            <li><a href="/trl" target="_blank" rel="noopener noreferrer">TRL Audit</a></li>
          </ul>
        </div>

        <div class="footer-col">
          <h4>Community</h4>
          <ul>
            <li><a href="https://discord.gg/deveco" target="_blank">Discord</a></li>
            <li><a href="https://github.com/devEcoConsultingLLC" target="_blank" rel="noopener noreferrer">GitHub</a></li>
            <li><a href="https://www.linkedin.com/company/developerecosystem/" target="_blank">LinkedIn</a></li>
            <li><a href="https://www.youtube.com/@thedeveco" target="_blank">YouTube</a></li>
            <li><a href="https://www.instagram.com/thedev.eco" target="_blank">Instagram</a></li>
            <li><a href="https://www.twitch.tv/thedeveco" target="_blank">Twitch</a></li>
          </ul>
        </div>

        <div class="footer-col">
          <h4>Explore</h4>
          <ul>
            <li><a href="https://deveco.io/" target="_blank">devEco.io</a></li>
            <li><a href="https://deveco.app/" target="_blank">devEco.app</a></li>
          </ul>
        </div>
      </div>

      <div class="footer-bottom">
        <span>{{ currentYear }} | devEco Consulting LLC | <router-link to="/privacy">Privacy</router-link></span>
        <span>EDGE AI FOUNDATION | <a href="https://www.edgeaifoundation.org/" target="_blank">Scholarship Partner</a></span>
      </div>
    </div>
  </footer>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const currentYear = computed(() => new Date().getFullYear())
</script>

<style scoped>
.footer {
  background: var(--text-dark);
  color: white;
  padding: var(--space-xl) 0 var(--space-lg);
}

.footer-grid {
  display: grid;
  grid-template-columns: 2fr repeat(4, 1fr);
  gap: var(--space-lg);
  margin-bottom: var(--space-xl);
}

.footer-logo {
  height: 45px;
  width: auto;
  margin-bottom: var(--space-md);
}

.footer-brand p {
  font-size: 0.875rem;
  color: var(--text-light);
}

.footer-col h4 {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  margin-bottom: var(--space-md);
  color: white;
}

.footer-col ul {
  list-style: none;
}

.footer-col li {
  margin-bottom: var(--space-sm);
}

.footer-col a {
  font-size: 0.875rem;
  color: var(--text-light);
  transition: color 0.2s;
}

.footer-col a:hover {
  color: var(--teal-light);
}

.footer-bottom {
  padding-top: var(--space-lg);
  border-top: 1px solid var(--text-mid);
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: var(--space-md);
  font-size: 0.75rem;
  color: var(--text-light);
}

.footer-bottom a {
  color: var(--teal-light);
}

@media (max-width: 768px) {
  .footer-grid {
    grid-template-columns: 1fr 1fr;
    gap: var(--space-lg);
  }

  .footer-brand {
    grid-column: 1 / -1;
  }

  .footer-bottom {
    flex-direction: column;
    text-align: center;
  }
}

@media (max-width: 480px) {
  .footer-grid {
    grid-template-columns: 1fr;
  }
}
</style>
