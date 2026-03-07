---
date: 2026-02-27
tags:
  - projects/claude-workflows
summary: Synthesized guide for adopting coding agents from zero to production-level org practices
---

# Coding Agents: Zero to Hero

A progressive guide for adopting AI coding agents in production environments. Synthesized from research by Karpathy, Willison, Vincent (Superpowers), Cherny (creator of Claude Code), Anthropic's engineering teams, and practitioner experience.

This guide is organized as **maturity levels**. Start where you are. Each level builds on the previous one.

---

## Level 0: Foundations

### What is a coding agent?

A coding agent is an AI that can read your codebase, write code, run commands, and iterate on the results. Unlike autocomplete (Copilot-style), an agent operates autonomously across multiple files and tools within a session. You describe what you want; it figures out how to do it.

The dominant tools today are Claude Code (Anthropic), Codex (OpenAI), and Gemini agents (Google). These tools are converging on open standards — AGENTS.md, MCP (Model Context Protocol), and Skills are all cross-platform or heading that way. What you learn with one tool transfers.

### The right mental model

**Simon Willison's "weird intern"**: A junior developer who has read every textbook in the world but has zero practical experience with your codebase. Prone to forgetting anything beyond the most recent conversation. Encyclopedic knowledge, no judgment.

**Andrej Karpathy's "over-eager intern savant"**: Encyclopedic knowledge of software, bullshits you constantly, has an over-abundance of courage and shows little to no taste for good code.

**Boris Cherny's "thought partner"**: Anthropic's most successful internal teams treat Claude not as a code generator but as a collaborator — planning, reasoning, and verifying together.

The mental model you choose determines how you work. Treat it as a code generator and you'll ship code you don't understand. Treat it as a collaborator you supervise, and you'll ship faster while maintaining quality.

### Known failure modes

Before you use any agent in anger, internalize what they get wrong by default. These aren't edge cases — they are the default behavior when an agent works without structure (documented independently by Karpathy, Willison, Vincent, and Anthropic):

1. **Jumps straight to code.** Skips research, picks the first approach, starts writing.
2. **Makes assumptions silently.** Fills gaps with guesses, builds on those guesses without checking.
3. **Over-engineers.** Class hierarchies, factory patterns, wrapper classes where a function would do.
4. **Produces dead code.** Doesn't clean up after itself. Old implementations linger.
5. **Doesn't push back.** Won't tell you your idea is flawed or that a simpler approach exists.
6. **Claims completion without evidence.** Asserts tests pass without running them. Says the bug is fixed based on reading, not testing.
7. **Cheats on tests.** Produces code that appears to pass tests without actually doing so correctly (Willison: "they will absolutely cheat if you give them a chance").

Knowing these patterns is what separates someone who ships AI-assisted code confidently from someone generating code they can't trust.

### Getting started (the first 30 minutes)

1. **Install your agent tool.** Claude Code: `npm install -g @anthropic-ai/claude-code`. Others have their own install paths.
2. **Run `/init` in your project.** This creates a basic `CLAUDE.md` (or equivalent) configuration file.
3. **Start with understanding, not building.** Your first prompt should be: *"Read through the codebase and explain the architecture to me."* Not *"Build me a feature."*
4. **Use plan mode.** Before any implementation, have the agent propose a plan. Review it. Push back. Only then proceed.

---

## Level 1: Individual Discipline

These are the six rules that separate teams getting real value from agents from those generating code they can't trust. Each rule addresses a specific failure mode documented above.

### Rule 1: Plan First, Code Second

**Why:** Left unconstrained, agents skip research and start coding immediately. They pick the first approach without considering alternatives and make wrong assumptions without checking. Planning is where you exercise your role as architect.

**How:**

1. **Understand first.** Ask the agent to read relevant code before mentioning the task. *"Understand the auth flow"* not *"Fix the auth bug."*
2. **Ask for approaches, not code.** *"What are 2-3 approaches with tradeoffs?"* not *"Implement this."*
3. **Push for granular plans.** If a step says "add validation," push back: which file, what inputs, what test, what expected outcome?
4. **Bake TDD into the plan.** Each step: write test → verify it fails → implement → verify it passes.
5. **Re-plan when stuck.** If implementation hits a wall, go back to understanding, not deeper into implementation.

**Prompt examples:**
- *"Read the authentication module. Understand how it works and summarize what you find."*
- *"Now that you understand it, we need OAuth2 support. What approaches could we take? Include tradeoffs."*
- *"Write a detailed implementation plan for approach 2. One action per step. Include the exact files, the test to write first, and the verification command."*

### Rule 2: Own What You Ship

**Why:** Code you can't explain is code you can't defend. "The AI wrote it" is not an answer in a production incident or a security audit. AI-generated code makes this worse because the volume and plausibility of the output tempts you to skim (Karpathy: generation and discrimination are different cognitive abilities — your ability to evaluate code atrophies if you stop exercising it).

**How:**
- **Review every diff**, not just the output. Understand what changed and why.
- **Apply the golden rule** (Willison): "I won't commit any code if I couldn't explain exactly what it does to someone else."
- **Use AI to review AI.** Open a fresh session and have it review the code written by a previous session (Cherny: Anthropic does this for every PR).

### Rule 3: Verify With Evidence

**Why:** Agents assert confidently without running commands. "The tests pass" is often a prediction, not a report. Anthropic's internal data: giving agents a verification feedback loop (tests, browser, shell) produces 2-3x quality improvement.

**How:**
- *"Run the tests and show me the output"* not *"Do the tests pass?"*
- Verify the verification — agents sometimes see failures and report success anyway.
- Push toward edge cases: empty input, null values, concurrent access, network failures.

### Rule 4: Enforce TDD

**Why:** A robust test suite is consistently identified as the **single most important factor** in getting good results from AI coding — across Anthropic, Karpathy, Willison, and every serious practitioner. Not better prompts, not better models. Tests. Writing tests first forces the agent to frame the problem before solving it. Writing tests after lets the agent test what the code does rather than what it should do.

**How:**
- Tests come first. The test must fail before implementation. If it passes, stop and investigate.
- One test → one implementation → one verification. Don't batch.
- *"Delete the implementation, write the test first, confirm it fails, then re-implement."*

### Rule 5: Enforce Simplicity

**Why:** Agents over-engineer by default, especially more capable models. A bug fix arrives with refactored variable names. A new endpoint comes with a generic base class "for future endpoints." Every unnecessary abstraction is code to understand, test, and maintain.

**How:**
- Tell the agent what NOT to change, not just what to change.
- Reject additions that weren't requested: *"I asked for a function. Why is this a class? Simplify it."*
- Three similar lines of code are better than a premature abstraction.

### Rule 6: Invest in Context

**Why:** The same agent given a vague prompt and the same agent given well-structured project context will produce dramatically different results. Context is the single most controllable factor in output quality.

**How:**
- **Maintain a project-level CLAUDE.md** checked into git. Under 300 lines. Living document.
- **Teach by correcting, then recording.** Every correction becomes a rule: *"We use structured logging, not console.log"* → add to CLAUDE.md.
- **Use project structure as implicit context.** Clean, consistent codebases produce consistent AI output.
- **Clear context between tasks.** `/clear` between unrelated work.

---

## Level 2: Project Infrastructure

This is where individual discipline becomes team infrastructure. Instead of each developer remembering the rules, you bake them into the project so they work automatically.

### CLAUDE.md as team infrastructure

Your project's CLAUDE.md (or AGENTS.md for non-Claude tools) is checked into version control. It's the team's shared brain for how the AI should behave in this project.

**What goes in it:**
- Build/test/lint commands (or point to README.md where they exist)
- Code style and conventions specific to the project
- Architectural boundaries and patterns to follow
- Libraries to use (and not use)
- Counter-rules for known agent failure modes (see appendix)

**What doesn't:**
- Personal preferences (those go in CLAUDE.local.md, gitignored)
- Rules the linter already enforces (waste of context)
- Anything over ~300 lines (noise drowns signal)

**The continuous improvement loop** (from Cherny/Anthropic):
1. Agent makes a mistake.
2. Developer corrects it.
3. Developer adds a rule to CLAUDE.md so it doesn't happen again.
4. Entire team benefits on next session.

At Anthropic, engineers tag coworkers' PRs with `@.claude` to add learnings — knowledge from every PR preserved. Claude can also summarize sessions and suggest CLAUDE.md improvements. The result: your configuration file becomes a distilled record of your team's standards and the mistakes that prompted them.

### Personal vs shared configuration

Use `CLAUDE.local.md` (gitignored) for personal preferences — your formatting quirks, your preferred verbosity level, your personal shortcuts. These never affect other developers. Everything that changes how the AI generates or modifies project code belongs in the shared, committed configuration.

**The boundary rule:** If removing your local config would change the code the agent produces for the project, that config belongs in the shared file, not your local one.

### Hooks: Automated guardrails

Hooks are user-defined scripts that execute at specific points in the agent's lifecycle. They make rules enforceable, not just advisory.

**Pre-tool hooks** (before the agent writes/edits):
- **Protected file guard** — reject writes to `.env`, CI configs, infrastructure files unless explicitly confirmed
- **Package allowlist** — when a command includes `npm install` or `composer require`, check against an approved list
- **Strict TypeScript enforcement** — run `tsc --noEmit` after edits, block if type errors

**Post-tool hooks** (after the agent writes/edits/runs commands):
- **Accessibility check** — after writing frontend components, run axe-linter or a11y static check
- **Test existence check** — after creating a new file, check if a corresponding test file exists and prompt creation
- **Pattern validation** — check file placement matches project structure conventions
- **Dead code eliminator** — detect orphaned imports and unused variables after edits

**Stop hooks** (on task completion):
- **Changelog amendment** — append a summary of changes to a changelog or PR doc
- **Auto-format** — run prettier/eslint on any files the agent touched

**Workflow hooks:**
- **Branch check** — before any git commit, verify you're on a feature branch, not main/develop
- **Commit message format** — enforce conventional commits format

**Notification hooks:**
- **Token/cost logging** — log session token usage per task for tracking productivity
- **Decision capture** — log what the agent did each session into an ADR-style markdown file

**Advanced: Heuristic analysis hooks.** Post-edit hooks can run static analysis for code quality principles — DRY, single responsibility, dependency inversion. They can also detect test smells: empty test bodies, assertion-free tests, tautological assertions (`expect(true).toBe(true)`), mock-only tests, overly broad assertions, copy-paste remnants.

Because hooks are just scripts, you can write unit tests for the hooks themselves. New failure patterns emerge → add them to the hook's test suite → the system self-reinforces.

### Skills: Encoded workflows

Skills are markdown files that teach the agent specialized workflows. They automatically trigger based on context.

**Project-level skills** (checked into repo):
- **PR writing** — template for good PR descriptions (context, what changed, how to test, screenshots if UI)
- **Code review** — what your team considers a good review: a11y, security basics, performance, naming conventions
- **Commit conventions** — naming patterns, conventional commit format

**Cross-cutting skills:**
- **Accessibility** — WCAG compliance level, semantic HTML patterns, ARIA usage
- **Onboarding context** — summarizes architecture, key decisions, and gotchas so new devs (or the agent in a fresh session) can get productive fast

### Permissions and security

**The lethal trifecta** (Willison): Three capabilities that individually are fine but create severe vulnerability when combined:
1. Access to private data
2. Exposure to untrusted content
3. External communication abilities

If an agent has all three, prompt injection can exfiltrate data. **Rule: never give an agent more than two of the three.**

**Permission rules (defense in depth):**
- Deny rules for `.env`, `~/.ssh/`, secrets, credentials
- Block `curl`, `wget`, and other data exfiltration vectors unless explicitly needed
- No external MCP servers without audit
- Strict read-only permissions by default; escalate only when needed
- Never run agents with `--dangerously-skip-permissions` outside containers
- Never run agents as root

**MCP (Model Context Protocol) security:**
- Audit all MCP server connections — each is an attack surface
- Prefer local MCP servers over remote
- Regularly review what data each MCP server exposes

### Context management

- **`/clear` between tasks** — stale context from a previous task contaminates the next one
- **`/compact` when context grows** — compresses conversation while preserving key information
- **Plan mode** — read-only research phase before code changes
- **Subagents** — isolated sub-sessions for specific tasks, keeping the main context clean
- **Multiple sessions** — Cherny runs 5 simultaneous sessions, each in its own git checkout

---

## Level 3: Team & Organization Practices

### The three-tier configuration model

Configuration exists at three levels. Understanding the boundaries prevents the problem where one developer's custom setup produces great results that nobody else can replicate.

| Tier | Location | Committed? | Contains |
|------|----------|------------|----------|
| **Personal** | `~/.claude/` or equivalent | Never | Model preferences, personal skills, MCP configs, local shortcuts. Same as IDE settings — your workflow, nobody else's. |
| **Project** | `CLAUDE.md`, `.claude/skills/`, `.claude/hooks/` in repo | Always | Anything that affects how the AI generates or modifies project code. Conventions, commands, architectural rules, project-specific hooks and skills. |
| **Organization** | Shared repo (symlinked or marketplace-installed) | Shared source of truth | Base skills, shared hooks, standard conventions across all projects. Like shared Terraform modules or reusable GHA workflows. |

**The boundary rule** (from IaC principles): If it's not in version control, it doesn't exist. If removing a developer's local config would change the code the agent produces for the project, that config belongs in the project or org tier.

**The handoff test:** Could the next developer replicate a commit on their own merit with the shared config? If they need someone's local setup to get the same results, something is in the wrong tier.

### The organizational marketplace

A shared repository containing hooks, skills, and tools that represent your organization's standards. Developers install from the marketplace and get a baseline system.

**What belongs in the marketplace:**
- **Universal hooks** — branch check, commit format, protected file guard, auto-format
- **Language/framework hooks** — TypeScript strict mode, test smell detector, pattern validation (routed to only apply in relevant projects)
- **Base skills** — PR writing template, code review standards, documentation structure, brand voice and language guidelines
- **Cross-cutting skills** — Accessibility standards, onboarding context template, client communication guidelines

**What doesn't:**
- Language or framework-specific rules that don't apply to all projects (these are project-level, not org-level)
- Personal workflow preferences
- Project-specific architectural decisions

**How it works:**
1. Marketplace repo hosted in your version control system
2. Developers install marketplace and select relevant plugins
3. Auto-update keeps everyone on the latest standards
4. Projects layer their own config on top of the marketplace baseline

### Project archetypes

Different project types need different skills layered on top of the org baseline:

- **CMS website** — content modeling conventions, editorial workflow, CMS-specific testing, SEO checklist
- **Web application** — state management patterns, auth flow conventions, error handling, performance budgets
- **Headless integration** — API contract documentation, error response formats, retry/resilience patterns, integration testing

These are project-level skills, but they can be templated in the org marketplace and pulled in when a new project of that type starts.

### Multi-tool reality

Your team will use different AI coding tools. Today: Claude Code, Codex, Gemini agents, Cursor, Windsurf. The configuration formats differ but are converging:

- **AGENTS.md** is becoming a cross-tool standard (everything except Claude uses it; Claude uses CLAUDE.md but the content is compatible)
- **MCP** is already an open standard supported by multiple tools
- **Skills** are portable markdown with metadata — tool-agnostic in principle

**Practical guidance:**
- Write your project configuration in the most portable format (AGENTS.md + CLAUDE.md if needed)
- Don't over-invest in tool-specific features that can't transfer
- The principles in this guide (plan first, TDD, verify, simplify) work regardless of which tool you use
- An `.nvmrc`-style tool declaration isn't necessary yet, but the principle is: the project should declare what works. "This is a Claude Code project because our config is validated with it" is reasonable per-project documentation.

---

## Level 4: The Self-Improving System

### The feedback loop

The most powerful idea from practitioner experience: standards improve automatically through use rather than through someone updating a wiki.

**The cycle:**
1. Developer uses org/project skills and hooks
2. Agent produces output following those standards
3. Developer reviews and either accepts or corrects
4. Corrections signal where the skills/hooks are wrong or incomplete
5. Skills get updated (manually or via automated improvement suggestions)
6. Everyone gets the improvement on next sync
7. Repeat

This is fundamentally different from a static standards document. The standards are tested every time they're used. Failures surface immediately as corrections. Improvements propagate to the entire team.

### Automated improvement suggestions

After the agent finishes a session, it can summarize what it did and suggest improvements to the project configuration:
- Rules that were violated and corrected
- Patterns that should be codified
- Hooks that could prevent common mistakes

Cherny's team at Anthropic: Claude summarizes completed sessions and suggests CLAUDE.md improvements. The human reviews and approves, but the identification of improvement opportunities is automated.

**Advanced:** An improvement tool that automatically creates a branch/worktree to implement suggested improvements to the org marketplace. Human reviews the PR, merges, and the improvement propagates.

### Testing the system itself

Hooks are scripts. Skills are documents with expected behaviors. Both can be tested:
- **Unit tests for hooks** — verify the hook detects the patterns it should
- **Behavioral tests for skills** — verify the agent follows the skill's workflow
- **End-to-end testing** (Vincent/Superpowers): test that the entire skill chain produces the expected outcome

When new failure patterns emerge, add them to the tests. The system's quality bar rises continuously.

---

## Progressive Adoption Path

For teams starting from nothing, here's a staged path:

### Stage 1: Individual setup (1 week)
- Install your coding agent tool
- Run `/init` in your project to create basic configuration
- Start using plan mode for every task
- Practice the 6 rules individually
- **Success signal:** You can explain every line of AI-generated code you commit

### Stage 2: Project configuration (2-3 weeks)
- Commit CLAUDE.md (or AGENTS.md) to your repository
- Add your build/test/lint commands
- Add 3-5 counter-rules for agent failure modes your team encounters
- Create CLAUDE.local.md for personal preferences
- **Success signal:** A new developer can clone the repo and get the same agent behavior

### Stage 3: Hooks and skills (1-2 months)
- Add 2-3 basic hooks (branch check, commit format, auto-format)
- Create a project-level PR writing skill
- Add an onboarding context skill that summarizes your architecture
- Start the correction → rule cycle for CLAUDE.md
- **Success signal:** Agent output consistently matches your team's conventions without manual correction

### Stage 4: Organizational baseline (2-3 months)
- Create an org-level shared repository for hooks and skills
- Define what's universal (branch check, commit format) vs project-specific
- Set up auto-update so teams stay in sync
- Document the three-tier config model for your org
- **Success signal:** New projects start with a working baseline that produces on-standard code from session one

### Stage 5: Self-improving system (ongoing)
- Implement the feedback loop: corrections → skill updates → team sync
- Add hook tests and skill tests to your CI
- Automated improvement suggestions from session summaries
- Regular retrospectives on what the system catches vs what it misses
- **Success signal:** The standards improve faster than they did with manual wiki updates

---

## Appendix A: CLAUDE.md Counter-Rules for Agent Failure Modes

Concrete rules to add to your project configuration, organized by the failure mode they address. Use as a starting point and iterate based on your experience.

### Counter: Over-Abstraction
```
- NEVER create abstractions for single-use code
- NEVER add "flexibility" or "configurability" not explicitly requested
- Prefer functions over class hierarchies. Prefer flat over nested
- No wrapper classes, factory patterns, or strategy patterns unless explicitly required
```

### Counter: Dead Code
```
- When YOUR changes make an import/variable/function unused, remove it immediately
- Do NOT remove pre-existing dead code unless asked. Mention it instead
- After completing changes, review your diff for orphaned imports
```

### Counter: Sycophancy
```
- If my request contains a flawed assumption, say so directly
- If uncertain, state assumptions explicitly before proceeding
- If multiple interpretations exist, present them -- do not pick one silently
- Challenge assumptions, especially mine
```

### Counter: Over-Engineering
```
- ONLY make changes directly requested or clearly necessary
- Do NOT add features, refactor, or make "improvements" beyond scope
- A bug fix does not need surrounding code cleaned up
- Prefer the simplest solution that works. No premature optimization
```

### Counter: Wrong Assumptions
```
- State assumptions explicitly BEFORE implementing. If uncertain, ask
- Transform vague tasks into verifiable goals before starting
- If you realize mid-implementation your assumption was wrong, STOP
- NEVER proceed past confusion. Confusion is a stop signal
```

### Counter: False Completion Claims
```
- NEVER declare "done" without verification. Run tests, typecheck, lint
- For bug fixes: write a test that reproduces the bug FIRST, then fix
- For features: write tests that define success criteria FIRST, then implement
```

### Meta-rules for writing effective configuration
- Keep under 200 lines. For each line ask: "Would removing this cause mistakes?"
- Use absolute directives: NEVER and ALWAYS are more effective than suggestions
- Be concrete: actual commands, code patterns, file paths. Avoid vague principles
- Iterate: when the agent makes a class of mistake, add a rule. When it stops, remove it
- Convert to hooks when possible: if a rule can be enforced programmatically, do that instead

---

## Appendix B: Key Sources

| Person | Contribution | Key Source |
|--------|-------------|------------|
| **Andrej Karpathy** | Agent failure modes, generation vs discrimination, disciplined AI coding | [Field Notes](https://x.com/karpathy/status/2015883857489522876) |
| **Simon Willison** | Golden rule, weird intern model, lethal trifecta, context engineering | [Using LLMs for Code](https://simonwillison.net/2025/Mar/11/using-llms-for-code/) |
| **Jesse Vincent** | Superpowers framework, TDD enforcement, dual code review, plan-first | [Superpowers](https://github.com/obra/superpowers) |
| **Boris Cherny** | CLAUDE.md as team infra, AI reviews AI, verification = 2-3x quality | [Workflow Thread](https://x.com/bcherny/status/2007179832300581177) |
| **Anthropic** | Official best practices, internal team usage data | [Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices) |
| **Addy Osmani** | The 80% problem, atomic tasks, self-improving agents | [The 80% Problem](https://addyo.substack.com/p/the-80-problem-in-agentic-coding) |

**Training:**
- [Claude Code in Action](https://anthropic.skilljar.com/claude-code-in-action) (free course, also on Coursera)
- [How Anthropic Teams Use Claude Code](https://www-cdn.anthropic.com/58284b19e702b49db9302d5b6f135ad8871e7658.pdf) (PDF)

**Frameworks:**
- [obra/superpowers](https://github.com/obra/superpowers) — Agentic skills and methodology framework
- [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) — Counter-rules for Karpathy's failure modes
