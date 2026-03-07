---
theme: ./theme
title: "Agentic Coding: Best Practices for Code Quality"
info: |
  Best practices for improving code quality when working with AI coding agents.
transition: slide-left
mdc: true
---

# Agentic Coding

Best practices for improving code quality

---
layout: two-cols
---

# What Are Agents?

::left::

<v-clicks>

**Simon Willison:**

> "A weird intern — encyclopedic knowledge, zero practical experience with your codebase"

**Andrej Karpathy:**

> "An over-eager intern savant — bullshits you constantly, shows no taste for good code"

</v-clicks>

::right::

<v-clicks>

**Boris Cherny** (creator of Claude Code)**:**

> "A thought partner — but only if you treat it that way"

</v-clicks>

---

# What Does Everyone Agree On?

I researched what the most vocal practitioners are saying — Karpathy, Willison, Anthropic's own teams, the creator of Claude Code — all working independently.

<v-clicks>

- They're hitting the **same problems**
- They're arriving at the **same solutions**
- The patterns are consistent enough to build a practical framework around

</v-clicks>

---

# The Common Problems

<v-clicks>

- **Assumption cascading** — they guess instead of asking, then build on those guesses
- **Sycophancy** — they agree with everything, don't push back, don't surface alternatives
- **Over-engineering** — class hierarchies, wrapper classes, config options nobody asked for
- **Unverified claims** — "the tests pass" is a prediction, not a report

</v-clicks>

---
layout: section
---

# A Phased Workflow

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

# Assumption Cascading

Karpathy's #5. Independently confirmed by Willison, Anthropic, Cherny.

<v-clicks>

- Agent fills gaps in your request with **guesses**, then builds on those guesses
- Picks the **first approach** without considering alternatives
- Narrows research to match a **premature hypothesis**
- Willison: the "weird intern" has read every textbook but knows nothing about **your** project

</v-clicks>

---

# Task-Agnostic Exploration

Separate understanding from tasking. Don't mention the problem until you both understand the area.

<v-clicks>

- No problem solving yet — just learning
- Strip the problem from the prompt — just ask the agent to **explore**
- You're building **shared context** before either of you commits to an approach
- Early days: you had to `@file.ts` every relevant file manually to build context
- Now: just talk to it — "learn how auth works" and it finds what it needs
- The agent explores better than you can curate

</v-clicks>

---

# Discover: In Action

<TerminalBlock title="claude-code">

```
> Learn how authentication works in this project

⠋ Exploring codebase...
  ↳ Agent: src/middleware/auth.ts
  ↳ Agent: src/services/session.ts
  ↳ Agent: src/routes/login.ts
  ↳ Agent: src/utils/jwt.ts

Authentication uses JWT tokens issued on login via
/api/auth/login. Tokens are verified in auth middleware
on every protected route. Sessions are stored in Redis
with a 24h TTL. Refresh tokens rotate on each use.

The middleware checks three things:
1. Token exists in Authorization header
2. Token signature is valid (jwt.verify)
3. Session exists in Redis and isn't expired
```

</TerminalBlock>

You now both have shared context.

---
layout: section
---

# Design

Think through the approach together

---

# Sycophancy

Karpathy's #4. "Blindly follows wrong assumptions, doesn't push back, doesn't surface inconsistencies."

<v-clicks>

- Agent encounters ambiguity and **picks an answer silently**
- Won't tell you your idea is flawed or that a simpler approach exists
- Adds "flexibility" nobody asked for instead of questioning the requirement
- You end up building the wrong thing confidently

</v-clicks>

---

# Present the Problem in Plan Mode

Start in plan mode — the agent can think but can't write any code yet.

<v-clicks>

- Describe the feature or problem. Let the agent process it.
- Plan mode means it **can't jump to implementation** — it has to stay in its head
- See what it gives back: how it frames the problem, what approach it suggests
- This is your first signal — does it understand the problem or is it already making assumptions?

</v-clicks>

---

# Then Drill In

Once you've both got a picture of the problem, start the brainstorm.

<v-clicks>

- "What are you uncertain about?"
- "What edge cases should we think about?"
- "What questions do you have for me?"
- "What's the simplest version of this that works?"
- "What could we change about this approach?"
- Make the agent a **thinking partner**, not a yes-man

</v-clicks>

---

# Design In Practice

<v-clicks>

- Hash out **scope** — what's in, what's explicitly out
- Surface **edge cases** before they become surprises
- Tell it what **NOT** to build: "No config options. No base classes. Just the function."
- Output: a **design doc** that captures scope, decisions, and constraints
- This is where the agent earns Cherny's "thought partner" label

</v-clicks>

---

# Design: In Action

<TerminalBlock title="claude-code">

```
> I think we've got a good idea how to build this. Before we get
  into it, do you have any questions for me?

A few things I'd like to clarify:

1. Which providers? Just Google, or also
   GitHub/Microsoft?
2. Should OAuth accounts link to existing
   email/password users, or stay separate?
3. Current session TTL is 24h — should OAuth
   sessions match that?
```

</TerminalBlock>

---

# Design: As A Skill

You can encode this workflow so the agent does it automatically.

```markdown
# Brainstorming Skill

## Process
1. Explore project context — check files, docs, recent commits
2. Ask clarifying questions — one at a time, not all at once
3. Propose 2-3 approaches — with trade-offs and a recommendation
4. Present design — get user approval before any code
5. Write design doc — save to docs/plans/

## Key Rules
- One question at a time. Don't overwhelm.
- YAGNI ruthlessly. Remove unnecessary features.
- Always explore alternatives before settling.
- Do NOT write any code until the design is approved.
```

---
layout: section
---

# Plan

Write a constrained, testable implementation plan

---

# What Planning Solves

<v-clicks>

- **Over-engineering** — Karpathy's #2 and #6. Anthropic: "Opus models specifically tend to over-engineer"
- **Scope creep** — without a plan, every file the agent touches gets "improved"
- **The confidence spiral** — agent jumps to fixing, gets it wrong, tries again with more confidence, never gets anywhere
- A constrained plan means: if it's not in the plan, it doesn't get built

</v-clicks>

---

# Why Written Plans Matter

<v-clicks>

- Sessions crash. Context compacts. Conversations get lost.
- A plan in a document is **durable** — it survives all of that
- Claude Code's plan mode saves plans to the working folder automatically
- A well-written plan lets you **one-shot most work** — hand it to a fresh session and go
- Plans are also **reviewable** — you catch bad ideas before they become bad code

</v-clicks>

---

# TDD Is Non-Negotiable

Cherny: test suite is the "single most important factor" in AI code quality.

<v-clicks>

- Every step in the plan: **write test &rarr; watch it fail &rarr; implement &rarr; watch it pass**
- Red-green is mandatory. If the test passes before implementation, something is wrong.
- Tests written **after** code describe what the code does, not what it **should** do
- Without red-green, the agent writes tests that confirm bugs

</v-clicks>

---

# Plan In Practice

<TerminalBlock title="claude-code">

```
> Write a detailed implementation plan based on our design.

  Rules for the plan:
  - Break work into small, atomic chunks
  - Each chunk gets its own commit
  - Every chunk follows red-green TDD:
    write test, watch it fail, implement, watch it pass
  - YAGNI: nothing beyond what the design specifies
  - DRY: reuse existing patterns in the codebase
  - One action per step. If a step has "and", split it.
```

</TerminalBlock>

---

# Plan: As A Skill

Write plans assuming the engineer has **zero context** and **questionable taste**.

```markdown
# Writing Plans Skill

## Each step is one action (2-5 minutes):
- "Write the failing test"           — step
- "Run it to confirm it fails"       — step
- "Implement minimal code to pass"   — step
- "Run tests to confirm they pass"   — step
- "Commit"                           — step

## Every task must include:
- Exact file paths (create, modify, test)
- Complete code — not "add validation"
- Exact commands with expected output
- DRY, YAGNI, TDD, frequent commits
```

---
layout: section
---

# Build

Execute the plan

---

# Let the Plan Do the Work

If the plan is well-structured, the Build phase is the simplest.

<v-clicks>

- The agent follows the plan — red-green TDD is already baked into every step
- You can let it run mostly autonomously and **watch the tests go green**
- Subagents can churn through tasks in parallel
- Your job shifts from directing to **reviewing and validating**

</v-clicks>

---

# Give Them Real Feedback Loops

The emerging trick: let agents **actually use** what they build.

<v-clicks>

- Built a backend endpoint? Have them **curl it and check the DB**
- Built a form? Have them **open a browser and fill it out**
- Built an API integration? Have them **call it and inspect the response**
- Anthropic: a verification feedback loop = **2-3x quality improvement**
- The more ways they can **see their own work in action**, the better the output

</v-clicks>

---

# Build: Validation

<v-clicks>

- Unit tests catch logic errors — but they're not the whole picture
- Push for **end-to-end validation**: does the feature actually work in context?
- Manual verification: "Go test this yourself. Show me what happened."
- AI reviews AI: open a **fresh session** to review code from the previous one
- The goal: **the agent proves it works**, not just claims it does

</v-clicks>

---

# Build: Human In The Loop

This is where you get your hands dirty.

<v-clicks>

- **Use the thing.** Open it, click around, fill out the form, hit the endpoint yourself.
- **Review the code.** Read the diffs. Understand what changed and why.
- **Review the commits.** Are they atomic? Do the messages make sense?
- **QA it.** Try the edge cases. Break it on purpose.
- Iteration is expected — find issues, send the agent back, repeat
- Willison's golden rule: "I won't commit code I couldn't explain to someone else"

</v-clicks>

---
layout: section
---

# Putting It Together

---

# Discover &rarr; Design &rarr; Plan &rarr; Build

<v-clicks>

- **Discover** — Learn how things work before mentioning the task. No assumptions, just context.
- **Design** — Brainstorm the approach together. Surface unknowns, constrain scope, challenge ideas.
- **Plan** — One action per step. TDD baked in. Dependencies mapped. No room for extras.
- **Build** — Red-green TDD. Every claim backed by command output. AI reviews AI.

</v-clicks>

---

# What's Next: The Learn Phase

<v-clicks>

- This talk covered **individual practices** — how you work with an agent
- The next level: **project infrastructure** that makes this automatic
- CLAUDE.md — persistent context that teaches the agent your standards
- Skills — encoded workflows that trigger automatically
- Hooks — guardrails that enforce the phases programmatically
- Corrections become rules, rules become enforcement

</v-clicks>

---
layout: quote
---

> Code is cheap. Show me the talk.
>
> — Faiq

---

# Sources

| Expert | Key Insight |
|--------|------------|
| **Andrej Karpathy** | 7 failure modes, generation vs discrimination |
| **Simon Willison** | Weird intern model, golden rule, "they will cheat", "lethal trifector" |
| **Boris Cherny** | Tests = most important factor, AI reviews AI |
| **Jesse Vincent** | TDD enforcement, plan-first workflow, "superpowers" skills marketplace |
| **Anthropic** | Verification = 2-3x quality (internal data) |
| **Addy Osmani** | The 80% problem |

---
layout: section
---

# Questions?

Built with Slidev + Claude
