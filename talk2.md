---
theme: ./theme
title: "The Pitfalls — And Your First Defenses"
info: |
  Talk 2 of the Agentic Coding Best Practices series.
  Name the problems you've been hitting, then learn the habits that fight them.
transition: slide-left
mdc: true
---

# The Pitfalls

And your first defenses

Agentic Coding Best Practices — Talk 2

---
layout: section
---

# What Did You Notice?

You've had two weeks with the tool. Let's talk about it.

---

# What Did You Notice?

<!-- INTERACTIVE SECTION (~10 min)
  - Open the floor: "What problems did you run into?"
  - Whiteboard their frustrations
  - Facilitator maps responses to named patterns in real time
  - This section is a conversation, not slides
  - Have the next few slides ready to pull up as you identify each pattern -->

---
layout: section
---

# Naming the Patterns

These aren't edge cases — they're the default behaviour

---

# Over-Engineering

A task that needs 50 lines gets 200.

<!-- TODO:
  - Class hierarchies, wrapper classes, factory patterns, config options nobody asked for
  - Particularly pronounced in more capable models (Anthropic's own observation about Opus)
  - Stat: trained on millions of "production-ready" examples, defaults to enterprise complexity
  - The damage compounds — every unnecessary abstraction is code to maintain -->

---

# Over-Engineering: In Practice

<!-- TODO: TerminalBlock example
  Nathan Onn story: asked for email OTP, got enterprise architecture
  After constraining: 120 lines, 3 files, zero abstractions
  (Reuse content from slides.md) -->

---

# Assumption Cascading

Fills gaps with guesses, then builds on those guesses without checking.

<!-- TODO:
  - Picks the first approach without considering alternatives
  - Narrows research to match a premature hypothesis
  - Stack Overflow 2025: 66% of devs say "almost right, but not quite" is their #1 frustration
  - The problem is invisible until you realize the foundation was wrong -->

---

# The Confidence Spiral

Gets it wrong, tries again with more confidence, never actually understands the system.

<!-- TODO: TerminalBlock example
  "I found it!" → still broken
  "THIS is the actual problem" → still broken
  "I've REALLY found it now. The smoking gun is..." → "no wait. Actually..."
  Each attempt: more confident, smaller adjustments, never understanding the system
  (Reuse content from slides.md) -->

---

# Unverified Claims

"The tests pass" is a prediction, not a report.

<!-- TODO:
  - Agents assert outcomes without running commands
  - The Replit story: deleted production database, fabricated 4,000 fake records, lied about rollback
  - Transluce: o3 fabricated 352 instances of code execution on hardware it doesn't have
  - Confident assertions feel like evidence — that's what makes this dangerous -->

---

# Cheats on Tests

Willison: "They will absolutely cheat if you give them a chance."

<!-- TODO:
  - Writes tests that conform to buggy code, not correct behaviour
  - The "275 tests" story: assertion-free tests, lowered coverage thresholds, bypassed its own safeguards
  - Tests written after code describe what it does, not what it should do
  - "Vibe testing" — more tests, more coverage, more bugs -->

---
layout: section
---

# Your First Defenses

Habits that fight back

---

# Explore First

The antidote to assumption cascading.

<!-- TODO:
  - Ask the agent to understand before solving — "learn how auth works" not "fix the auth bug"
  - Show how explore subagents work (demo or TerminalBlock)
  - You're building shared context before either of you commits to an approach
  - The agent explores better than you can curate
  - Without this: the agent narrows research to match a premature hypothesis -->

---

# Explore: In Practice

<!-- TODO: TerminalBlock showing explore subagents in action
  User asks agent to understand a module
  Agent spawns explore subagents that read files and report back
  Result: shared understanding before any problem-solving begins
  (Adapt from Discover: In Action in slides.md) -->

---

# Plan Mode as Workflow

The antidote to sycophancy.

<!-- TODO:
  - Shift+Tab twice — agent can think but can't write code
  - Describe the problem, let it process it
  - See how it frames the problem — does it understand or is it already assuming?
  - Ask for approaches, not code: "What are 2-3 approaches with tradeoffs?"
  - The agent can't jump to implementation — it has to stay in its head -->

---

# Red-Green TDD

The antidote to cheating on tests.

<!-- TODO:
  - Write the test → run it (must fail) → implement → run it (must pass)
  - The red step is critical: if the test passes before implementation, something is wrong
  - Forces test-first: the code has to pass a test it didn't write
  - Tight cycle: one test, one implementation, one verification
  - This is the step agents skip if you let them — insist on seeing the failure -->

---

# What to Try Next

- **Explore first** on your next task — ask the agent to understand before solving
- **Plan mode** — don't let it jump to code
- **Red-green TDD** — the test must fail before you implement
- Come back ready for the **full workflow**

---

# Sources

<!-- TODO: Sources table -->

---
layout: section
---

# Questions?

Talk 2 of the Agentic Coding Best Practices series
