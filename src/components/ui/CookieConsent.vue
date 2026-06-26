<script setup lang="ts">
import { ref, onMounted } from 'vue'

const STORAGE_KEY = 'deveco-cookie-consent'
const visible = ref(false)

onMounted(() => {
  try {
    if (localStorage.getItem(STORAGE_KEY) !== 'accepted') visible.value = true
  } catch {
    visible.value = true
  }
})

function accept() {
  try {
    localStorage.setItem(STORAGE_KEY, 'accepted')
  } catch {
    // storage unavailable; dismiss for this session only
  }
  visible.value = false
}
</script>

<template>
  <Transition name="cookie-fade">
    <div v-if="visible" class="cookie-banner" role="region" aria-label="Cookie notice">
      <div class="cookie-inner">
        <p class="cookie-message">
          This site uses only essential cookies needed to function. It runs no advertising,
          analytics, or tracking. Read our
          <router-link to="/privacy" class="cookie-link">Privacy and Cookies</router-link>
          statement for details.
        </p>
        <button class="btn btn-primary cookie-button" @click="accept">Got it</button>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
.cookie-banner {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  background: var(--bg-white);
  border-top: 2px solid var(--navy);
}

.cookie-inner {
  max-width: 1200px;
  margin: 0 auto;
  padding: var(--space-md) var(--space-lg);
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-lg);
}

.cookie-message {
  font-size: 0.8125rem;
  color: var(--text-dark);
  line-height: 1.5;
  margin: 0;
}

.cookie-link {
  color: var(--teal);
  text-decoration: underline;
}

.cookie-button {
  flex-shrink: 0;
}

.cookie-fade-enter-active,
.cookie-fade-leave-active {
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.cookie-fade-enter-from,
.cookie-fade-leave-to {
  transform: translateY(100%);
  opacity: 0;
}

@media (max-width: 768px) {
  .cookie-inner {
    flex-direction: column;
    align-items: flex-start;
    gap: var(--space-md);
  }

  .cookie-button {
    width: 100%;
    text-align: center;
    justify-content: center;
  }
}
</style>
