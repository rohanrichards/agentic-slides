---
theme: ./theme
title: "Levelling Up"
info: |
  Talk 4 of the Agentic Coding Best Practices series.
  Skills, hooks, advanced patterns, and the self-improving system.
transition: slide-left
mdc: true
---

# Levelling Up

Agentic Coding Best Practices — Talk 4

---
layout: section
---

# Skills

Encode your workflows so the agent follows them automatically

---

# What Are Skills?

<!-- TODO:
  - Markdown files with YAML frontmatter
  - Automatically trigger based on context
  - Lightweight vs token-expensive API schemas
  - Willison: "potentially a bigger deal than MCP"
  - Project-level skills checked into repo — team infrastructure -->

---

# Project Skills

<!-- TODO: Practical examples:
  - PR writing skill — template for good PR descriptions
  - Code review skill — what your team considers a good review
  - Commit conventions skill
  - Onboarding context skill — summarizes architecture for new devs/fresh sessions -->

---

# Building a Skill

<!-- TODO: Live example or walkthrough
  - Show a real SKILL.md file
  - The brainstorming skill from Talk 3
  - The writing-plans skill from Talk 3
  - Tip: use Claude to help build skills — "Help me make a skill that does X"
  - Claude Code is great at meta tasks — use the agent to learn the agent's tooling -->

---
layout: section
---

# Hooks

Guardrails that enforce practices programmatically

---

# What Are Hooks?

<!-- TODO:
  - User-defined shell commands at lifecycle points
  - Pre-tool hooks (before agent writes/edits)
  - Post-tool hooks (after agent writes/edits)
  - Can approve, deny, or modify tool requests
  - Rules that are enforced, not just advisory -->

---

# Essential Hooks

<!-- TODO:
  - Branch check — verify you're on a feature branch before any commit
  - Commit message format — enforce conventional commits
  - Protected file guard — reject writes to .env, CI configs
  - Dead code eliminator — detect orphaned imports after edits
  - Test existence check — after creating a file, check if corresponding test exists
  - Auto-format — run prettier/eslint on files the agent touched -->

---

# Building a Hook

<!-- TODO: Live example or walkthrough
  - Show a real hook configuration
  - Hooks are just scripts — you can test them
  - When new failure patterns emerge, add them to the hook's test suite -->

---
layout: section
---

# Advanced Patterns

---

# AI Reviews AI

<!-- TODO:
  - Open a fresh session with no prior context
  - Have it review the code written by the previous session
  - Fresh session doesn't have the same biases
  - Split reviews: spec compliance (did it do what was asked?) + code quality (is it well-written?)
  - Cherny's team: "For each PR, we open a new context window and let Claude review the code written by Claude" -->

---

# Parallel Sessions

<!-- TODO:
  - Cherny: 5 simultaneous sessions, each in its own git checkout
  - Subagents for isolated subtasks
  - Writer/Reviewer pattern: Session A implements, Session B reviews in clean context
  - Tasks feature: dependencies and blockers, shared task lists for coordination -->

---

# Spec-Driven Development

<!-- TODO:
  - Write the spec before Phase 1 (Discover)
  - The agent has a contract to work against
  - Discover is more targeted, Design is more grounded, Build has clearer acceptance criteria
  - Reduces the confidence spiral — agent has something concrete to verify against
  - Could frame as "Phase 0" that makes everything downstream better -->

---
layout: section
---

# The Self-Improving System

---

# The Feedback Loop

<!-- TODO:
  - Developer uses skills and hooks
  - Agent produces output following standards
  - Developer reviews, accepts or corrects
  - Corrections signal where skills/hooks are incomplete
  - Skills get updated
  - Everyone gets the improvement on next sync
  - Repeat — standards improve faster than manual wiki updates -->

---

# Corrections → Rules → Skills → Hooks

<!-- TODO: Visual showing the progression:
  - Correction: "Don't use console.log, use the logger"
  - Rule: Added to CLAUDE.md
  - Skill: Encoded workflow that checks logging patterns
  - Hook: Automated check that rejects console.log in PRs
  - Each level is more automated, less dependent on human memory -->

---

# Testing the System Itself

<!-- TODO:
  - Unit tests for hooks
  - Behavioral tests for skills
  - End-to-end testing of skill chains
  - New failure patterns → add to test suite
  - The system's quality bar rises continuously -->

---

# Where to Go From Here

- **Create your first skill** — start with a PR template or code review checklist
- **Add your first hook** — branch check is the easiest starting point
- **Try AI reviews AI** — open a fresh session to review your last PR
- **Contribute to your team's CLAUDE.md** — every correction is a contribution
- **Build the self-improving system** — corrections → rules → skills → hooks

---

# The Series in Review

<!-- TODO:
  Talk 1: Getting Started Right — set up properly from day one
  Talk 2: The Pitfalls — name the problems, learn the first habits
  Talk 3: The Workflow — Discover → Design → Plan → Build
  Talk 4: Levelling Up — automation and the self-improving system

  The arc: learn the tool → understand the problems → apply structure → automate the structure -->

---

# Sources

<!-- TODO: Full sources table -->

---
layout: section
---

# Questions?

Talk 4 of the Agentic Coding Best Practices series
