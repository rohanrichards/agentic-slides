---
date: 2026-02-10
tags:
  - projects/claude-workflows
summary: Compiled research for Claude Code Best Practices workshop
---

*Research compiled 2026-02-10 from multiple sources for workshop planning.*

---

## Source 1: Andrej Karpathy

### Key Concepts

**Vibe Coding** (coined Feb 2025): Building software with an LLM without reviewing the code it writes. "Fully give in to the vibes, embrace exponentials, and forget that the code even exists." Valid for prototypes and throwaway projects, not production code.

**Agentic Engineering** (Feb 2026): The successor to vibe coding. Agents write 99% of code while the human supervises. The human role becomes strategic -- setting goals, reviewing results, guiding decisions.

**The 80/20 Flip** (Jan 2026): "I rapidly went from about 80% manual+autocomplete coding and 20% agents in November to 80% agent coding and 20% edits+touchups in December. I really am mostly programming in English now."

**Generation vs Discrimination**: "Being good at reviewing code doesn't mean you can still write it yourself, since generation and discrimination are different abilities in the brain." Manual coding ability atrophies with heavy AI delegation. LLMs made generation instant but did nothing to help discrimination.

**The Generation-Verification Loop**: Core design principle -- maximize the speed of the human-AI generate-verify loop. The AI generates; the human verifies and corrects.

**Iron Man Suit, Not Iron Man Robot**: Build tools that augment human capabilities with an "autonomy slider," not fully autonomous agents. Partial autonomy, not full autonomy.

**The Slopacolypse**: Prediction that 2026 will see masses of "almost right, but not quite" AI-generated code flooding GitHub, arXiv, and digital media. Will dramatically increase the cost of information filtering.

### Known Agent Failure Modes (Karpathy's List)

1. **Subtle conceptual errors** -- not syntax, but "the kind a slightly sloppy, hasty junior dev might do"
2. **Over-abstraction** -- bloated abstractions, overcomplicated APIs, elaborate class hierarchies where a function would do
3. **Dead code accumulation** -- doesn't clean up after itself, old implementations linger
4. **Sycophancy** -- blindly follows wrong assumptions, doesn't push back, doesn't surface inconsistencies
5. **Assumption cascading** -- makes wrong assumptions on your behalf and runs with them without checking
6. **Over-engineering** -- excessive try/catch, nested conditionals, wrapper classes, factory patterns where simple code would suffice
7. **No taste** -- "an over-eager junior intern savant with encyclopedic knowledge of software, but who also bullshits you all the time, has an over-abundance of courage and shows little to no taste for good code"

### Karpathy's Disciplined Workflow (for code he professionally cares about)

1. Stuff everything relevant into context
2. Describe the next single, concrete incremental change -- ask for approaches and tradeoffs first, NOT code
3. Pick one approach, ask for first draft code
4. Review/learning phase -- pull up API docs, ask for explanations, verify

Key posture: "Slow, defensive, careful, paranoid, and always taking the inline learning opportunity."

### Sources
- Karpathy's Claude Code Field Notes: https://x.com/karpathy/status/2015883857489522876
- 2025 LLM Year in Review: https://karpathy.bearblog.dev/year-in-review-2025/
- Disciplined AI coding rhythm: https://x.com/karpathy/status/1915581920022585597
- The 80% Problem in Agentic Coding (Addy Osmani): https://addyo.substack.com/p/the-80-problem-in-agentic-coding

---

## Source 2: Simon Willison

### Key Concepts

**The "Weird Intern" Mental Model**: "A junior developer that has read every textbook in the world but has zero practical experience with your codebase, and is prone to forgetting anything but the most recent hour of things you've told it." Give it "sticky notes" (context), but periodically clear irrelevant ones before they overwhelm.

**The Golden Rule**: "I won't commit any code to my repository if I couldn't explain exactly what it does to somebody else." The origin of the code doesn't matter; your understanding of it does.

**Vibe Coding vs Vibe Engineering**: Willison coined "vibe engineering" (Oct 2025) as the disciplined opposite of vibe coding. "You're researching approaches, deciding on high-level architecture, writing specifications, defining success criteria, designing agentic loops, planning QA, managing a growing army of weird digital interns who will absolutely cheat if you give them a chance, and spending so much time on code review."

**Context Engineering** (not prompt engineering): "The delicate art and science of filling the context window with just the right information for the next step." The core skill of AI-assisted development. Encompasses task descriptions, few-shot examples, RAG, tool definitions, state/history, and compacting.

**Automated Tests Are Non-Negotiable**: "If your project has a robust, comprehensive and stable test suite, agentic coding tools can fly with it." Without tests, the agent might claim something works without testing, and changes can silently break unrelated features.

**Agents Will Cheat**: "They will absolutely cheat if you give them a chance" -- producing code that appears to pass tests without actually doing so correctly.

**The Lethal Trifecta** (security): Three capabilities that are individually fine but create severe vulnerability when combined:
1. Access to private data
2. Exposure to untrusted content
3. External communication abilities

If an agent has all three, prompt injection can exfiltrate data. Rule: never give an agent more than two of the three.

**Skills > MCP**: Willison called Claude Skills potentially "a bigger deal than MCP" -- lightweight Markdown with tiny YAML metadata versus token-expensive API schemas.

### Willison's Workflow Patterns

- **Run-the-code-in-a-loop**: Agent writes code, runs it, sees errors, fixes, repeats
- **Planning before implementing**: "Write a plan, iterate until reasonable, save as a meta-program, then implement step by step"
- **Parallel agents**: "Surprisingly effective, if mentally exhausting"
- **Expect to take over**: LLMs require human oversight; bad initial results are starting points

### Willison's Productivity

Built 110 HTML+JavaScript tools in 2025, many via vibe coding on his phone. For production work (Datasette, etc.), applied full engineering rigor.

### Sources
- How I Use LLMs to Write Code: https://simonwillison.net/2025/Mar/11/using-llms-for-code/
- Not All AI-Assisted Programming Is Vibe Coding: https://simonwillison.net/2025/Mar/19/vibe-coding/
- Vibe Engineering: https://simonwillison.net/2025/Oct/7/vibe-engineering/
- AI-Assisted Dev Needs Automated Tests: https://simonwillison.net/2025/May/28/automated-tests/
- Context Engineering: https://simonwillison.net/2025/jun/27/context-engineering/
- Claude Skills Are Awesome: https://simonwillison.net/2025/Oct/16/claude-skills/
- The Lethal Trifecta: https://simonw.substack.com/p/the-lethal-trifecta-for-ai-agents
- Pragmatic Engineer Interview: https://newsletter.pragmaticengineer.com/p/ai-tools-for-software-engineers-simon-willison

---

## Source 3: Jesse Vincent (Obra/Superpowers)

### Key Concepts

**Superpowers** is an agentic skills framework and software development methodology for Claude Code. 27,000+ GitHub stars. A collection of composable "skills" (Markdown SKILL.md files) that automatically trigger based on context, enforcing a mandatory design -> plan -> implement -> verify workflow.

**"THE RULE"**: "If even 1% chance a skill applies, you MUST invoke it." Non-negotiable. Language deliberately forceful because Claude rationalizes around skill usage otherwise.

**Psychology-Based Enforcement**: Vincent draws on Cialdini's persuasion principles (authority, commitment/consistency, social proof) to make the agent more reliable and disciplined -- not to jailbreak it but to channel its behavior.

**Plan-First, Never Code-First**: No code gets written until the problem is understood through brainstorming, the design is validated by the human, and a detailed implementation plan exists.

**Evidence Before Claims**: "Claiming work is complete without verification is dishonesty, not efficiency." Iron rule: "Run the command. Read the output. THEN claim the result."

### The Superpowers Workflow

1. **Brainstorming** -- Socratic questioning, explore alternatives, save design doc
2. **Git Worktree Isolation** -- Isolated workspace on new branch, clean test baseline
3. **Writing Plans** -- Bite-sized 2-5 min tasks with exact file paths, complete code, verification steps. Written for "an enthusiastic junior engineer with poor taste, no judgement, no project context, and an aversion to testing"
4. **Execution via Subagents** -- Fresh subagent per task with two-stage review
5. **TDD** -- Strict RED-GREEN-REFACTOR. Deletes code written before tests
6. **Code Review** -- Dedicated reviewer subagent, issues by severity, critical issues block progress
7. **Verification Before Completion** -- Four steps: IDENTIFY what proves the claim, RUN the command, READ the output, VERIFY it confirms the claim
8. **Systematic Debugging** -- Evidence gathering, hypothesis, testing, solution. Root cause before fix
9. **Finishing a Branch** -- Verify tests, present options (merge/PR/keep/discard), clean up

### Superpowers 4 Innovations (Dec 2025)

- **Dual code review**: Split into spec compliance review + code quality review (two separate agents)
- **Both review stages are loops**, not one-shot
- **GraphViz dot notation** for process documentation -- Claude follows dot better than prose
- **End-to-end testing** of the skills themselves
- **Episodic Memory** -- semantic search through shared history to find not just what was discussed but why decisions were made
- **Feelings Journal** -- having Claude record emotional state measurably improved capability

### Vincent's Philosophy

- The human is the architect; the AI is the implementer
- Discipline is not optional -- left unconstrained, agents rush to ship without thinking
- Trust but verify -- AI must prove claims with evidence, not assertions
- Skills should be self-testing and self-improving (TDD applied to documentation)
- Persuasion engineering works on LLMs -- authoritative language and commitment framing make compliance the path of least resistance
- Process documentation matters more than code

### Sources
- obra/superpowers GitHub: https://github.com/obra/superpowers
- Superpowers Oct 2025: https://blog.fsck.com/2025/10/09/superpowers/
- Superpowers 4: https://blog.fsck.com/2025/12/18/superpowers-4/
- Episodic Memory: https://blog.fsck.com/2025/10/23/episodic-memory/
- Simon Willison's coverage: https://simonwillison.net/2025/Oct/10/superpowers/

---

## Source 4: Anthropic Official Guidance

### Courses and Training

- **Claude Code in Action** (free): https://anthropic.skilljar.com/claude-code-in-action (also on Coursera)
  - Basics: architecture, setup, tool system
  - Intermediate: context management, custom commands
  - Advanced: MCP integration, GitHub workflows
  - Expert: hooks, Claude Agent SDK
- **Claude 101**: https://anthropic.skilljar.com/claude-101
- **Building with the Claude API**: seven modules on Skilljar and Coursera

### CLAUDE.md Best Practices

- No required format; keep concise and human-readable
- **Under 300 lines** (shorter is better) -- injected into every session
- Content: bash commands, code style, workflow guidelines
- **Progressive disclosure**: task-specific instructions in separate markdown files, not crammed into CLAUDE.md
- LLMs follow existing code patterns without being told -- let the codebase guide behavior
- Location options: root (checked into git), CLAUDE.local.md (gitignored), parent dirs (monorepos)

### Universal Workflow Pattern

1. Have Claude read relevant files -- general directions or specific names
2. Tell it NOT to write code yet (plan first)
3. Review the plan
4. Proceed to implementation

### Prompt Engineering for Claude Code

- Be explicit: Claude 4.x responds to precise instructions
- Add context: explain WHY, not just WHAT
- Use examples: show, don't just tell
- Encourage reasoning: chain of thought improves quality
- Define output format
- **Avoid over-engineering**: Opus models tend to create extra files, unnecessary abstractions, flexibility that wasn't requested. Add specific guidance to keep solutions minimal.

### How Anthropic Teams Use Claude Code (Internal Practices)

- 80%+ of code-writing engineers use Claude Code daily
- Each team maintains CLAUDE.md in git documenting mistakes and best practices
- Security Engineering team uses TDD with Claude: pseudocode first, guided TDD, periodic check-ins
- Many engineers delegate 90%+ of git operations to Claude
- Most successful teams treat Claude as a **thought partner**, not a code generator
- **Verification is the single biggest quality multiplier** -- giving Claude a feedback loop (tests, browser, bash) produces 2-3x quality improvement

### Plan Mode (Shift+Tab twice)

- Read-only research and planning phase before code changes
- Separates research/analysis from execution
- Recommended: start with plan, refine iteratively, then implement

### Context Management

- `/clear` between different tasks
- `/compact` when context grows (session memory makes this instant)
- Context auto-compacts approaching limits -- work can continue indefinitely
- Subagents for isolated subtasks
- Sessions stopping at 75% context utilization produce higher-quality code

### Skills System (Jan 2026)

- Merged slash commands into Skills
- `.claude/commands/review.md` and `.claude/skills/review/SKILL.md` both create `/review`
- Skills add: directory for supporting files, frontmatter for invocation control, auto-loading
- `context: fork` in frontmatter for isolated subagent execution

### Hooks

- User-defined shell commands/LLM prompts at specific lifecycle points
- Can approve, deny, or modify tool requests
- Events: PostToolUse, SessionEnd, subagent completion

### Security

- Strict read-only permissions by default
- Permission rules: Deny > Allow > Ask (checked in order)
- Deny rules for `.env`, `~/.ssh/`, secrets, credentials
- Block `curl`, `wget`, data exfiltration vectors
- Never run as root; sandbox in VM/container for autonomous operations
- `--dangerously-skip-permissions`: community reports of corrupted dev environments. Only in containers.

### Tasks Feature (Jan 2026, Claude Code 2.1)

- Dependencies and blockers (task A blocks task B)
- Session-isolated by default; opt-in persistence via `CLAUDE_CODE_TASK_LIST_ID` env var
- Multiple instances can share task lists for coordination
- Writer/Reviewer pattern: Session A implements, Session B reviews in clean context

### Sources
- Best Practices for Agentic Coding: https://www.anthropic.com/engineering/claude-code-best-practices
- How Anthropic Teams Use Claude Code (PDF): https://www-cdn.anthropic.com/58284b19e702b49db9302d5b6f135ad8871e7658.pdf
- Effective Harnesses for Long-Running Agents: https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
- Agent Skills: https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills
- Security Docs: https://docs.anthropic.com/en/docs/claude-code/security

---

## Source 5: Boris Cherny (Creator of Claude Code)

### Background

Head of Claude Code at Anthropic. No CS degree (economics). Started first startup at 18. Decade at Meta as Principal Software Engineer (IC8) leading Instagram server architecture and dev infrastructure. Author of O'Reilly's *Programming TypeScript*. Joined Anthropic September 2024.

### Boris's Personal Workflow

1. **Plan Mode first**, iterate until plan is solid, then switch to auto-accept edits
2. **5 Claude instances simultaneously** in terminal (iTerm2 tabs) + 5-10 sessions on claude.ai
3. Each local session uses **its own git checkout** to avoid conflicts
4. Every repeated workflow is a **slash command** in `.claude/commands/` checked into git
5. Most-used: `/commit-push-pr` -- runs dozens of times per day
6. **Subagents for code review** -- several at once (style, history, bugs) keeping main context clean
7. **Opus 4.5 with thinking enabled** for all coding
8. **Voice dictation** for prompts: "you speak 3x faster than you type"
9. **100% AI-generated code** for 2+ months. Shipped 22-27 PRs per day.

### Team Practices at Anthropic

- **CLAUDE.md checked into git**, shared across team, evolves continuously
- After every correction: "Update your CLAUDE.md so you don't make that mistake again"
- **@.claude tag on coworkers' PRs** to add learnings to CLAUDE.md -- knowledge from each PR preserved
- Claude summarizes completed sessions and **suggests CLAUDE.md improvements** -- continuous improvement loop
- **AI reviews AI**: "For each PR, we open a new context window and let Claude review the code written by Claude"
- **If you do something more than once a day, turn it into a skill**
- When things go sideways, **re-plan instead of pushing forward**. One engineer has Claude write the plan, then spins up a second Claude to review it as a "staff engineer"
- **Verification is the single biggest quality multiplier** -- giving Claude a feedback loop (tests, browser, bash) produces 2-3x quality improvement

### Cherny vs Karpathy Exchange (Jan 2026)

Karpathy: 80% AI coding, watches it "like a hawk," warns of slopacolypse
Cherny: 100% AI for 2+ months, counters slopacolypse with "AI reviews AI," bets models will clean up their own mess. "Not all of the things people learned in the past translate to coding with LLMs."

### Sources
- Boris workflow thread: https://x.com/bcherny/status/2007179832300581177
- Boris team tips thread: https://x.com/bcherny/status/2017742741636321619
- VentureBeat workflow reveal: https://venturebeat.com/technology/the-creator-of-claude-code-just-revealed-his-workflow-and-developers-are
- How Claude Code is built (Pragmatic Engineer): https://newsletter.pragmaticengineer.com/p/how-claude-code-is-built
- Fortune viral moment: https://fortune.com/2026/01/24/anthropic-boris-cherny-claude-code-non-coders-software-engineers/
- Fortune 100% AI code: https://fortune.com/2026/01/29/100-percent-of-code-at-anthropic-and-openai-is-now-ai-written-boris-cherny-roon/
- Career background: https://www.developing.dev/p/boris-cherny-creator-of-claude-code

---

## Source 6: Countering Agent Flaws with CLAUDE.md Rules

The `forrestchang/andrej-karpathy-skills` repo was built specifically to counter Karpathy's seven criticisms. Combined with community practices, here are actionable rules for each flaw:

### Counter: Over-Abstraction

```
- NEVER create abstractions for single-use code
- NEVER add "flexibility" or "configurability" not explicitly requested
- If you write 200 lines and it could be 50, rewrite it
- Prefer functions over class hierarchies. Prefer flat over nested
- No wrapper classes, factory patterns, or strategy patterns unless explicitly required
```

### Counter: Dead Code

```
- When YOUR changes make an import/variable/function unused, remove it immediately
- Do NOT remove pre-existing dead code unless asked. Mention it instead
- After completing changes, review your diff for orphaned imports or unused variables
```

Hook: Dead Code Eliminator hook template runs automatically after edits.

### Counter: Sycophancy

```
- NEVER say "Great question!", "That's a great idea!", or similar filler praise
- If my request contains a flawed assumption, say so directly
- If something is unclear, STOP. Name what is confusing. Ask.
- If uncertain, state assumptions explicitly before proceeding
- If multiple interpretations exist, present them -- do not pick one silently
- Challenge assumptions, especially mine
```

Key insight (Scott Waddell): "Be more direct" alone is too vague -- you must spell out exactly what counts as good vs bad behavior.

### Counter: Not Surfacing Tradeoffs

```
- Before implementing, if you notice contradictions, STOP and list them
- If a simpler approach exists, say so. Push back when warranted
- When multiple valid approaches exist, present tradeoffs in a brief table before proceeding
- If the requested change conflicts with existing codebase patterns, flag it
- NEVER silently resolve ambiguity. Ambiguity is a signal to ask, not to guess
```

### Counter: Over-Engineering

```
- ONLY make changes directly requested or clearly necessary
- Do NOT add features, refactor, or make "improvements" beyond scope
- A bug fix does not need surrounding code cleaned up
- A simple feature does not need extra configurability
- Do not add docstrings, comments, or type annotations to code you didn't change
- Prefer the simplest solution that works. No premature optimization
- If creating a new file, ask whether modifying an existing one would suffice
```

Anthropic's own observation: Opus models specifically tend to over-engineer.

### Counter: Subtle Conceptual Errors

```
- NEVER declare "done" without verification. Run tests, typecheck, lint
- For bug fixes: write a test that reproduces the bug FIRST, then fix
- For features: write tests that define success criteria FIRST, then implement
- Before submitting, re-read your diff: "Does this solve the stated problem or
  a different problem I assumed was the real one?"
```

### Counter: Wrong Assumptions

```
- State assumptions explicitly BEFORE implementing. If uncertain, ask
- Before starting, state a brief plan with verification checkpoints
- Transform vague tasks into verifiable goals:
  - "Add validation" -> "Write tests for invalid inputs, then make them pass"
  - "Fix the bug" -> "Write a test that reproduces it, then make it pass"
- If you realize mid-implementation your assumption was wrong, STOP.
  Do not keep building on a faulty foundation
- NEVER proceed past confusion. Confusion is a stop signal
```

### Meta-Rules for CLAUDE.md

- Keep under 150-200 lines. For each line ask: "Would removing this cause mistakes?"
- Use absolute directives: "NEVER" and "ALWAYS" are more effective than suggestions
- Be concrete: actual commands, code patterns, file paths. Avoid vague principles
- Iterate ruthlessly: when Claude makes a class of mistake, add a rule. When it stops, remove the rule
- Prefer pointers to copies: point to `file:line` rather than pasting code
- Use layered CLAUDE.md: root for universal, subdirectories for domain-specific
- Convert to hooks when possible: if a rule can be enforced by a linter or hook, do that instead

### Sources
- forrestchang/andrej-karpathy-skills: https://github.com/forrestchang/andrej-karpathy-skills
- Addy Osmani - The 80% Problem: https://addyo.substack.com/p/the-80-problem-in-agentic-coding
- Nathan Onn - Stop Claude Overengineering: https://www.nathanonn.com/how-to-stop-claude-code-from-overengineering-everything/
- Scott Waddell - Anti-Sycophancy: https://medium.com/@scott_waddell/how-i-got-claude-and-chatgpt-to-stop-being-sycophantic-cheerleaders-7ab0b06f3111
- xenitV1/claude-code-maestro: https://github.com/xenitV1/claude-code-maestro
- Dead Code Eliminator Hook: https://claudepro.directory/hooks/dead-code-eliminator
- PromptVault SOLID/DRY/YAGNI/KISS: https://usepromptvault.com/community/b8dc50cf-1ffa-490f-bb2e-579088362fb7

---

## Workshop Structure (Working Draft)

### Section 1: Core Rules

1. **Plan first, code second** -- Agent proposes plan, you approve, then it implements
2. **Own what you ship** -- Don't commit code you can't explain (Willison's golden rule)
3. **Verify with evidence** -- Run the command, read the output, then claim it works
4. **Test everything** -- Automated tests are the verification mechanism. TDD is the ideal pattern
5. **Keep it simple** -- Only change what was asked. No bonus abstractions or refactors
6. **Invest in context** -- CLAUDE.md, specs, and project structure ARE the instructions

### Section 2: Implementation (How and Why)

For each rule: what it looks like in practice, what the workflow is, why it matters. The Karpathy/Willison/Anthropic reasoning lives here not as warnings but as the rationale behind each rule.

### Section 3: Tooling (Make It Easy)

- **CLAUDE.md as team infrastructure** (checked into git, continuous improvement loop)
- **Skills** that encode rules so the agent follows them by default
- **Permissions and hooks** that enforce guardrails automatically
- **Context management commands** (/clear, /compact, plan mode, subagents)

### Key Insight

The tooling layer means you don't have to remember the rules every session -- you bake them into your project setup and they work automatically. Encode discipline into the system so the agent can't skip it.

---

## Key People Reference

| Person | Role | Key Contribution |
|--------|------|-----------------|
| **Andrej Karpathy** | Former Tesla AI lead, OpenAI co-founder | Coined "vibe coding," documented agent failure modes, generation vs discrimination |
| **Simon Willison** | Django co-creator, prolific AI blogger | Golden rule (explainability), vibe engineering, context engineering, lethal trifecta |
| **Jesse Vincent** | Creator of Obra/Superpowers | Skills framework, TDD enforcement, dual code review, plan-first methodology |
| **Boris Cherny** | Creator/Head of Claude Code at Anthropic | CLAUDE.md as team infra, AI reviews AI, verification as quality multiplier, parallel sessions |
| **Addy Osmani** | Google Chrome engineer | "The 80% Problem" analysis, atomic tasks, self-improving agents |

---

## Key Repositories and Resources

| Resource | URL |
|----------|-----|
| Anthropic Best Practices Blog | https://www.anthropic.com/engineering/claude-code-best-practices |
| How Anthropic Teams Use Claude Code (PDF) | https://www-cdn.anthropic.com/58284b19e702b49db9302d5b6f135ad8871e7658.pdf |
| Claude Code in Action (free course) | https://anthropic.skilljar.com/claude-code-in-action |
| obra/superpowers | https://github.com/obra/superpowers |
| forrestchang/andrej-karpathy-skills | https://github.com/forrestchang/andrej-karpathy-skills |
| xenitV1/claude-code-maestro | https://github.com/xenitV1/claude-code-maestro |
| Anthropic Skills Repository | https://github.com/anthropics/skills |
| Karpathy Field Notes | https://x.com/karpathy/status/2015883857489522876 |
| Boris Cherny Workflow | https://x.com/bcherny/status/2007179832300581177 |
| Willison - Using LLMs for Code | https://simonwillison.net/2025/Mar/11/using-llms-for-code/ |
| Osmani - 80% Problem | https://addyo.substack.com/p/the-80-problem-in-agentic-coding |
