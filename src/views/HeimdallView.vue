<template>
  <section class="heimdall-hero">
    <div class="container">
      <p class="label">You found the Bifrost</p>
      <h1>Heimdall</h1>
      <p class="heimdall-subtitle">
        The all-seeing gateway to the devEco™ universe. You weren't supposed to find this.
      </p>
    </div>
  </section>

  <section class="heimdall-content">
    <div class="container">
      <div class="section-header">
        <h2>What is this place?</h2>
      </div>
      <p class="heimdall-text">
        This is a hidden corner of the devEco™ ecosystem. If you found your way here, you were curious enough to look beyond the surface. That's exactly the kind of person we build for.
      </p>
      <p class="heimdall-text">
        More will appear here in time. For now, you've proven you pay attention to the details.
      </p>
    </div>
  </section>
</template>

<style scoped>
.heimdall-hero {
  background: var(--navy);
  color: white;
  padding: var(--space-2xl) 0;
  text-align: center;
}

.heimdall-hero .label {
  color: var(--teal-light);
  margin-bottom: var(--space-md);
}

.heimdall-hero h1 {
  color: white;
  margin-bottom: var(--space-lg);
}

.heimdall-subtitle {
  font-size: 0.875rem;
  color: var(--text-mid);
  max-width: 600px;
  margin: 0 auto;
}

.heimdall-content {
  background: var(--bg-light);
}

.heimdall-text {
  font-size: 1rem;
  color: var(--text-mid);
  max-width: 600px;
  line-height: 1.7;
  margin-bottom: var(--space-md);
}
</style>
