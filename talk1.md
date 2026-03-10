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

<v-clicks>

- It has **tools** — file reads, edits, bash commands, web search, subagents
- It calls those tools in a loop — act, observe, decide, act again
- It manages its own context — decides what to read, what to compact, when to delegate
- Left unsupervised, it will **confidently build the wrong thing**

</v-clicks>

---

# The Context Window

Everything the agent knows lives in one context window. Here's what fills it:

<v-clicks>

- **System prompt** — built-in instructions from Anthropic (~2,800 tokens)
- **Tool definitions** — every tool the agent can call (~9,400 tokens)
- **CLAUDE.md** — your project rules, injected every session
- **Conversation** — your prompts + its responses + tool calls + file contents
- Performance **degrades as context fills** — Claude literally gets worse the more it's holding

</v-clicks>

---

# Where Things Live on Disk

<v-clicks>

- `~/.claude/CLAUDE.md` — your personal preferences (just you, all projects)
- `./CLAUDE.md` — project root, checked into git (shared with team)
- `.claude/settings.json` — permission rules, MCP configs
- `.claude/commands/` — custom slash commands
- Parent/child directories — monorepo support, loaded on demand

</v-clicks>

---

# Not All Agents Are Equal

The same model produces different quality in different harnesses.

<v-clicks>

- **Architecture is the variable**, not just the model — same model, different harness, different quality
- Claude Code: one main loop, flat message list, no multi-agent orchestration
- **Dedicated tools** for high-frequency actions — reduces decision complexity for the model
- Self-managed context — compaction, subagents, todo lists
- CLAUDE.md = human-editable persistent prompt that **survives compaction**
- 50%+ of auxiliary calls go to faster/cheaper models — keeps costs down

</v-clicks>

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
