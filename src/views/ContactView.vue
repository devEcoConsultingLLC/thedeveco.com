<template>
  <!-- Hero Section -->
  <section class="hero">
    <div class="container">
      <div class="hero-content">
        <p class="label">Contact</p>
        <h1>Let's talk.</h1>
        <p class="hero-subtitle">
          No pitch decks. No 30-slide frameworks. Just a conversation about what you're building.
        </p>
      </div>
    </div>
  </section>

  <!-- Contact Options -->
  <section class="contact-section">
    <div class="container">
      <div class="contact-grid">
        <div class="contact-option primary">
          <span class="stamp stamp-teal">Preferred</span>
          <h3>Email Us</h3>
          <p>The fastest way to reach our team.</p>
          <a href="mailto:hello@thedeveco.com" class="contact-link">hello@thedeveco.com</a>
        </div>

        <div class="contact-option">
          <span class="stamp stamp-pink">Community</span>
          <h3>Join Discord</h3>
          <p>Talk to the team and community.</p>
          <a href="https://discord.gg/deveco" target="_blank" class="contact-link">discord.gg/deveco</a>
        </div>

        <div class="contact-option">
          <span class="stamp stamp-navy">Network</span>
          <h3>LinkedIn</h3>
          <p>Connect with us professionally.</p>
          <a href="https://linkedin.com/company/thedeveco" target="_blank" class="contact-link">linkedin.com/company/thedeveco</a>
        </div>
      </div>
    </div>
  </section>

  <!-- Contact Form -->
  <section class="form-section">
    <div class="container">
      <div class="form-wrapper">
        <div class="form-header">
          <h2>Start a Project</h2>
          <p>Tell us what you're building. We'll respond within 24 hours.</p>
        </div>

        <form @submit.prevent="submitForm" class="contact-form">
          <div class="form-row">
            <div class="form-group">
              <label for="name">Name *</label>
              <input type="text" id="name" v-model="form.name" required placeholder="Your name" />
            </div>

            <div class="form-group">
              <label for="email">Email *</label>
              <input type="email" id="email" v-model="form.email" required placeholder="your@email.com" />
            </div>
          </div>

          <div class="form-row">
            <div class="form-group">
              <label for="company">Company</label>
              <input type="text" id="company" v-model="form.company" placeholder="Your company" />
            </div>

            <div class="form-group">
              <label for="service">How can we help? *</label>
              <select id="service" v-model="form.service" required>
                <option value="">Select a service</option>
                <option value="community">Community Building</option>
                <option value="content">Technical Content</option>
                <option value="devrel">DevRel Strategy</option>
                <option value="adoption">Product Adoption</option>
                <option value="training">Training & Workshops</option>
                <option value="portal">portalNetwork Integration</option>
                <option value="other">Other</option>
              </select>
            </div>
          </div>

          <div class="form-group full-width">
            <label for="message">Project Details *</label>
            <textarea
              id="message"
              v-model="form.message"
              required
              rows="6"
              placeholder="Tell us about your project, goals, and timeline."
            ></textarea>
          </div>

          <div class="form-actions">
            <button type="submit" class="btn btn-primary" :disabled="isSubmitting">
              {{ isSubmitting ? 'Sending...' : 'Send Message' }}
            </button>
            <p class="form-note">We respond within 24 hours.</p>
          </div>
        </form>

        <div v-if="showSuccess" class="success-message">
          <div class="success-icon">
            <svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <polyline points="20,6 9,17 4,12"></polyline>
            </svg>
          </div>
          <h3>Message Sent</h3>
          <p>We'll get back to you within 24 hours.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- Office Hours -->
  <section class="hours-section">
    <div class="container">
      <div class="hours-content">
        <h2>Virtual Office Hours</h2>
        <p>Weekly open Q&A with our team. No appointment needed.</p>
        <div class="hours-details">
          <div class="hours-item">
            <span class="hours-label">When</span>
            <span class="hours-value">Fridays, 2:00 PM EST</span>
          </div>
          <div class="hours-item">
            <span class="hours-label">Where</span>
            <span class="hours-value">devEco™ Discord</span>
          </div>
        </div>
        <a href="https://discord.gg/deveco" target="_blank" class="btn btn-primary">Join Office Hours</a>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'

const isSubmitting = ref(false)
const showSuccess = ref(false)

const form = reactive({
  name: '',
  email: '',
  company: '',
  service: '',
  message: ''
})

const submitForm = async () => {
  isSubmitting.value = true

  try {
    await new Promise(resolve => setTimeout(resolve, 2000))

    form.name = ''
    form.email = ''
    form.company = ''
    form.service = ''
    form.message = ''

    showSuccess.value = true
    setTimeout(() => {
      showSuccess.value = false
    }, 5000)
  } catch (error) {
    console.error('Form submission error:', error)
  } finally {
    isSubmitting.value = false
  }
}
</script>

<style scoped>
.hero {
  background: var(--teal);
  color: white;
  padding: var(--space-xl) 0;
  text-align: center;
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
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
}

/* Contact Options */
.contact-section {
  background: var(--bg-white);
  border-bottom: 2px solid var(--border);
}

.contact-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: var(--space-lg);
}

.contact-option {
  background: white;
  border: 2px solid var(--border);
  padding: var(--space-xl);
  text-align: center;
  transition: all 0.2s;
}

.contact-option:hover {
  border-color: var(--teal);
  box-shadow: 6px 6px 0 var(--teal);
}

.contact-option.primary {
  border-color: var(--teal);
}

.contact-option .stamp {
  margin-bottom: var(--space-md);
}

.contact-option h3 {
  margin-bottom: var(--space-sm);
}

.contact-option p {
  font-size: 0.875rem;
  color: var(--text-mid);
  margin-bottom: var(--space-md);
}

.contact-link {
  color: var(--teal);
  font-weight: 600;
  text-decoration: none;
  transition: color 0.2s;
}

.contact-link:hover {
  color: var(--teal-dark);
  text-decoration: underline;
}

/* Form */
.form-section {
  background: var(--bg-light);
}

.form-wrapper {
  max-width: 700px;
  margin: 0 auto;
  position: relative;
}

.form-header {
  margin-bottom: var(--space-xl);
}

.form-header h2 {
  margin-bottom: var(--space-sm);
}

.form-header p {
  color: var(--text-mid);
}

.contact-form {
  background: white;
  border: 2px solid var(--border);
  padding: var(--space-xl);
}

.form-row {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: var(--space-md);
  margin-bottom: var(--space-md);
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-group.full-width {
  grid-column: 1 / -1;
}

.form-group label {
  font-size: 0.875rem;
  font-weight: 600;
  color: var(--navy);
  margin-bottom: var(--space-sm);
}

.form-group input,
.form-group select,
.form-group textarea {
  padding: var(--space-md);
  border: 2px solid var(--border);
  font-family: var(--font-mono);
  font-size: 0.875rem;
  transition: all 0.2s;
  background: white;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  border-color: var(--teal);
}

.form-group textarea {
  resize: vertical;
  min-height: 120px;
}

.form-actions {
  margin-top: var(--space-lg);
  text-align: center;
}

.form-note {
  margin-top: var(--space-md);
  font-size: 0.875rem;
  color: var(--text-mid);
}

.success-message {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: white;
  border: 2px solid var(--teal);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: var(--space-xl);
}

.success-icon {
  width: 80px;
  height: 80px;
  background: var(--teal);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
  margin-bottom: var(--space-lg);
}

.success-message h3 {
  margin-bottom: var(--space-sm);
}

.success-message p {
  color: var(--text-mid);
}

/* Hours */
.hours-section {
  background: var(--navy);
  color: white;
  text-align: center;
}

.hours-content h2 {
  color: white;
  margin-bottom: var(--space-md);
}

.hours-content > p {
  color: var(--text-light);
  margin-bottom: var(--space-lg);
}

.hours-details {
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  padding: var(--space-lg);
  margin-bottom: var(--space-lg);
  max-width: 400px;
  margin-left: auto;
  margin-right: auto;
}

.hours-item {
  display: flex;
  justify-content: space-between;
  padding: var(--space-sm) 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.hours-item:last-child {
  border-bottom: none;
}

.hours-label {
  color: var(--text-light);
}

.hours-value {
  font-weight: 600;
}

@media (max-width: 768px) {
  .contact-grid {
    grid-template-columns: 1fr;
  }

  .form-row {
    grid-template-columns: 1fr;
  }
}
</style>
