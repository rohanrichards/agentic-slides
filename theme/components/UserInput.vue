<script setup>
defineProps({
  text: { type: String, default: '' },
})
</script>

<template>
  <span class="term-label term-you">&#9998; you</span> <span class="term-text">{{ text }}<slot /></span>
</template>

<style scoped>
.term-label {
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.6rem;
  font-weight: 600;
  padding: 0.1rem 0.35rem;
  border-radius: 3px;
  margin-right: 0.3rem;
}

.term-you {
  color: #00d4ff;
  background: rgba(0, 212, 255, 0.1);
}

.term-text {
  color: #e6edf3;
}
</style>
