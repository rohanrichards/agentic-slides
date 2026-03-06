<template>
  <div class="slidev-layout gp-cover gp-mesh-bg gp-grid-overlay">
    <div class="gp-cover-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.gp-cover {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  text-align: center;
  padding: 3rem;
}

.gp-cover-content {
  position: relative;
  z-index: 1;
  max-width: 80%;
}

.gp-cover :deep(h1) {
  font-size: 4rem;
  font-weight: 700;
  letter-spacing: -0.04em;
  line-height: 1.1;
  background: linear-gradient(135deg, var(--gp-accent) 0%, var(--gp-accent-secondary) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: gp-headline-in 0.8s cubic-bezier(0.22, 1, 0.36, 1) both;
}

.gp-cover :deep(p) {
  color: var(--gp-text-muted);
  font-size: 1.25rem;
  margin-top: 1rem;
  animation: gp-fade-up 0.6s cubic-bezier(0.22, 1, 0.36, 1) 0.3s both;
}
</style>
