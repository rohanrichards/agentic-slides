# Geekpunk Theme Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a stunning local Slidev theme called "Geekpunk" — dark, cyan-accented, animated, startup-hacker aesthetic.

**Architecture:** Local theme at `./theme/` referenced via `theme: ./theme` in slides.md headmatter. Theme provides CSS variables, animations, 7 custom layouts, and 3 custom Vue components. Pure CSS animations (no JS animation libraries). UnoCSS available for utility classes.

**Tech Stack:** Vue 3 SFCs, CSS custom properties, CSS `@keyframes`, UnoCSS utilities, Google Fonts (Inter, JetBrains Mono)

---

### Task 1: Theme Scaffold — package.json + styles entry

**Files:**
- Create: `theme/package.json`
- Create: `theme/styles/index.ts`

**Step 1: Create theme package.json**

```json
{
  "name": "slidev-theme-geekpunk",
  "version": "0.1.0",
  "keywords": ["slidev-theme"],
  "slidev": {
    "defaults": {
      "fonts": {
        "sans": "Inter",
        "mono": "JetBrains Mono",
        "weights": "400,500,600,700"
      },
      "transition": "slide-left"
    }
  }
}
```

**Step 2: Create styles entry**

```ts
import './base.css'
import './animations.css'
import './layouts.css'
```

**Step 3: Commit**

```bash
git add theme/
git commit -m "feat: scaffold geekpunk theme package"
```

---

### Task 2: Base CSS — Variables, Typography, Global Styles

**Files:**
- Create: `theme/styles/base.css`

**Step 1: Write base.css with all design tokens and global styles**

```css
:root {
  --gp-bg: #0d1117;
  --gp-bg-surface: #161b22;
  --gp-bg-elevated: #1c2128;
  --gp-text: #e6edf3;
  --gp-text-muted: #8b949e;
  --gp-accent: #00d4ff;
  --gp-accent-secondary: #7c3aed;
  --gp-accent-glow: rgba(0, 212, 255, 0.15);
  --gp-border: rgba(139, 148, 158, 0.2);
  --gp-code-bg: rgba(0, 212, 255, 0.05);
  --gp-radius: 8px;

  --slidev-theme-primary: var(--gp-accent);
}

/* Base slide styling */
.slidev-layout {
  background: var(--gp-bg);
  color: var(--gp-text);
  font-family: 'Inter', ui-sans-serif, system-ui, sans-serif;
  position: relative;
  overflow: hidden;
}

/* Typography */
.slidev-layout h1 {
  color: var(--gp-text);
  font-weight: 700;
  letter-spacing: -0.03em;
  line-height: 1.1;
}

.slidev-layout h2 {
  color: var(--gp-text);
  font-weight: 600;
  letter-spacing: -0.02em;
}

.slidev-layout h3 {
  color: var(--gp-text-muted);
  font-weight: 500;
}

.slidev-layout h1 + p {
  color: var(--gp-text-muted);
  margin-top: -0.5rem;
}

/* Links */
.slidev-layout a {
  color: var(--gp-accent);
  text-decoration: none;
  border-bottom: 1px solid rgba(0, 212, 255, 0.3);
  transition: border-color 0.2s ease;
}

.slidev-layout a:hover {
  border-color: var(--gp-accent);
}

/* Code blocks */
.slidev-layout pre {
  background: var(--gp-bg-surface) !important;
  border: 1px solid var(--gp-border);
  border-left: 3px solid var(--gp-accent);
  border-radius: var(--gp-radius);
  padding: 1.25rem !important;
}

.slidev-layout :not(pre) > code {
  background: var(--gp-code-bg);
  color: var(--gp-accent);
  padding: 0.15em 0.4em;
  border-radius: 4px;
  font-size: 0.9em;
  font-family: 'JetBrains Mono', monospace;
}

/* Lists */
.slidev-layout ul > li::marker {
  color: var(--gp-accent);
}

.slidev-layout ol > li::marker {
  color: var(--gp-accent);
  font-weight: 600;
}

/* Blockquotes */
.slidev-layout blockquote {
  border-left: 3px solid var(--gp-accent);
  padding-left: 1rem;
  color: var(--gp-text-muted);
  font-style: italic;
}

/* Strong/emphasis */
.slidev-layout strong {
  color: var(--gp-text);
  font-weight: 600;
}

/* Horizontal rule */
.slidev-layout hr {
  border: none;
  height: 1px;
  background: linear-gradient(90deg, var(--gp-accent), var(--gp-accent-secondary), transparent);
  margin: 2rem 0;
}

/* Slide number */
.slidev-page-number {
  color: var(--gp-text-muted) !important;
  font-family: 'JetBrains Mono', monospace !important;
  font-size: 0.75rem !important;
}
```

**Step 2: Commit**

```bash
git add theme/styles/base.css
git commit -m "feat: add geekpunk base CSS variables and typography"
```

---

### Task 3: Animations CSS — Keyframes, Transitions, v-click Overrides

**Files:**
- Create: `theme/styles/animations.css`

**Step 1: Write all animation keyframes and transition overrides**

```css
/* === Gradient Mesh Background === */
@keyframes gp-mesh-drift {
  0%, 100% {
    background-position: 0% 50%;
  }
  25% {
    background-position: 50% 0%;
  }
  50% {
    background-position: 100% 50%;
  }
  75% {
    background-position: 50% 100%;
  }
}

.gp-mesh-bg::before {
  content: '';
  position: absolute;
  inset: -50%;
  background: radial-gradient(ellipse at 20% 50%, rgba(0, 212, 255, 0.08) 0%, transparent 50%),
              radial-gradient(ellipse at 80% 20%, rgba(124, 58, 237, 0.06) 0%, transparent 50%),
              radial-gradient(ellipse at 50% 80%, rgba(0, 212, 255, 0.04) 0%, transparent 50%);
  background-size: 200% 200%;
  animation: gp-mesh-drift 20s ease-in-out infinite;
  z-index: 0;
  pointer-events: none;
}

.gp-mesh-bg > * {
  position: relative;
  z-index: 1;
}

/* === Grid Overlay (subtle) === */
.gp-grid-overlay::after {
  content: '';
  position: absolute;
  inset: 0;
  background-image:
    linear-gradient(rgba(139, 148, 158, 0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(139, 148, 158, 0.03) 1px, transparent 1px);
  background-size: 60px 60px;
  z-index: 0;
  pointer-events: none;
}

/* === v-click animation overrides === */
.slidev-vclick-target {
  transition: all 0.4s cubic-bezier(0.22, 1, 0.36, 1);
}

.slidev-vclick-hidden {
  opacity: 0;
  transform: translateY(12px);
}

.slidev-vclick-current,
.slidev-vclick-prior {
  opacity: 1;
  transform: translateY(0);
}

.slidev-vclick-prior {
  opacity: 0.7;
}

/* === Headline reveal === */
@keyframes gp-headline-in {
  from {
    opacity: 0;
    transform: translateY(20px);
    filter: blur(4px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
    filter: blur(0);
  }
}

.gp-headline-reveal {
  animation: gp-headline-in 0.6s cubic-bezier(0.22, 1, 0.36, 1) both;
}

/* === Underline draw === */
@keyframes gp-underline-draw {
  from {
    width: 0;
  }
  to {
    width: 100%;
  }
}

.gp-underline {
  position: relative;
  display: inline-block;
}

.gp-underline::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  height: 2px;
  background: linear-gradient(90deg, var(--gp-accent), var(--gp-accent-secondary));
  animation: gp-underline-draw 0.8s cubic-bezier(0.22, 1, 0.36, 1) 0.3s both;
}

/* === Fade up for general elements === */
@keyframes gp-fade-up {
  from {
    opacity: 0;
    transform: translateY(16px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.gp-fade-up {
  animation: gp-fade-up 0.5s cubic-bezier(0.22, 1, 0.36, 1) both;
}

/* === Code block glow on enter === */
@keyframes gp-code-glow {
  0% {
    border-color: var(--gp-border);
    box-shadow: none;
  }
  50% {
    border-color: var(--gp-accent);
    box-shadow: 0 0 20px rgba(0, 212, 255, 0.1);
  }
  100% {
    border-color: var(--gp-border);
    box-shadow: none;
  }
}

.slidev-layout pre {
  animation: gp-code-glow 2s ease 0.3s both;
}

/* === Gradient text utility === */
.gp-gradient-text {
  background: linear-gradient(135deg, var(--gp-accent), var(--gp-accent-secondary));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* === Slide transition overrides === */
.slide-left-enter-active,
.slide-left-leave-active {
  transition: all 0.4s cubic-bezier(0.22, 1, 0.36, 1);
}

.slide-left-enter-from {
  opacity: 0;
  transform: translateX(40px) scale(0.98);
}

.slide-left-leave-to {
  opacity: 0;
  transform: translateX(-40px) scale(0.98);
}
```

**Step 2: Commit**

```bash
git add theme/styles/animations.css
git commit -m "feat: add geekpunk animations — mesh bg, v-click, headlines, glow"
```

---

### Task 4: Layouts CSS + Default Layout

**Files:**
- Create: `theme/styles/layouts.css`
- Create: `theme/layouts/default.vue`

**Step 1: Write layouts.css**

```css
/* Layout-specific overrides */
.slidev-layout.gp-cover h1 {
  font-size: 3.5rem;
  letter-spacing: -0.04em;
}

.slidev-layout.gp-section h1 {
  font-size: 3rem;
  letter-spacing: -0.03em;
}

.slidev-layout.gp-quote blockquote {
  font-size: 1.5rem;
  line-height: 1.6;
  border-left-width: 4px;
}

.slidev-layout.gp-code-focus pre {
  font-size: 1.1rem;
}
```

**Step 2: Write default layout**

```vue
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
```

**Step 3: Verify — update slides.md to use `theme: ./theme`, run dev server, confirm default layout renders with dark bg + mesh**

**Step 4: Commit**

```bash
git add theme/styles/layouts.css theme/layouts/default.vue
git commit -m "feat: add layouts CSS and default layout with mesh background"
```

---

### Task 5: Cover Layout

**Files:**
- Create: `theme/layouts/cover.vue`

**Step 1: Write cover layout — full-screen centered, gradient text headline, animated mesh**

```vue
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
```

**Step 2: Verify — first slide in slides.md should use `layout: cover`, check gradient text + animation**

**Step 3: Commit**

```bash
git add theme/layouts/cover.vue
git commit -m "feat: add cover layout with gradient text and reveal animation"
```

---

### Task 6: Section, Quote, Two-Cols, Code-Focus, Image-Full Layouts

**Files:**
- Create: `theme/layouts/section.vue`
- Create: `theme/layouts/quote.vue`
- Create: `theme/layouts/two-cols.vue`
- Create: `theme/layouts/code-focus.vue`
- Create: `theme/layouts/image-full.vue`

**Step 1: Write section.vue**

```vue
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
```

**Step 2: Write quote.vue**

```vue
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
```

**Step 3: Write two-cols.vue**

```vue
<template>
  <div class="slidev-layout gp-two-cols gp-mesh-bg gp-grid-overlay">
    <div class="gp-cols-header" v-if="$slots.default">
      <slot />
    </div>
    <div class="gp-cols-container">
      <div class="gp-col gp-col-left">
        <slot name="left" />
      </div>
      <div class="gp-col-divider" />
      <div class="gp-col gp-col-right">
        <slot name="right" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.gp-two-cols {
  display: flex;
  flex-direction: column;
  height: 100%;
  padding: 2.5rem 3rem;
}

.gp-cols-header {
  position: relative;
  z-index: 1;
  margin-bottom: 1.5rem;
}

.gp-cols-container {
  display: flex;
  flex: 1;
  gap: 2rem;
  position: relative;
  z-index: 1;
}

.gp-col {
  flex: 1;
}

.gp-col-divider {
  width: 1px;
  background: linear-gradient(180deg, transparent, var(--gp-border), transparent);
}
</style>
```

**Step 4: Write code-focus.vue**

```vue
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
```

**Step 5: Write image-full.vue**

```vue
<script setup>
defineProps({
  image: { type: String, required: true },
  dim: { type: String, default: '0.4' },
})
</script>

<template>
  <div class="slidev-layout gp-image-full">
    <img :src="image" class="gp-bg-image" />
    <div class="gp-image-overlay" :style="{ opacity: dim }" />
    <div class="gp-image-content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.gp-image-full {
  position: relative;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 3rem;
}

.gp-bg-image {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.gp-image-overlay {
  position: absolute;
  inset: 0;
  background: var(--gp-bg);
}

.gp-image-content {
  position: relative;
  z-index: 1;
}

.gp-image-content :deep(h1) {
  font-size: 3rem;
  text-shadow: 0 2px 20px rgba(0, 0, 0, 0.5);
}
</style>
```

**Step 6: Verify — update slides.md with one slide per layout, check each renders correctly**

**Step 7: Commit**

```bash
git add theme/layouts/
git commit -m "feat: add section, quote, two-cols, code-focus, image-full layouts"
```

---

### Task 7: GlowCard Component

**Files:**
- Create: `theme/components/GlowCard.vue`

**Step 1: Write GlowCard — glassmorphism card with animated border glow**

```vue
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
```

**Step 2: Commit**

```bash
git add theme/components/GlowCard.vue
git commit -m "feat: add GlowCard component with animated border glow"
```

---

### Task 8: TerminalBlock Component

**Files:**
- Create: `theme/components/TerminalBlock.vue`

**Step 1: Write TerminalBlock — terminal window chrome with colored dots and monospace content**

```vue
<script setup>
defineProps({
  title: { type: String, default: 'terminal' },
})
</script>

<template>
  <div class="gp-terminal">
    <div class="gp-terminal-header">
      <div class="gp-terminal-dots">
        <span class="gp-dot gp-dot-red" />
        <span class="gp-dot gp-dot-yellow" />
        <span class="gp-dot gp-dot-green" />
      </div>
      <span class="gp-terminal-title">{{ title }}</span>
      <div class="gp-terminal-spacer" />
    </div>
    <div class="gp-terminal-body">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.gp-terminal {
  border-radius: var(--gp-radius);
  overflow: hidden;
  border: 1px solid var(--gp-border);
  background: var(--gp-bg-surface);
  animation: gp-code-glow 2s ease 0.3s both;
}

.gp-terminal-header {
  display: flex;
  align-items: center;
  padding: 0.6rem 1rem;
  background: var(--gp-bg-elevated);
  border-bottom: 1px solid var(--gp-border);
}

.gp-terminal-dots {
  display: flex;
  gap: 6px;
}

.gp-dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
}

.gp-dot-red { background: #ff5f57; }
.gp-dot-yellow { background: #febc2e; }
.gp-dot-green { background: #28c840; }

.gp-terminal-title {
  flex: 1;
  text-align: center;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.75rem;
  color: var(--gp-text-muted);
}

.gp-terminal-spacer {
  width: 52px;
}

.gp-terminal-body {
  padding: 1.25rem;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.9rem;
  line-height: 1.6;
  color: var(--gp-text);
}

.gp-terminal-body :deep(pre) {
  background: none !important;
  border: none !important;
  padding: 0 !important;
  margin: 0;
  animation: none !important;
  box-shadow: none !important;
}
</style>
```

**Step 2: Commit**

```bash
git add theme/components/TerminalBlock.vue
git commit -m "feat: add TerminalBlock component with window chrome"
```

---

### Task 9: AnimatedCode Component

**Files:**
- Create: `theme/components/AnimatedCode.vue`

**Step 1: Write AnimatedCode — typewriter effect for code display**

```vue
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
```

**Step 2: Commit**

```bash
git add theme/components/AnimatedCode.vue
git commit -m "feat: add AnimatedCode component with typewriter effect"
```

---

### Task 10: Wire Theme Into slides.md + Demo All Layouts

**Files:**
- Modify: `slides.md`

**Step 1: Update slides.md headmatter to `theme: ./theme` and create demo slides for each layout**

Update the frontmatter to `theme: ./theme` and create one slide per layout + component demo slides to verify everything works visually.

**Step 2: Run `npm run dev`, visually verify all layouts and components**

**Step 3: Commit**

```bash
git add slides.md
git commit -m "feat: wire geekpunk theme and add layout demo slides"
```

---

### Task 11: Polish Pass — Visual QA

**Step 1: Run dev server and review each slide**

Check for:
- Mesh background rendering on all layouts
- v-click fade-up animations working
- Code block glow animation
- GlowCard border animation
- TerminalBlock window chrome
- AnimatedCode typewriter
- Gradient text on cover
- Section underline draw
- Two-cols divider
- Quote border styling

**Step 2: Fix any visual issues found**

**Step 3: Final commit**

```bash
git add -A
git commit -m "fix: polish pass — visual QA fixes"
```
