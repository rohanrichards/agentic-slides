<template>
  <div class="slidev-layout gp-code-focus gp-mesh-bg">
    <div class="gp-code-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.gp-code-focus {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 100%;
  padding: 2rem 3rem;
}

.gp-code-content {
  position: relative;
  z-index: 1;
}

.gp-code-focus :deep(pre) {
  font-size: 1rem;
  max-height: 80vh;
}

.gp-code-focus :deep(h1),
.gp-code-focus :deep(h2),
.gp-code-focus :deep(h3) {
  margin-bottom: 1rem;
  font-size: 1.5rem;
}
</style>
