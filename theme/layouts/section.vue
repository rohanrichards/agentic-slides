<template>
  <div class="slidev-layout gp-section gp-mesh-bg">
    <div class="gp-section-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.gp-section {
  display: flex;
  align-items: center;
  height: 100%;
  padding: 3rem 4rem;
}

.gp-section-content {
  position: relative;
  z-index: 1;
}

.gp-section :deep(h1) {
  font-size: 3.5rem;
  font-weight: 700;
  letter-spacing: -0.03em;
  animation: gp-headline-in 0.6s cubic-bezier(0.22, 1, 0.36, 1) both;
}

.gp-section :deep(h1)::after {
  content: '';
  display: block;
  height: 3px;
  margin-top: 1rem;
  background: linear-gradient(90deg, var(--gp-accent), var(--gp-accent-secondary), transparent);
  animation: gp-underline-draw 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.3s both;
}

.gp-section :deep(p) {
  color: var(--gp-text-muted);
  font-size: 1.25rem;
  margin-top: 1rem;
  animation: gp-fade-up 0.5s cubic-bezier(0.22, 1, 0.36, 1) 0.4s both;
}
</style>
