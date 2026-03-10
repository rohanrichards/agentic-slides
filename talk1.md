---
theme: ./theme
title: "Getting Started Right"
info: |
  Talk 1 of the Agentic Coding Best Practices series.
  What agents are, why Claude Code, and how to set yourself up right from day one.
transition: slide-left
mdc: true
---

# Getting Started Right

Agentic Coding Best Practices — Talk 1

---
layout: section
---

# Anatomy of an Agent

What's actually happening under the hood

---

# What Is an Agent?

Not autocomplete. An autonomous tool that reads your codebase, writes code, runs commands, and iterates on the results.

<!-- TODO: Infographic showing the layers of context:
  System prompt → CLAUDE.md → Conversation history → Tool definitions → Available tools
  Show where each lives on disk -->

---

# The Context Window

<!-- TODO: Diagram/infographic showing what fills the context window and in what order:
  - System prompt (built-in)
  - CLAUDE.md (injected every session)
  - Conversation (your prompts + responses)
  - Tool calls and results
  - File contents read by the agent
  Show file paths: ~/.claude/, .claude/settings.json, CLAUDE.md hierarchy (root, parent dirs, user-level) -->

---

# Not All Agents Are Equal

The same model produces different quality in different harnesses.

<!-- TODO: What makes Claude Code's architecture good:
  - Simple main loop, flat message list
  - Dedicated tools for high-frequency actions
  - Self-managed context (compaction, subagents)
  - CLAUDE.md = human-editable persistent prompt that survives compaction
  - 50%+ of auxiliary calls go to faster/cheaper models
  Why this matters: agent architecture is the variable, not just the model -->

---
layout: section
---

# Why Claude Code

---

# Why Claude Code

<!-- TODO:
  - Opus 4.6 on subscription — best model available, best value vs API pricing (all other providers use API pricing)
  - Terminal version leads IDE in features — IDE integrations are lagging behind
  - Not the desktop app — lacks skills, hooks, CLI power
  - Open source — can read its own source code, use the agent to learn the agent
  - Developing at lightning speed — new features constantly
  - Honest about gaps: needs GitHub for Cloud, IDE integration lagging -->

---
layout: section
---

# Claude Code 101

The things I wish I knew on day one

---

# Essential Commands

<!-- TODO:
  - /init — generates starter CLAUDE.md, what it does and why to run it first
  - Plan mode (Shift+Tab twice) — read-only research and planning, start here for non-trivial tasks
  - /clear — flush context between unrelated tasks (context contamination is real)
  - /compact — compress conversation when context grows
  - @ — reference files directly in prompts
  - Subagents — isolated sub-sessions for investigation, keeps main context clean -->

---

# The Single Highest-Leverage Tip

Give Claude a way to verify its work.

<!-- TODO:
  - Anthropic internal data: verification feedback loop = 2-3x quality improvement
  - Instead of "implement email validation" → "write validateEmail, here are test cases, run them after implementing"
  - Tests, run commands, check output — the more ways it can see its own work, the better
  - This is the thing that separates good results from bad across ALL sources -->

---
layout: section
---

# Set Yourself Up Right

---

# CLAUDE.md

A shared configuration file committed to your repository.

<!-- TODO:
  - Run /init to generate starter
  - Prune ruthlessly — under 200 lines. For each line: "Would removing this cause mistakes?"
  - Check it into git — this is team infrastructure, not personal config
  - What goes in: build/test/lint commands, code style, architectural patterns, libraries to use/avoid
  - What doesn't: personal preferences (those go in CLAUDE.local.md, gitignored), rules the linter enforces -->

---

# Work On It As a Team

<!-- TODO: Cherny's team practices:
  - After every correction: "Update your CLAUDE.md so you don't make that mistake again"
  - Tag coworkers' PRs with @.claude to add learnings
  - Claude suggests CLAUDE.md improvements after sessions
  - The correction → rule loop: institutional memory that compounds
  - Commit CLAUDE.md updates alongside the code changes that prompted them -->

---

# Permission Whitelist

The agent runs shell commands by default. You decide which ones.

<!-- TODO:
  - Deny rules: .env, ~/.ssh/, credentials, secrets
  - Approve explicitly: Deny > Allow > Ask
  - Audit frequently — review as the project evolves
  - Never run with --dangerously-skip-permissions outside containers
  - This is your security baseline — set it up on day one, not after an incident -->

---

# Your First Hour Checklist

<!-- TODO:
  1. Install Claude Code
  2. cd your-project && claude
  3. Run /init
  4. Prune CLAUDE.md to under 200 lines
  5. Learn Shift+Tab (Normal → Auto-Accept → Plan Mode)
  6. Run /permissions to configure your whitelist
  7. Start in Plan Mode for your first real task
  8. After each task: /clear
  9. When Claude makes a mistake: correct it, then "Update your CLAUDE.md"
  10. Always give Claude verification: "Run the tests after implementing" -->

---

# Go Try It

Your homework for the next two weeks:

- Install Claude Code and set up your project
- Create your CLAUDE.md and check it in
- Configure your permission whitelist
- Try plan mode on a real task
- **Come back ready to share what you noticed**

---

# Sources

<!-- TODO: Sources table -->

---
layout: section
---

# Questions?

Talk 1 of the Agentic Coding Best Practices series
