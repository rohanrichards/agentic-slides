<script setup>
defineProps({
  text: { type: String, default: '' },
})
</script>

<template>
  <span class="term-label term-claude">&#9881; claude</span> <span class="term-text term-claude-text">{{ text }}<slot /></span>
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

.term-claude {
  color: #a78bfa;
  background: rgba(124, 58, 237, 0.1);
}

.term-text {
  color: #e6edf3;
}

.term-claude-text {
  color: #8b949e;
}
</style>
