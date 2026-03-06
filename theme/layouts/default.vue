<template>
  <div class="slidev-layout gp-default gp-mesh-bg gp-grid-overlay">
    <div class="gp-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.gp-default {
  display: flex;
  flex-direction: column;
  padding: 2.5rem 3rem;
  height: 100%;
}

.gp-content {
  flex: 1;
  position: relative;
  z-index: 1;
}
</style>
