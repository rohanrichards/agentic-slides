# Talk Series Design: Agentic Coding Best Practices

**Date:** 2026-03-10
**Status:** Design approved

## Context

Company-backed upskilling initiative for engineering team. CEO pushing agent adoption company-wide. Delivered as community of practice series — fortnightly initially, possibly weekly later. 10-20 people per session, mostly the same audience. Mixed starting levels: some haven't touched AI tools, some tinkering, a few ahead.

## Series Arc

Each talk plants seeds for concepts expanded later. Fortnightly cadence means people have time to try what they learned between talks.

| Talk | Title | Audience State | Timing |
|------|-------|---------------|--------|
| 1 | Getting Started Right | Mixed — some at zero, some tinkering | Week 0 |
| 2 | The Pitfalls — And Your First Defenses | Everyone has 2 weeks hands-on | Week 2-4 |
| 3 | The Workflow | A month in, know the problems, have basic habits | Week 4-6 |
| 4 | Levelling Up (if we get the slot) | 6+ weeks in, using the workflow, want automation | Week 6-8 |

## Talk 1: "Getting Started Right" (~20-25 min)

### 1. Anatomy of an Agent
- What's actually happening under the hood when you run `claude`
- Architecture: system prompt → CLAUDE.md injection → conversation → tools
- Infographic showing the layers of context and where things live on disk
- Concrete example of each layer
- What makes agents different even on the same model — harness, tool use, context management matter
- File paths and config locations (~/.claude/, .claude/settings.json, CLAUDE.md hierarchy)

### 2. Why Claude Code
- Opus 4.6 on subscription — best model, best value vs API pricing
- Terminal leads IDE in features (IDE version lagging behind)
- No desktop app — lacks skills/hooks/CLI power
- Open source — can read its own source code
- Honest about gaps (needs GitHub for Cloud, IDE integration lagging)

### 3. Claude Code 101
- /init — what it does and why
- Plan mode (Shift+Tab twice) — start here for non-trivial tasks
- /clear between tasks (context contamination)
- /compact when context grows
- @ for file references
- Subagents for research
- Give it verification — the single highest-leverage tip (2-3x quality)

### 4. Set Yourself Up Right
- CLAUDE.md: run /init, prune to under 200 lines, check into git
- Work on it as a team — Cherny's practices (correction→rule loop, @.claude on PRs)
- What goes in CLAUDE.md vs CLAUDE.local.md
- Permission whitelist: deny rules for .env/.ssh/secrets, audit it
- The correction→rule loop: every mistake becomes a rule

### Actionable Takeaway
Install Claude Code, run /init, set up CLAUDE.md and whitelist, try plan mode on a real task. Come back in two weeks ready to share what you noticed.

---

## Talk 2: "The Pitfalls — And Your First Defenses" (~25-30 min)

### 1. "What Did You Notice?" (Interactive, ~10 min)
- Open the floor — people share frustrations from their first 2 weeks
- Whiteboard their experiences
- Facilitator maps responses to named patterns in real time

### 2. Name the Patterns
The flaws they'll most likely have hit:
- **Over-engineering** — class hierarchies for a simple function (Nathan Onn example)
- **Assumption cascading** — guesses, builds on guesses, never checks
- **The confidence spiral** — "THIS IS THE SMOKING GUN" × 5 (real experience example)
- **Unverified claims** — "the tests pass" without running them (Replit story)
- Back each with real examples and stats from research

### 3. Your First Defenses
Basic workflow habits to adopt immediately:
- **Explore first** — ask the agent to understand before solving. Demo how explore subagents work
- **Plan mode as workflow** — don't let it jump to code
- **Red-green TDD** — test must fail first, then code makes it pass. Stops cheating

### Actionable Takeaway
Try explore-first on your next task. Try red-green TDD. Come back ready for the full workflow.

---

## Talk 3: "The Workflow" (~25-30 min)

### 1. The Full Workflow: Discover → Design → Plan → Build
- Discover: Task-agnostic exploration (antidote to assumption cascading)
- Design: Plan mode brainstorming (antidote to sycophancy)
- Plan: Constrained, testable implementation plans with TDD baked in
- Build: Execute with verification, AI reviews AI
- TerminalBlock examples for each phase (existing polished content)
- Skill examples: brainstorming skill, writing-plans skill

### 2. Security Practices
- The lethal trifecta — concept + Willison's 11+ products documentation
- No external MCPs — Postmark attack, GitHub MCP data heist examples
- Audited whitelist in depth
- The right time: audience is now sophisticated enough to care

### 3. Best Practices in Depth
- The deeper "why" behind CLAUDE.md and TDD (building on Talks 1 and 2)
- Human risks: generation vs discrimination, METR study (believed 20% faster, actually 19% slower)
- The 80% problem

### Actionable Takeaway
Run the full Discover → Design → Plan → Build workflow on a real task end-to-end.

---

## Talk 4: "Levelling Up" (~25-30 min, if we get the slot)

### 1. Skills
- Encode workflows so the agent follows them automatically
- Project-level skills (checked into repo)
- Real skill examples with live demos

### 2. Hooks
- Guardrails that enforce practices programmatically
- Branch check, dead code eliminator, test existence check
- Hooks are scripts — you can test them

### 3. Advanced Patterns
- AI reviews AI — fresh session reviews previous session's code
- Parallel sessions (each in own git checkout)
- Spec-driven development
- Claude is great at meta tasks — use the agent to learn the agent's tooling

### 4. The Self-Improving System
- Corrections become rules
- Rules become skills
- Skills become hooks
- The system gets better through use

### Actionable Takeaway
Create your first project skill, add your first hook, try a dual-session code review.

---

## Content Mapping

| Existing asset | Talk |
|---|---|
| What Are Agents? slide (quotes) | Talk 1 (repurposed into anatomy section) |
| Claude Code 101 research | Talk 1 |
| Cherny team practices | Talk 1 |
| Code quality flaws + examples | Talk 2 |
| Real-world failure examples research | Talk 2 + Talk 3 (security) |
| Explore-first, subagent demo | Talk 2 (intro) → Talk 3 (full workflow) |
| Best practices (CLAUDE.md, whitelist, TDD) | Talk 1 (setup) → Talk 3 (depth) |
| Security risks + MCP examples | Talk 3 |
| Human risks (METR study, 80% problem) | Talk 3 |
| Phased workflow (D→D→P→B) | Talk 3 |
| Skill/hook examples | Talk 3 (intro) → Talk 4 (full) |
| What's Next: The Learn Phase | Talk 4 |
| Zero to Hero guide (Levels 2-4) | Talk 4 |
| slides.md (current restructured deck) | Talk 3 primary source |

## File Structure

```
slides.md          → current deck (preserved as-is)
talk1.md           → Getting Started Right
talk2.md           → The Pitfalls
talk3.md           → The Workflow
talk4.md           → Levelling Up
```
