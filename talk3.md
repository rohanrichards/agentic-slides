---
theme: ./theme
title: "The Workflow"
info: |
  Talk 3 of the Agentic Coding Best Practices series.
  The full Discover → Design → Plan → Build workflow, security practices, and best practices in depth.
transition: slide-left
mdc: true
---

# The Workflow

Agentic Coding Best Practices — Talk 3

---
layout: section
---

# Discover &rarr; Design &rarr; Plan &rarr; Build

A structured approach to better outcomes

---

# Discover &rarr; Design &rarr; Plan &rarr; Build

<v-clicks>

- **Discover** — Understand the codebase before touching it
- **Design** — Think through the approach together
- **Plan** — Write a constrained, testable implementation plan
- **Build** — Execute with TDD and evidence-based verification

</v-clicks>

---
layout: section
---

# Discover

Understand the codebase before touching it

---

# Task-Agnostic Exploration

<!-- TODO: Expanded from Talk 2's intro to explore-first
  - Separate understanding from tasking
  - Don't mention the problem until you both understand the area
  - No problem solving yet — just learning
  - The agent explores better than you can curate
  (Reuse polished content from slides.md) -->

---

# Discover: In Action

<!-- TODO: TerminalBlock — agent exploring auth module
  (Reuse from slides.md) -->

---
layout: section
---

# Design

Think through the approach together

---

# Present the Problem in Plan Mode

<!-- TODO: The antidote to sycophancy (callback to Talk 2)
  - Start in plan mode — can think but can't write code
  - Describe the feature, let the agent process
  - See how it frames the problem
  - Ask for approaches, not code
  (Reuse polished content from slides.md) -->

---

# Then Drill In

<!-- TODO:
  - "What are you uncertain about?"
  - "What edge cases should we think about?"
  - "What's the simplest version of this that works?"
  - Make the agent a thinking partner, not a yes-man
  (Reuse polished content from slides.md) -->

---

# Design In Practice

<!-- TODO:
  - Hash out scope, surface edge cases
  - Tell it what NOT to build
  - Output: a design doc
  (Reuse polished content from slides.md) -->

---

# Design: In Action

<!-- TODO: TerminalBlock — agent asking clarifying questions
  (Reuse from slides.md) -->

---

# Design: As A Skill

<!-- TODO: Example brainstorming skill markdown
  (Reuse from slides.md) -->

---
layout: section
---

# Plan

Write a constrained, testable implementation plan

---

# What Planning Solves

<!-- TODO:
  - Over-engineering (callback to Talk 2)
  - Scope creep
  - The confidence spiral (callback to Talk 2)
  - If it's not in the plan, it doesn't get built
  (Reuse polished content from slides.md) -->

---

# Why Written Plans Matter

<!-- TODO:
  - Plans persist — survive context compaction and session clears
  - One-shot most work with a good plan
  - Plans are reviewable
  (Reuse polished content from slides.md) -->

---

# TDD Is Non-Negotiable

<!-- TODO: Deeper treatment building on Talk 2's red-green intro
  - Cherny: test suite is "single most important factor"
  - Every step: write test → watch fail → implement → watch pass
  - Tests after code describe what code does, not what it should do
  (Reuse polished content from slides.md) -->

---

# Plan In Practice

<!-- TODO: TerminalBlock — prompting for a good plan
  (Reuse from slides.md) -->

---

# Plan: As A Skill

<!-- TODO: Example writing-plans skill markdown
  (Reuse from slides.md) -->

---
layout: section
---

# Build

Execute the plan

---

# Let the Plan Do the Work

<!-- TODO:
  - Agent follows the plan, TDD baked in
  - Your job: reviewing and validating
  - Subagents for parallel execution
  (Reuse polished content from slides.md) -->

---

# Give Them Real Feedback Loops

<!-- TODO:
  - Built an endpoint? Curl it and check the DB
  - Built a form? Open a browser and fill it out
  - Verification feedback loop = 2-3x quality
  (Reuse polished content from slides.md) -->

---

# Build: Validation

<!-- TODO:
  - End-to-end validation
  - AI reviews AI: fresh session reviews previous session's code
  - The agent proves it works, not just claims it does
  (Reuse polished content from slides.md) -->

---

# Build: Human In The Loop

<!-- TODO:
  - Use the thing yourself
  - Review the code and the commits
  - QA it — try the edge cases, break it on purpose
  - Willison's golden rule
  (Reuse polished content from slides.md) -->

---
layout: section
---

# Security Practices

The risks you need to understand

---

# The Lethal Trifecta

<!-- TODO:
  - Simon Willison: same attack across 11+ products (2023-2025)
  - Three capabilities lethal when combined:
    1. Access to private data
    2. Exposure to untrusted content
    3. External communication abilities
  - The rule: never give an agent more than two of the three
  (Reuse polished content from slides.md) -->

---

# No External MCPs

<!-- TODO:
  - Each external MCP = unaudited code execution
  - Postmark attack: one line, thousands of stolen emails
  - GitHub MCP data heist: malicious issue exfiltrates salary data
  - Anthropic's own Inspector tool had RCE vulnerability
  - Prefer local. Audit everything. If you can't audit it, don't connect it
  (Reuse polished content from slides.md) -->

---

# MCP Attack: In Practice

<!-- TODO: TerminalBlock — GitHub MCP prompt injection example
  (Reuse from slides.md) -->

---
layout: section
---

# The Bigger Picture

---

# Human Risks

<!-- TODO:
  - Generation vs discrimination — skill atrophy
  - The 80% problem — feels done before it is
  - METR 2025: believed 20% faster, actually 19% slower
  (Reuse polished content from slides.md) -->

---

# Putting It Together

<!-- TODO:
  - Discover → Design → Plan → Build summary
  - The workflow channels all the best practices into a repeatable process
  (Reuse polished content from slides.md) -->

---

# What to Try Next

- Run the **full workflow** on a real task end-to-end
- **Discover** before you mention the problem
- **Design** in plan mode — ask for approaches, not code
- **Plan** with TDD baked into every step
- **Build** with verification — the agent proves it works

---

# Sources

<!-- TODO: Full sources table -->

---
layout: section
---

# Questions?

Talk 3 of the Agentic Coding Best Practices series
