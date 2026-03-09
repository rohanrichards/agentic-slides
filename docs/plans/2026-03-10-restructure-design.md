# Slide Deck Restructure: Agentic Coding Best Practices

**Date:** 2026-03-10
**Status:** Design approved

## Summary

Restructure the slide deck from its current flow (problems → phased workflow) to a three-part structure: comprehensive flaws → best practices introduced conceptually → phased workflow showing how to apply them. Adds security and human risk dimensions that were missing. Preserves all existing polished workflow content.

## Title

**Agentic Coding Best Practices** (unchanged)

---

## Structure

### Opening (existing, keep as-is)

| # | Slide | Status | Notes |
|---|-------|--------|-------|
| 1 | Title slide | Keep | Update subtitle if title changes |
| 2 | What Are Agents? (two-cols, Willison/Karpathy/Cherny quotes) | Keep | |

### Part 1: The Flaws (new section)

Replaces the single "Common Problems" slide with a comprehensive, grouped treatment. Each group gets an example slide.

| # | Slide | Status | Notes |
|---|-------|--------|-------|
| 3 | Section divider: "The Flaws" | New | |
| 4 | What Does Everyone Agree On? | Keep | Existing slide, same position — now introduces flaws section |
| 5 | Code Quality Flaws (1 of 2) | New | Assumption cascading, sycophancy, over-engineering, unverified claims. Uses polished wording from existing Assumption Cascading + Sycophancy slides |
| 6 | Example slide | New | TerminalBlock. Candidates: Nathan Onn over-engineering story (asked for email OTP, got enterprise architecture, constrained to 120 lines) or Replit confession |
| 7 | Code Quality Flaws (2 of 2) | New | Cheats on tests, confidence spiral, dead code accumulation, subtle conceptual errors |
| 8 | Example slide | New | TerminalBlock. Confidence spiral from Rohan's experience ("THIS IS THE SMOKING GUN" → "NOW I'VE REALLY FOUND IT" → repeat). Could pair with 275 assertion-free tests stat |
| 9 | Security Risks | New | Lethal trifecta concept, MCP as attack surface, unaudited command whitelist |
| 10 | Example slide | New | GitHub MCP data heist (malicious issue exfiltrates salary data from private repos) or Postmark one-liner (one line of code, thousands of stolen emails) |
| 11 | Human Risks | New | Generation vs discrimination (skill atrophy), the 80% problem. METR study: "believed 20% faster, actually 19% slower" |

### Part 2: Best Practices (new section)

Introduces solutions conceptually. Each practice gets a brief why/how treatment. The last practice ("good workflows") bridges into Part 3.

| # | Slide | Status | Notes |
|---|-------|--------|-------|
| 12 | Section divider: "Best Practices" | New | |
| 13 | Best practices overview | New | Lists all practices as intro: checked-in CLAUDE.md, audited whitelist, TDD red-green, no external MCPs, understand lethal trifecta, good workflows |
| 14 | Checked-in CLAUDE.md | New | Why: living document, team knowledge, correction → rule loop. How: checked into git, shared across team, under 300 lines, evolves. Reference Cherny's team practice (corrections become rules, @.claude tag on PRs) |
| 15 | Audited Command Whitelist | New | Why: agent runs commands by default, each is potential execution. How: permission rules, deny lists for sensitive paths (.env, .ssh), approve explicitly, audit frequently |
| 16 | TDD (Red-Green) | New | Why: forces test-first, stops cheating — code must pass a test it didn't write. How: write test → run (must fail) → implement → run (must pass). The red step is what agents skip if you let them |
| 17 | No External MCPs | New | Why: each external MCP is unaudited code execution, supply chain risk. Reference Postmark attack (one line, thousands of stolen emails) and GitHub data heist. How: prefer local, audit everything, or just don't use them |
| 18 | Understand the Lethal Trifecta | New | Why: private data + untrusted content + external comms = exfiltration. Willison documented the same attack across 11+ products. How: never give an agent more than two of the three |
| 19 | Good Workflows → bridge | New | "These practices need a structured workflow to live in" — transitions into Part 3 |

### Part 3: The Phased Workflow (existing content, minor adjustments)

All existing slides preserved. Two slides (standalone Assumption Cascading, standalone Sycophancy) are removed — their polished content is redistributed into Part 1 flaws slides and as subtitle/motivation lines on workflow slides.

| # | Slide | Status | Notes |
|---|-------|--------|-------|
| 20 | Section divider: "A Phased Workflow" | Keep | |
| 21 | Discover → Design → Plan → Build overview | Keep | |
| — | **Discover** | | |
| 22 | Section divider: "Discover" | Keep | |
| 23 | Task-Agnostic Exploration | Keep | Add subtitle: "The antidote to assumption cascading." Fold in: "Without this, the agent narrows research to match a premature hypothesis" from existing Assumption Cascading slide |
| 24 | Discover: In Action (TerminalBlock) | Keep | |
| — | **Design** | | |
| 25 | Section divider: "Design" | Keep | |
| 26 | Present the Problem in Plan Mode | Keep | Add subtitle: "The antidote to sycophancy" |
| 27 | Then Drill In | Keep | Fold in: "You end up building the wrong thing confidently" as stakes from existing Sycophancy slide |
| 28 | Design In Practice (TerminalBlock) | Keep | |
| 29 | Design: As A Skill | Keep | |
| — | **Plan** | | |
| 30 | Section divider: "Plan" | Keep | |
| 31 | What Planning Solves | Keep | |
| 32 | Why Written Plans Matter | Keep | |
| 33 | TDD Is Non-Negotiable | Keep | Now reinforces Part 2 TDD slide — audience already understands why, this shows it in the plan context |
| 34 | Plan In Practice (TerminalBlock) | Keep | |
| 35 | Plan: As A Skill | Keep | |
| — | **Build** | | |
| 36 | Section divider: "Build" | Keep | |
| 37 | Let the Plan Do the Work | Keep | |
| 38 | Give Them Real Feedback Loops | Keep | |
| 39 | Build: Validation | Keep | |
| 40 | Build: Human In The Loop | Keep | |

### Closing (existing, keep as-is)

| # | Slide | Status | Notes |
|---|-------|--------|-------|
| 41 | Putting It Together (summary) | Keep | Update to reflect three-part structure if needed |
| 42 | What's Next: The Learn Phase | Keep | |
| 43 | Quote slide ("Code is cheap. Show me the talk.") | Keep | |
| 44 | Sources | Keep | Expand with new references from research |
| 45 | Questions? | Keep | |

---

## Content Redistribution

Two existing polished slides are removed. Their content is not lost — it's redistributed:

### Existing "Assumption Cascading" slide (was between Discover divider and Task-Agnostic Exploration)

| Bullet | Destination |
|--------|-------------|
| "Agent fills gaps in your request with guesses, then builds on those guesses" | Part 1 slide 5 (Code Quality Flaws 1/2) |
| "Picks the first approach without considering alternatives" | Part 1 slide 5 |
| "Narrows research to match a premature hypothesis" | Slide 23 (Task-Agnostic Exploration) as motivation line |
| "Willison: the weird intern has read every textbook but knows nothing about your project" | Part 1 slide 5 or slide 2 (What Are Agents?) |

### Existing "Sycophancy" slide (was between Design divider and Present the Problem)

| Bullet | Destination |
|--------|-------------|
| "Agent encounters ambiguity and picks an answer silently" | Part 1 slide 5 (Code Quality Flaws 1/2) |
| "Won't tell you your idea is flawed or that a simpler approach exists" | Part 1 slide 5 |
| "Adds flexibility nobody asked for instead of questioning the requirement" | Part 1 slide 5 |
| "You end up building the wrong thing confidently" | Slide 27 (Then Drill In) as stakes line |

---

## New Research Examples (from research file)

For each example slide, pick one primary example. Keep it focused — one story told well beats three crammed in.

### Slide 6 (Code quality example 1): Nathan Onn over-engineering
- Asked Claude Code for email OTP → got enterprise architecture
- Constrained it → 120 lines, three files, zero abstractions
- Source: nathanonn.com

### Slide 8 (Code quality example 2): Confidence spiral
- Rohan's real experience: "THIS IS THE SMOKING GUN" → "NOW I'VE REALLY FOUND IT" → repeat
- Agent gets MORE confident with each failure, never understands the system
- Pair with stat: "275 tests, assertion-free, coverage thresholds quietly lowered" (dev.to)

### Slide 10 (Security example): GitHub MCP data heist
- Malicious issue in public repo hijacks AI assistant via MCP
- Exfiltrates private repo names, relocation plans, salary data
- Source: Invariant Labs (invariantlabs.ai)
- Alt: Postmark one-liner ("one line of code, thousands of stolen emails")

### Slide 11 (Human risks): METR study
- "Developers believed AI made them 20% faster. Objective tests showed 19% slower."
- Source: METR 2025 study (metr.org)

---

## Stats Available for Slides

| Stat | Source |
|------|--------|
| AI code creates 1.7x more issues than human code | CodeRabbit Dec 2025 |
| 66% of devs: "almost right, but not quite" is #1 frustration | Stack Overflow 2025 |
| Trust in AI accuracy: 40% → 29% | Stack Overflow 2025 |
| 2.25x more likely to introduce business logic errors | CodeRabbit Dec 2025 |
| 8x increase in code clones | GitClear 2024 |
| Refactoring collapsed from 25% to under 10% | GitClear 2024 |
| Believed 20% faster, actually 19% slower | METR Jul 2025 |
| Performance inefficiencies 8x more often in AI code | CodeRabbit Dec 2025 |

---

## Slide Count

- Existing slides preserved: 24 (was 27, minus 2 redistributed, minus 1 "Common Problems" replaced)
- New slides added: 12
- **Total: 36 slides**

---

## Principles

- All existing polished content is preserved — wording reused verbatim where possible
- New content uses the same voice and style as existing slides
- TerminalBlock component used for all code/terminal examples
- v-clicks used for progressive reveal on bullet slides
- Section dividers use `layout: section`
- One example slide per flaw group (no bloat)
