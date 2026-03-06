<template>
  <div class="gp-glow-card">
    <div class="gp-glow-card-inner">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.gp-glow-card {
  position: relative;
  border-radius: var(--gp-radius);
  padding: 1px;
  background: linear-gradient(135deg, var(--gp-accent), var(--gp-accent-secondary), var(--gp-accent));
  background-size: 200% 200%;
  animation: gp-glow-rotate 4s ease-in-out infinite;
}

@keyframes gp-glow-rotate {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.gp-glow-card::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: var(--gp-radius);
  background: inherit;
  filter: blur(12px);
  opacity: 0.4;
  z-index: -1;
}

.gp-glow-card-inner {
  background: var(--gp-bg-surface);
  border-radius: calc(var(--gp-radius) - 1px);
  padding: 1.5rem;
  backdrop-filter: blur(10px);
}
</style>
