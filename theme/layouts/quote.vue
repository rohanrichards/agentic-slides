<template>
  <div class="slidev-layout gp-quote gp-mesh-bg">
    <div class="gp-quote-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.gp-quote {
  display: flex;
  align-items: center;
  height: 100%;
  padding: 3rem 5rem;
}

.gp-quote-content {
  position: relative;
  z-index: 1;
  max-width: 85%;
}

.gp-quote :deep(blockquote),
.gp-quote :deep(p:first-child) {
  font-size: 1.75rem;
  line-height: 1.6;
  font-style: italic;
  color: var(--gp-text);
  border-left: 4px solid var(--gp-accent);
  padding-left: 1.5rem;
  animation: gp-fade-up 0.6s cubic-bezier(0.22, 1, 0.36, 1) both;
}

.gp-quote :deep(p:last-child) {
  color: var(--gp-text-muted);
  font-size: 1rem;
  font-style: normal;
  margin-top: 1.5rem;
  padding-left: 1.5rem;
  animation: gp-fade-up 0.5s cubic-bezier(0.22, 1, 0.36, 1) 0.2s both;
}
</style>
