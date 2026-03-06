<script setup>
import { ref, onMounted } from 'vue'

const props = defineProps({
  code: { type: String, required: true },
  speed: { type: Number, default: 30 },
  delay: { type: Number, default: 500 },
})

const displayed = ref('')
const showCursor = ref(true)

onMounted(() => {
  setTimeout(() => {
    let i = 0
    const interval = setInterval(() => {
      if (i < props.code.length) {
        displayed.value += props.code[i]
        i++
      } else {
        clearInterval(interval)
        setTimeout(() => { showCursor.value = false }, 1500)
      }
    }, props.speed)
  }, props.delay)
})
</script>

<template>
  <div class="gp-animated-code">
    <pre><code>{{ displayed }}<span v-if="showCursor" class="gp-cursor">|</span></code></pre>
  </div>
</template>

<style scoped>
.gp-animated-code pre {
  background: var(--gp-bg-surface) !important;
  border: 1px solid var(--gp-border) !important;
  border-left: 3px solid var(--gp-accent) !important;
  border-radius: var(--gp-radius);
  padding: 1.25rem !important;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.95rem;
  line-height: 1.6;
  color: var(--gp-text);
  animation: none !important;
}

.gp-cursor {
  color: var(--gp-accent);
  animation: gp-blink 0.8s step-end infinite;
}

@keyframes gp-blink {
  50% { opacity: 0; }
}
</style>
