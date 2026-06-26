<script setup lang="ts">
import { RouterView } from 'vue-router'
import HeaderNav from './components/layout/HeaderNav.vue'
import FooterNav from './components/layout/FooterNav.vue'
import CookieConsent from './components/ui/CookieConsent.vue'
</script>

<template>
  <div id="app">
    <HeaderNav />
    <main class="main-content">
      <RouterView />
    </main>
    <FooterNav />
    <CookieConsent />
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;700&display=swap');

:root {
  /* DevEco Brand Colors */
  --teal-dark: #2a6b6b;
  --teal: #3d9991;
  --teal-light: #4db3a8;
  --navy: #1e3a3a;
  --bg-light: #eef3f6;
  --bg-white: #f8fafb;

  /* Accent Colors */
  --accent-pink: #e84a7f;
  --accent-yellow: #f7c948;
  --accent-cyan: #4ecdc4;
  --accent-blue: #5bc0eb;

  /* Neutrals */
  --text-dark: #1a2e2e;
  --text-mid: #4a5e5e;
  --text-light: #7a8e8e;
  --border: #c5d5d5;

  /* Typography */
  --font-mono: 'JetBrains Mono', monospace;

  /* Spacing */
  --space-xs: 0.25rem;
  --space-sm: 0.5rem;
  --space-md: 1rem;
  --space-lg: 2rem;
  --space-xl: 4rem;
  --space-2xl: 6rem;
}

*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: var(--font-mono);
  background-color: var(--bg-light);
  color: var(--text-dark);
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

::selection {
  background: var(--teal);
  color: white;
}

a {
  color: inherit;
  text-decoration: none;
}

img {
  max-width: 100%;
  display: block;
}

#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  width: 100%;
  overflow-x: hidden;
}

.main-content {
  flex: 1;
  width: 100%;
  overflow-x: hidden;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 var(--space-lg);
}

/* Typography */
h1, h2, h3, h4 {
  font-weight: 700;
  line-height: 1.1;
  color: var(--navy);
}

h1 {
  font-size: clamp(2.5rem, 6vw, 4rem);
}

h2 {
  font-size: clamp(1.75rem, 4vw, 2.5rem);
}

h3 {
  font-size: 1.25rem;
}

.label {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.2em;
  color: var(--teal);
}

.highlight {
  background: var(--accent-yellow);
  padding: 0 0.25rem;
  color: var(--navy);
}

/* Buttons */
.btn {
  display: inline-flex;
  align-items: center;
  gap: var(--space-sm);
  padding: 0.875rem 1.5rem;
  font-family: var(--font-mono);
  font-size: 0.875rem;
  font-weight: 700;
  border: none;
  cursor: pointer;
  transition: all 0.2s;
}

.btn-primary {
  background: var(--teal);
  color: white;
  box-shadow: 4px 4px 0 var(--navy);
}

.btn-primary:hover {
  background: var(--teal-dark);
  transform: translate(-2px, -2px);
  box-shadow: 6px 6px 0 var(--navy);
}

.btn-secondary {
  background: transparent;
  color: var(--navy);
  border: 2px solid var(--navy);
}

.btn-secondary:hover {
  background: var(--navy);
  color: white;
}

/* Stamps & Badges */
.stamp {
  display: inline-block;
  padding: 0.25rem 0.75rem;
  font-size: 0.625rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  border: 2px dashed currentColor;
  transform: rotate(-2deg);
}

.stamp-teal { color: var(--teal); }
.stamp-pink { color: var(--accent-pink); }
.stamp-yellow { color: #b8941a; }
.stamp-navy { color: var(--navy); }

.badge {
  display: inline-block;
  padding: 0.25rem 0.5rem;
  font-size: 0.625rem;
  font-weight: 700;
  text-transform: uppercase;
}

.badge-teal { background: var(--teal); color: white; }
.badge-pink { background: var(--accent-pink); color: white; }
.badge-navy { background: var(--navy); color: white; }

/* Cards */
.card {
  background: white;
  border: 2px solid var(--border);
  padding: var(--space-lg);
  transition: all 0.2s;
}

.card:hover {
  border-color: var(--teal);
  box-shadow: 6px 6px 0 var(--teal);
}

/* Sections */
section {
  padding: var(--space-2xl) 0;
}

.section-header {
  margin-bottom: var(--space-xl);
}

.section-header h2 {
  position: relative;
  display: inline-block;
}

.section-header h2::after {
  content: '';
  position: absolute;
  bottom: -8px;
  left: 0;
  width: 60px;
  height: 4px;
  background: var(--teal);
}

/* Grid utilities */
.grid {
  display: grid;
  gap: var(--space-lg);
}

.grid-2 {
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}

.grid-3 {
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
}

@media (max-width: 768px) {
  .container {
    padding: 0 var(--space-md);
  }

  section {
    padding: var(--space-xl) 0;
  }

  .btn {
    padding: 1rem 1.5rem;
    font-size: 1rem;
    width: 100%;
    text-align: center;
    justify-content: center;
  }

  .grid-2, .grid-3 {
    grid-template-columns: 1fr;
  }
}
</style>
