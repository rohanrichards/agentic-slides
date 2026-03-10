# Getting Started Right with Claude Code -- Research Report

## Research Methodology
Six official and expert sources researched in parallel using multi-query decomposition. All findings extracted from primary sources, cross-referenced for consistency, and categorized by priority.

---

## SECTION 1: Official Claude Code Documentation (code.claude.com/docs)

### 1.1 The /init Command
- **Source**: [Claude Code Memory Docs](https://code.claude.com/docs/en/memory), [Best Practices](https://code.claude.com/docs/en/best-practices)
- **Tip**: Run `/init` in your project root. It analyzes your codebase (build systems, test frameworks, code patterns) and generates a starter CLAUDE.md file. If one already exists, it suggests improvements rather than overwriting.
- **Why it matters**: Gets you 80% of the way to a good CLAUDE.md without writing anything from scratch. Detects conventions you might forget to document.
- **Priority**: DAY ONE ESSENTIAL

### 1.2 CLAUDE.md Setup Recommendations
- **Source**: [Memory Docs](https://code.claude.com/docs/en/memory), [Best Practices](https://code.claude.com/docs/en/best-practices)
- **Tips**:
  - Keep it under 200 lines. Bloated files cause Claude to ignore your actual instructions.
  - Include only things Claude cannot figure out by reading code: bash commands it cannot guess, code style rules that differ from defaults, testing instructions, repo etiquette (branch naming, PR conventions), architectural decisions, developer environment quirks, common gotchas.
  - Exclude: standard language conventions Claude already knows, detailed API docs (link instead), information that changes frequently, file-by-file codebase descriptions, self-evident practices like "write clean code."
  - For each line, ask: "Would removing this cause Claude to make mistakes?" If not, cut it.
  - Add emphasis (e.g., "IMPORTANT" or "YOU MUST") for critical rules to improve adherence.
  - Check CLAUDE.md into git so your team can contribute.
  - Use `@path/to/file` import syntax to reference READMEs, package.json, etc. without duplicating content.
- **Why it matters**: CLAUDE.md is loaded every session and consumes context tokens. Too long = instructions get lost. Too vague = Claude ignores them. This is the single most important configuration file.
- **Priority**: DAY ONE ESSENTIAL

### 1.3 CLAUDE.md File Hierarchy
- **Source**: [Memory Docs](https://code.claude.com/docs/en/memory)
- **Tips**:
  - `~/.claude/CLAUDE.md` -- personal preferences for all projects (just you)
  - `./CLAUDE.md` or `./.claude/CLAUDE.md` -- project root, check into git (team-shared)
  - Parent directories -- useful for monorepos (both root/CLAUDE.md and root/foo/CLAUDE.md load)
  - Child directories -- loaded on demand when Claude works in those dirs
  - `.claude/rules/*.md` -- path-specific rules that only load when matching files are opened
- **Why it matters**: Understanding the hierarchy means you can put personal preferences in one place, team standards in another, and never have them conflict.
- **Priority**: DAY ONE ESSENTIAL

### 1.4 Permission Configuration Best Practices
- **Source**: [Settings Docs](https://code.claude.com/docs/en/settings), [Best Practices](https://code.claude.com/docs/en/best-practices)
- **Tips**:
  - Default: Claude asks permission for file writes, bash commands, MCP tools. Safe but tedious.
  - Use `/permissions` to allowlist safe commands you run often (e.g., `npm run lint`, `git commit`, `bun run test:*`).
  - Store permission rules in `.claude/settings.json` and check into git for team sharing.
  - For contained workflows (lint fixes, boilerplate), consider sandboxing via `/sandbox` for OS-level isolation.
  - Only use `--dangerously-skip-permissions` in containers without internet access.
  - Three permission modes cycle via Shift+Tab: Normal -> Auto-Accept -> Plan Mode.
- **Why it matters**: Without configuring permissions, you'll click "approve" hundreds of times per session. Allowlisting safe commands removes 80% of interruptions while keeping you in control.
- **Priority**: DAY ONE ESSENTIAL

### 1.5 Plan Mode (Shift+Tab twice)
- **Source**: [Common Workflows](https://code.claude.com/docs/en/common-workflows), [Best Practices](https://code.claude.com/docs/en/best-practices)
- **Tips**:
  - Shift+Tab cycles: Normal Mode -> Auto-Accept Mode -> Plan Mode.
  - In Plan Mode, Claude reads files and answers questions but makes NO changes.
  - Use for: multi-step implementations, code exploration, when unfamiliar with the code.
  - Workflow: Explore (Plan Mode) -> Plan (Plan Mode) -> Implement (Normal Mode) -> Commit.
  - Press Ctrl+G to open the plan in your text editor for direct editing before Claude proceeds.
  - Skip planning when the scope is clear and the fix is small (typo, log line, rename).
  - Can start a session in Plan Mode: `claude --permission-mode plan`.
- **Why it matters**: Jumping straight to coding often produces code that solves the wrong problem. Plan Mode separates thinking from doing, which is especially critical for beginners who haven't yet developed intuition for when Claude needs guidance.
- **Priority**: DAY ONE ESSENTIAL

### 1.6 Key Slash Commands Every User Should Know
- **Source**: [Quickstart](https://code.claude.com/docs/en/quickstart), [Common Workflows](https://code.claude.com/docs/en/common-workflows)
- **Tips**:
  - `/help` -- show available commands
  - `/init` -- generate starter CLAUDE.md
  - `/clear` -- reset conversation context (use between unrelated tasks)
  - `/compact [instructions]` -- summarize context to free space (e.g., `/compact Focus on the API changes`)
  - `/memory` -- browse all loaded CLAUDE.md files and auto memory
  - `/permissions` -- manage permission allowlists
  - `/hooks` -- configure lifecycle event hooks
  - `/model` -- switch models or adjust effort level
  - `/resume` -- pick from recent conversations
  - `/rename` -- give sessions descriptive names
  - `/rewind` -- restore conversation and/or code to a previous checkpoint
  - `/status` -- see active settings sources and errors
  - `/config` -- toggle settings like thinking mode
  - `/sandbox` -- enable OS-level isolation
  - `/agents` -- view/create subagents
  - `/plugin` -- browse plugin marketplace
  - Type `/` to see all available commands and skills
- **Why it matters**: Slash commands are the primary interface for controlling Claude Code beyond natural language. Knowing these saves significant time.
- **Priority**: DAY ONE ESSENTIAL (top 6), NICE TO KNOW (rest)

### 1.7 Context Management (/clear, /compact)
- **Source**: [Best Practices](https://code.claude.com/docs/en/best-practices)
- **Tips**:
  - Context window is THE most important resource to manage. Performance degrades as it fills.
  - `/clear` -- use FREQUENTLY between unrelated tasks. "The single most underused command."
  - `/compact [instructions]` -- when auto-compaction triggers, Claude summarizes what matters most. You can also trigger it manually with custom focus (e.g., `/compact Focus on the API changes`).
  - After two failed corrections on the same issue, `/clear` and start fresh with a better prompt.
  - Use `Esc + Esc` or `/rewind` to selectively summarize from a specific message.
  - Customize compaction in CLAUDE.md: "When compacting, always preserve the full list of modified files and any test commands."
  - Use subagents for investigation to keep your main context clean.
- **Why it matters**: This is the #1 cause of degraded output quality. Most beginners never use /clear, leading to "kitchen sink sessions" where context is full of irrelevant information. Claude literally gets dumber as context fills.
- **Priority**: DAY ONE ESSENTIAL

### 1.8 Session Management
- **Source**: [Common Workflows](https://code.claude.com/docs/en/common-workflows)
- **Tips**:
  - `claude --continue` (-c) -- resume most recent conversation in current directory.
  - `claude --resume` (-r) -- select from recent conversations.
  - `/rename` -- name sessions (e.g., "oauth-migration") so you can find them later.
  - Treat sessions like branches: different workstreams get separate persistent contexts.
  - Every action creates a checkpoint. Double-tap Escape or `/rewind` to restore conversation, code, or both.
  - Checkpoints persist across sessions -- close terminal, come back later, still rewind.
  - `claude --from-pr 123` -- resume sessions linked to a specific PR.
- **Why it matters**: Work spans multiple sessions. Without naming and resuming, you waste time re-explaining context.
- **Priority**: NICE TO KNOW (first week)

### 1.9 Keyboard Shortcuts
- **Source**: [Quickstart](https://code.claude.com/docs/en/quickstart), [Common Workflows](https://code.claude.com/docs/en/common-workflows)
- **Tips**:
  - `?` -- see all keyboard shortcuts
  - `Tab` -- command completion
  - Up arrow -- command history
  - `Esc` -- stop Claude mid-action (context preserved, can redirect)
  - `Esc + Esc` -- open rewind menu
  - `Shift+Tab` -- cycle permission modes (Normal -> Auto-Accept -> Plan)
  - `Ctrl+G` -- open plan in text editor
  - `Ctrl+O` -- toggle verbose mode (see thinking process)
  - `Option+T` / `Alt+T` -- toggle thinking mode
  - `@` -- reference files directly in prompts
- **Why it matters**: Keyboard shortcuts are the fastest way to control Claude Code. The Esc key alone is a game-changer.
- **Priority**: DAY ONE ESSENTIAL (Esc, Shift+Tab, @), NICE TO KNOW (rest)

---

## SECTION 2: Claude Code in Action Course (Anthropic Skilljar)

### Source: [Claude Code in Action](https://anthropic.skilljar.com/claude-code-in-action), summarized via [SmartScope Guide](https://smartscope.blog/en/generative-ai/claude/claude-code-course-guide/) and [Joe Njenga's writeup](https://medium.com/ai-software-engineer/anthropic-just-released-claude-code-course-and-i-earned-my-certificate-ad68745d46de)

### 2.1 Module Breakdown

**Part 1 -- Fundamentals**: Claude Code operates as an agent using a "tool system." File reads, edits, bash commands are all defined as tools. Claude combines multiple tools to handle complex tasks step by step.
- **Priority**: DAY ONE ESSENTIAL (conceptual understanding)

**Part 2 -- Context Management (CLAUDE.md)**: Files load hierarchically. Build CLAUDE.md iteratively by recording corrections, not writing comprehensive manuals upfront. Use `#` shortcut to add quick insights. Emphasis markers like `**Important**` improve compliance.
- **Priority**: DAY ONE ESSENTIAL

**Part 3 -- Custom Commands (Slash Commands)**: Store commands as Markdown templates in `.claude/commands/`. Arguments via `$ARGUMENTS`. Example: `/fix-github-issue 123` automates entire dev cycles. Chain strict review commands to loop "until no issues found."
- **Priority**: NICE TO KNOW (first week)

**Part 4 -- MCP Server Integration**: Add via `claude mcp add` with scope options (local, project, user). Context7 MCP solves AI knowledge gaps by fetching latest library specs. Database MCP enables natural language SQL queries.
- **Priority**: NICE TO KNOW (second week)

**Part 5 -- Hooks (Deterministic Automation)**: Seven event types: PreToolUse, PostToolUse, UserPromptSubmit, PermissionRequest, Stop, SessionStart, SessionEnd. Exit code 2 blocks tool execution. PostToolUse + file path = auto-format after edits. PreToolUse = block dangerous operations before Claude attempts them. Stop hooks = automatic end-of-turn quality checks.
- **Priority**: NICE TO KNOW (advanced)

**Part 6 -- GitHub Integration**: GitHub Actions with `anthropics/claude-code-action@v1`. Automated PR review. Natural language to multi-step automation.
- **Priority**: NICE TO KNOW (team setup)

**Part 7 -- Claude Code SDK**: Programmatic integration for custom tools. Tool allowlisting via `allowedTools`. Batch processing beyond CLI.
- **Priority**: NICE TO KNOW (advanced)

### 2.2 Non-Obvious Course Tips
- The `#` shortcut in the terminal lets you add insights to CLAUDE.md on the fly without opening the file.
- Avoid duplicating instructions across CLAUDE.md files -- contradictions confuse Claude.
- Security consideration: review hook configurations when cloning third-party repos, since hooks execute automatically.
- Suggested learning sequence: Week 1 (fundamentals + CLAUDE.md), Week 2 (slash commands), Week 3 (MCP), Week 4 (hooks + GitHub Actions).

---

## SECTION 3: Anthropic's "Best Practices for Agentic Coding" Blog Post

### Source: [Best Practices](https://code.claude.com/docs/en/best-practices) (now at code.claude.com/docs, redirected from anthropic.com/engineering)

### 3.1 The #1 Tip: Give Claude a Way to Verify Its Work
- **Tip**: Include tests, screenshots, or expected outputs so Claude can check itself. This is "the single highest-leverage thing you can do."
- **Example**: Instead of "implement a function that validates email addresses," say "write a validateEmail function. Example test cases: user@example.com is true, invalid is false, user@.com is false. Run the tests after implementing."
- **Why it matters**: Without verification criteria, Claude produces plausible-looking code that might not work. You become the only feedback loop.
- **Priority**: DAY ONE ESSENTIAL

### 3.2 Explore First, Then Plan, Then Code
- **Tip**: Four-phase workflow: Explore (Plan Mode) -> Plan (Plan Mode) -> Implement (Normal Mode) -> Commit.
- **Why it matters**: Letting Claude jump straight to coding produces code that solves the wrong problem.
- **Priority**: DAY ONE ESSENTIAL

### 3.3 Provide Specific Context in Prompts
- **Tips**:
  - Scope the task: specify which file, what scenario, testing preferences.
  - Point to sources: "look through ExecutionFactory's git history" instead of "why does it have such a weird api?"
  - Reference existing patterns: "look at HotDogWidget.php and follow the pattern" instead of "add a calendar widget."
  - Describe symptoms: provide the symptom, likely location, and what "fixed" looks like.
  - Use `@` to reference files instead of describing where code lives.
  - Paste images directly (copy/paste or drag-and-drop).
  - Pipe in data: `cat error.log | claude`.
- **Priority**: DAY ONE ESSENTIAL

### 3.4 Let Claude Interview You for Big Features
- **Tip**: For larger features, start with "I want to build [brief description]. Interview me in detail using the AskUserQuestion tool. Ask about technical implementation, UI/UX, edge cases, concerns, and tradeoffs. Don't ask obvious questions, dig into the hard parts."
- **Why it matters**: Claude asks about things you haven't considered. Results in a written spec you can execute in a fresh session.
- **Priority**: NICE TO KNOW (powerful but not day-one)

### 3.5 Course-Correct Early and Often
- **Tips**:
  - `Esc` to stop mid-action (context preserved).
  - `Esc + Esc` or `/rewind` to restore previous state.
  - "Undo that" to have Claude revert changes.
  - `/clear` to reset context between unrelated tasks.
  - After 2+ corrections on the same issue, `/clear` and start fresh with a better prompt.
- **Priority**: DAY ONE ESSENTIAL

### 3.6 Common Failure Patterns to Avoid
- **The kitchen sink session**: Start with one task, ask something unrelated, go back. Fix: `/clear` between tasks.
- **Correcting over and over**: Context polluted with failed approaches. Fix: After two failures, `/clear` and write a better initial prompt.
- **The over-specified CLAUDE.md**: Too long, Claude ignores half. Fix: Ruthlessly prune.
- **The trust-then-verify gap**: Plausible code that doesn't handle edge cases. Fix: Always provide verification.
- **The infinite exploration**: Claude reads hundreds of files filling context. Fix: Scope investigations or use subagents.
- **Priority**: DAY ONE ESSENTIAL (memorize these)

### 3.7 Scaling: Parallel Sessions and Fan-Out
- **Tips**:
  - `claude -p "prompt"` for non-interactive mode (CI, scripts, pre-commit hooks).
  - `--output-format json` or `stream-json` for structured output.
  - `claude --worktree feature-name` creates isolated worktree with its own branch.
  - Writer/Reviewer pattern: one session implements, another reviews with fresh context.
  - Fan-out: loop through files calling `claude -p` for each, with `--allowedTools` to scope permissions.
- **Priority**: NICE TO KNOW (week two)

---

## SECTION 4: How Anthropic Teams Use Claude Code (PDF/Blog)

### Source: [Anthropic Blog](https://claude.com/blog/how-anthropic-teams-use-claude-code), [Ernest Chiang Summary](https://www.ernestchiang.com/en/posts/2025/how-anthropic-teams-use-claude-code/)

### 4.1 Treat Claude as a Thought Partner, Not a Code Generator
- **Tip**: The most successful teams explore possibilities, prototype rapidly, and share discoveries across technical and non-technical users.
- **Why it matters**: Teams that just say "write this function" get worse results than teams that collaborate with Claude on the problem.
- **Priority**: DAY ONE ESSENTIAL (mindset)

### 4.2 Use Auto-Accept Mode for Rapid Prototyping
- **Tip**: Product teams enable auto-accept mode and let Claude work autonomously, writing ~70% of implementation code. Review the 80% complete solution, then take over for final refinements.
- **Why it matters**: Reduces the review-per-change overhead for exploratory work.
- **Priority**: NICE TO KNOW

### 4.3 The Self-Verification Loop
- **Tip**: Framework operates as "write code -> run tests/CI -> automatically fix errors." This is the core agentic development cycle.
- **Why it matters**: When Claude can verify its own work, quality jumps 2-3x.
- **Priority**: DAY ONE ESSENTIAL

### 4.4 Team Workflow Sharing Sessions
- **Tip**: Anthropic teams held sessions where members demonstrated their Claude Code workflows to each other. This spread best practices and revealed different usage patterns people hadn't discovered on their own.
- **Why it matters**: No two people use Claude Code the same way. Sharing workflows multiplies everyone's effectiveness.
- **Priority**: NICE TO KNOW (but recommend for your talk audience)

### 4.5 Specific Team Use Cases
- **Data Infrastructure**: Fed Kubernetes error screenshots into Claude Code; it diagnosed IP exhaustion and provided exact gcloud commands. Saved ~20 min per incident.
- **Security Engineering**: Shifted to TDD with Claude. Stack trace analysis went from 10-15 min to ~5 min (3x speedup).
- **Product Design**: Imports Figma files, initiates autonomous loops where Claude writes features, runs tests, iterates.
- **Data Science**: New data scientists use Claude to navigate massive codebases by reading CLAUDE.md files first.
- **Legal**: Prototyped phone tree systems without engineering help -- non-technical departments can build custom solutions.
- **Growth Marketing**: Built agentic system processing CSV ad files, generating hundreds of ad variations in minutes instead of hours.
- **Priority**: NICE TO KNOW (great examples for your talk)

### 4.6 Two-Agent Systems Beat Monolithic
- **Tip**: Specialized sub-agents handling specific tasks (identification + generation) work more effectively than single monolithic agents.
- **Priority**: NICE TO KNOW (advanced)

---

## SECTION 5: Boris Cherny's Team Tips (Twitter/Threads)

### Source: [Boris Cherny's Twitter Thread](https://x.com/bcherny/status/2017742741636321619), [10 Tips Summary](https://paddo.dev/blog/claude-code-team-tips/), [How Boris Uses Claude Code](https://paddo.dev/blog/how-boris-uses-claude-code/)

### 5.1 Check CLAUDE.md into Git
- **Tip**: The Claude Code team maintains a shared CLAUDE.md checked into git. The entire team contributes to it multiple times a week.
- **Why it matters**: CLAUDE.md becomes institutional memory that compounds over time. New team members get immediate context. Everyone benefits from everyone else's corrections.
- **Priority**: DAY ONE ESSENTIAL

### 5.2 The Correction-to-Rule Loop
- **Tip**: After every correction, end with: "Update your CLAUDE.md so you don't make that mistake again." Claude is "eerily good" at writing rules for itself. Ruthlessly edit your CLAUDE.md over time.
- **How it works**: Claude makes mistake -> you correct it -> Claude writes a rule -> mistake rate drops -> repeat. Build CLAUDE.md iteratively, not as a comprehensive manual.
- **Why it matters**: This creates a self-improving system. Over time, Claude's error rate measurably drops for your specific project.
- **Priority**: DAY ONE ESSENTIAL (this is THE workflow for CLAUDE.md)

### 5.3 Working on CLAUDE.md as a Team
- **Tip**: It is each team's job to keep their CLAUDE.md up to date. Anytime Claude does something incorrectly, add it to CLAUDE.md so Claude knows not to do it next time.
- **Why it matters**: Makes CLAUDE.md a living document rather than a one-time setup artifact.
- **Priority**: DAY ONE ESSENTIAL

### 5.4 The @.claude PR Tag Practice
- **Tip**: During code review, tag @.claude on coworkers' PRs to add something to the CLAUDE.md as part of the PR. Uses the Claude Code GitHub Action (`/install-github-action`).
- **Why it matters**: CLAUDE.md improvements happen organically during code review, which is when you discover the patterns and anti-patterns.
- **Priority**: NICE TO KNOW (team-level setup)

### 5.5 CLAUDE.md vs CLAUDE.local.md
- **Tip**: CLAUDE.md is shared (checked into git). CLAUDE.local.md is personal (gitignored). Use `.claude/settings.local.json` for personal project overrides. Import personal preferences from `~/.claude/CLAUDE.md` or `~/.claude/my-project-instructions.md`.
- **Example**: Project CLAUDE.md has team standards. Your personal `~/.claude/CLAUDE.md` has your code style preferences and tooling shortcuts.
- **Why it matters**: Separates team knowledge from personal preferences. Team rules are shared; personal quirks are not.
- **Priority**: DAY ONE ESSENTIAL

### 5.6 Parallelization Is the Top Unlock
- **Tip**: Run 3-5 git worktrees simultaneously, each with its own Claude session. Shell aliases (za, zb, zc) for one-keystroke switching. Also maintain 5-10 sessions on claude.ai/code with `--teleport` to switch between local and web.
- **Why it matters**: Single biggest multiplier for productivity. Separate working directories prevent cross-contamination.
- **Priority**: NICE TO KNOW (week two, but high-impact)

### 5.7 Most Sessions Start in Plan Mode
- **Tip**: Start most sessions in Plan Mode (Shift+Tab twice). Iterate with Claude on the plan before switching to auto-accept mode for execution. "A good plan is really important!"
- **Priority**: DAY ONE ESSENTIAL

### 5.8 Slash Commands as Institutional Knowledge
- **Tip**: Convert any repeated daily task into a reusable skill checked into version control. Store in `.claude/commands/`. Example: `/commit-push-pr` used dozens of times daily.
- **Why it matters**: Battle-tested workflows become immediately available to new team members.
- **Priority**: NICE TO KNOW (first week)

### 5.9 Use PostToolUse Hooks for Auto-Formatting
- **Tip**: Use PostToolUse hooks to auto-format code after generation, handling the "final 10%" that prevents CI failures. Apply deterministic triggers for mechanical problems.
- **Priority**: NICE TO KNOW (advanced)

### 5.10 Give Claude a Way to Verify Its Work
- **Tip**: "Probably the most important thing to get great results out of Claude Code -- give Claude a way to verify its work." Improves quality 2-3x.
- **Why it matters**: This is the single tip that appears in EVERY source. It is consensus #1.
- **Priority**: DAY ONE ESSENTIAL

### 5.11 Prompting as Provocation
- **Tip**: Challenge Claude to justify its work: "Grill me on these changes before making a PR." "Prove this works by diffing behavior." After mediocre fixes: "Knowing everything now, implement the elegant solution."
- **Why it matters**: Treats Claude as a peer requiring justification, elevating output quality.
- **Priority**: NICE TO KNOW (powerful technique)

### 5.12 Use /permissions, Not --dangerously-skip-permissions
- **Tip**: Pre-allow safe bash commands via `/permissions` (e.g., `bun run build:*`, `bun run test:*`). Store in `.claude/settings.json` for team sharing.
- **Priority**: DAY ONE ESSENTIAL

---

## SECTION 6: What Makes a Good vs Bad Agent

### Source: [Decoding Claude Code (MinusX)](https://minusx.ai/blog/decoding-claude-code/), [Deep Agent Architecture (DEV)](https://dev.to/apssouza22/a-deep-dive-into-deep-agent-architecture-for-ai-coding-assistants-3c8b), various search results

### 6.1 Architectural Simplicity is the Key Differentiator
- **Finding**: Claude Code uses one main loop with a flat list of messages, NOT multi-agent orchestration. Maximum one sub-agent branch, results reintegrated as tool responses. "With every layer of abstraction you add, your system becomes harder to debug and deviates from the general-model-improvement trajectory."
- **Why it matters for beginners**: You don't need complex setups. The tool works because it's simple, not despite being simple.
- **Priority**: NICE TO KNOW (context for your talk)

### 6.2 Tool System Design
- **Finding**: Layered tool hierarchy -- Low-level (Bash, Read, Write), Medium-level (Edit, Grep, Glob), High-level (Task, WebFetch, TodoWrite). High-frequency actions get dedicated tools with elaborate prompt descriptions and examples, reducing decision complexity for the model.
- **Why it matters**: Claude Code rejects RAG-based search in favor of LLM-driven commands (ripgrep, jq, find). Hidden failure modes in RAG (similarity functions, chunking) create opacity. LLM search is learnable through RL.
- **Priority**: NICE TO KNOW

### 6.3 Context Window Management is Everything
- **Finding**: System prompt ~2,800 tokens. Tool descriptions ~9,400 tokens. CLAUDE.md ~1,000-2,000 tokens. Over 50% of important LLM calls use Claude 3.5 Haiku for auxiliary tasks (file reading, git history, summarization) at 70-80% cost reduction.
- **Why it matters**: Explains why keeping CLAUDE.md concise matters -- every token competes for space in the context window.
- **Priority**: NICE TO KNOW (explains the "why" behind context management)

### 6.4 Prompt Engineering Inside Claude Code
- **Finding**: Uses XML tags like `<system-reminder>` for reminders. Good/bad examples contrasting acceptable vs problematic tool-calling patterns. Explicit capitalization ("IMPORTANT", "VERY IMPORTANT") for critical rules. Algorithmic descriptions rather than disconnected rule lists.
- **Why it matters**: This is why emphasis markers work in CLAUDE.md -- the system itself uses them.
- **Priority**: NICE TO KNOW

### 6.5 Self-Managed Todo Lists
- **Finding**: The agent maintains an explicit, self-managed todo list. Unlike multi-agent handoff systems (PM -> implementer -> QA), this preserves model flexibility while combating context rot through frequent todo list reference prompting.
- **Why it matters**: Claude can course-correct mid-implementation while remaining goal-focused. This is why breaking tasks into steps works.
- **Priority**: NICE TO KNOW

### 6.6 Why Different Agents Produce Different Quality
- **Finding**: Same model, different results because of:
  1. Minimized moving parts (single loop eliminates hidden failure modes)
  2. Dedicated tool specialization (reduces decision complexity)
  3. Explicit task tracking (maintains coherence in long sessions)
  4. Context file standardization (CLAUDE.md provides persistent, human-editable constraints)
  5. Elaborate prompting with heuristics, examples, and reminders
- **Why it matters**: The agent harness matters as much as the model. This is why Claude Code produces better results than other tools using the same Claude model.
- **Priority**: NICE TO KNOW (great talking point)

---

## CONSOLIDATED: "FIRST HOUR" CHECKLIST

For someone sitting down with Claude Code for the very first time, do these in order:

1. Install Claude Code (`curl -fsSL https://claude.ai/install.sh | bash` or platform equivalent)
2. `cd your-project && claude` -- start your first session
3. Run `/init` to generate a starter CLAUDE.md
4. Read and prune the generated CLAUDE.md (under 200 lines, only things Claude can't infer)
5. Learn the permission mode cycle: Shift+Tab (Normal -> Auto-Accept -> Plan)
6. Start in Plan Mode for your first real task: "I want to [goal]. Create a plan."
7. Run `/permissions` to allowlist your safe commands (test runners, linters, git)
8. Ask your first real question: "What does this project do?" or "Explain the folder structure"
9. Make your first change: describe what you want, review the diff, approve
10. After your task, run `/clear` before starting something unrelated
11. When Claude makes a mistake, correct it and say "Update your CLAUDE.md so you don't make that mistake again"
12. Give Claude verification: "Run the tests after implementing" or "Take a screenshot and compare"

## CONSOLIDATED: "FIRST WEEK" ADDITIONS

13. Name your sessions with `/rename` so you can find them later
14. Create your first slash command in `.claude/commands/`
15. Try the Writer/Reviewer pattern with two sessions
16. Set up git worktrees for parallel work (`claude --worktree feature-name`)
17. Install the `gh` CLI so Claude can interact with GitHub natively
18. Try piping data: `cat error.log | claude -p "explain the root cause"`
19. Have Claude interview you for a bigger feature
20. Check your CLAUDE.md into git and tell your team to contribute

---

## CROSS-SOURCE CONSENSUS (Tips That Appeared in 3+ Sources)

These tips were independently recommended by multiple official sources:

1. **Give Claude a way to verify its work** (ALL 6 sources) -- Consensus #1 by far
2. **Keep CLAUDE.md concise and iterate on it** (5 sources)
3. **Use /clear between unrelated tasks** (4 sources)
4. **Start in Plan Mode for non-trivial tasks** (4 sources)
5. **Check CLAUDE.md into git for team use** (4 sources)
6. **Use the correction-to-rule loop** (3 sources)
7. **Be specific in prompts with file references** (3 sources)
8. **Use subagents to keep main context clean** (3 sources)

---

## SOURCES

- [Claude Code Overview](https://code.claude.com/docs/en/overview)
- [Claude Code Quickstart](https://code.claude.com/docs/en/quickstart)
- [Best Practices for Claude Code](https://code.claude.com/docs/en/best-practices)
- [How Claude Remembers Your Project (Memory)](https://code.claude.com/docs/en/memory)
- [Claude Code Settings](https://code.claude.com/docs/en/settings)
- [Common Workflows](https://code.claude.com/docs/en/common-workflows)
- [Claude Code in Action Course (Skilljar)](https://anthropic.skilljar.com/claude-code-in-action)
- [SmartScope Course Guide](https://smartscope.blog/en/generative-ai/claude/claude-code-course-guide/)
- [How Anthropic Teams Use Claude Code (Blog)](https://claude.com/blog/how-anthropic-teams-use-claude-code)
- [How Anthropic Teams Use Claude Code (PDF)](https://www-cdn.anthropic.com/58284b19e702b49db9302d5b6f135ad8871e7658.pdf)
- [Ernest Chiang Summary](https://www.ernestchiang.com/en/posts/2025/how-anthropic-teams-use-claude-code/)
- [Boris Cherny Twitter Thread (Team Tips)](https://x.com/bcherny/status/2017742741636321619)
- [Boris Cherny Twitter Thread (Personal Setup)](https://x.com/bcherny/status/2007179832300581177)
- [10 Tips from Inside the Claude Code Team (Paddo)](https://paddo.dev/blog/claude-code-team-tips/)
- [How Boris Uses Claude Code (Paddo)](https://paddo.dev/blog/how-boris-uses-claude-code/)
- [Decoding Claude Code Architecture (MinusX)](https://minusx.ai/blog/decoding-claude-code/)
