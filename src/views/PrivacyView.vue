<script setup lang="ts">
// Static privacy and cookies statement. No backend, no icons.
</script>

<template>
  <!-- Hero Section -->
  <section class="hero">
    <div class="container">
      <div class="hero-content">
        <span class="stamp stamp-teal">Privacy</span>
        <h1>Privacy and Cookies</h1>
        <p class="hero-subtitle">How thedeveco.com handles your information.</p>
      </div>
    </div>
  </section>

  <!-- Statement -->
  <section class="statement-section">
    <div class="container">
      <div class="statement">
        <h2>Overview</h2>
        <p>
          devEco Consulting LLC operates thedeveco.com. This statement explains what information
          the site does and does not collect, in plain language. It is current as of 2026-06-26.
        </p>

        <h2>Information We Collect</h2>
        <p>
          The site does not collect or store personal information. There is no account system, no
          analytics, and no tracking. If you contact devEco through the email or social channels
          listed on the <router-link to="/contact">Contact</router-link> page, devEco receives only
          the information you choose to share in that message.
        </p>

        <h2>Cookies and Local Storage</h2>
        <p>
          The site uses only strictly necessary browser storage required for it to function: a
          single session storage entry that supports in-app navigation. It does not set cookies for
          advertising, analytics, or tracking. Because only essential storage is used, consent is
          not legally required, but the site shows a brief notice for transparency. You can clear
          this storage at any time through your browser settings.
        </p>

        <h2>Third-Party Services</h2>
        <p>
          The site loads the JetBrains Mono typeface from Google Fonts. When your browser requests
          the font files, Google may receive standard request data such as your IP address, handled
          under Google's own privacy policy. The site is hosted on GitHub Pages, and as part of
          serving the site, GitHub may process standard server information such as IP addresses,
          handled under GitHub's privacy practices. devEco does not control or receive this data.
        </p>

        <h2>Your Choices</h2>
        <p>
          You can dismiss the cookie notice by selecting Got it. You can clear or block browser
          storage through your browser settings at any time. Because the site does not track you,
          there is nothing to opt out of.
        </p>

        <h2>Contact</h2>
        <p>
          Questions about this statement can be sent to
          <a href="mailto:hello@thedeveco.com">hello@thedeveco.com</a>.
        </p>

        <h2>Changes</h2>
        <p>
          devEco may update this statement as the site changes. This version is current as of
          2026-06-26.
        </p>
      </div>
    </div>
  </section>
</template>

<style scoped>
.hero {
  background: var(--navy);
  color: white;
  text-align: center;
  padding: var(--space-2xl) 0;
}

.hero-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--space-md);
}

.hero h1 {
  color: white;
}

.hero-subtitle {
  font-size: 0.9375rem;
  color: var(--bg-light);
  max-width: 600px;
}

.statement {
  max-width: 720px;
  margin: 0 auto;
}

.statement h2 {
  margin-top: var(--space-xl);
  margin-bottom: var(--space-md);
}

.statement h2:first-child {
  margin-top: 0;
}

.statement p {
  font-size: 0.875rem;
  color: var(--text-mid);
  line-height: 1.7;
}

.statement a {
  color: var(--teal);
  text-decoration: underline;
}
</style>
