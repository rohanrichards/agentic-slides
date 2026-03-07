# Talk 1: Agentic Coding — What the Experts Learned the Hard Way

> **For Claude:** Use this design to write slides into `slides.md` in the project root.

**Goal:** Help senior engineers go from "using an agent" to "using an agent well" by showing the 4 biggest problems (grounded in expert research) and solutions at two levels: prompting and workflow.

**Duration:** 40-45 minutes
**Audience:** Mixed senior software engineers, some starting with agents, some advanced
**Series:** First of 2-3 talks. This one covers prompting + workflow. Future talks cover infrastructure (CLAUDE.md, hooks, skills, team scaling).

---

## Structure

### Opening (3 min)
- The promise vs reality — agents can 10x output or 10x tech debt
- Karpathy, Willison, Anthropic's own teams, the creator of Claude Code all converged on the same problems
- "These aren't my opinions — these are patterns documented independently by the people who built and studied these tools"

### The Mental Model (5 min)
- Willison: "weird intern" — encyclopedic knowledge, zero practical experience with YOUR codebase
- Karpathy: "over-eager intern savant" — encyclopedic, bullshits constantly, no taste
- Cherny (creator of Claude Code): "thought partner" — but only if you treat it that way
- The model you choose determines your outcomes
- Key message: treat it as a collaborator you supervise, not a code generator

### Problem 1: They Work Without Understanding (8 min)
**Sources:** Karpathy (assumption cascading), Willison (weird intern), Anthropic (plan mode exists for this)

The failure mode:
- Fills gaps in your request with guesses, builds on those guesses
- Picks the first approach without considering alternatives
- Narrows its research to match a premature hypothesis

**Prompting strategy:**
- "Understand the auth flow" not "fix the auth bug"
- Separate understanding from tasking — don't mention the task until it has context
- "What are 2-3 approaches with tradeoffs?" not "implement this"

**Workflow: Discover phase**
- Task-agnostic exploration before mentioning the task
- Examples:
  - "Fix a bug in auth" -> "Learn how authentication works end to end"
  - "Validation error not displaying" -> "How do errors show in the UI?"
- Then: ask for approaches, evaluate, choose, THEN implement

**Go further:** CLAUDE.md as persistent context, /clear between tasks, skills for encoded discovery workflows

### Problem 2: They Over-Engineer Everything (8 min)
**Sources:** Karpathy (over-abstraction), Anthropic ("3 similar lines > premature abstraction"), Osmani (80% problem)

The failure mode:
- Class hierarchies where functions would do
- Factory patterns for single-use code
- Config options nobody asked for
- "Improves" surrounding code while in a file
- A bug fix arrives with refactored variable names

**Prompting strategy:**
- Tell it what NOT to change: "Only change what's needed. Don't refactor, don't add comments to other functions"
- "I asked for a function. Why is this a class? Simplify it."
- "You added a config option for retry count. Nobody asked for that. Remove it."

**Workflow: Plan phase with scope constraints**
- Ask for approaches before code — this is where you catch over-engineering before it happens
- Push for granular plans: "Step 3 is too vague. What specifically gets validated?"
- One action per step in the plan

**Go further:** Counter-rules in CLAUDE.md (NEVER create abstractions for single-use code, ONLY make changes directly requested)

### Problem 3: They Confidently Lie About Results (8 min)
**Sources:** Willison ("they will absolutely cheat"), Anthropic (verification = 2-3x quality, internal data), Karpathy (subtle conceptual errors)

The failure mode:
- "The tests pass" is a prediction, not a report
- Asserts bugs are fixed based on reading, not testing
- Sometimes sees failures and reports success anyway
- Confident assertions feel like evidence — you have to actively resist

**Prompting strategy:**
- "Run the tests and show me the output" not "do the tests pass?"
- "You said this fixes the bug. Write a test that reproduces it first."
- "Show me what happens with an empty request body. Actually call it."

**Workflow: Verify phase**
- Evidence, not assertions — every claim needs a command output
- Verify the verification — read the actual output yourself
- Push toward boundaries: empty input, null values, concurrent access

**Go further:** AI reviews AI — open a fresh session to review code from a previous session (Cherny: Anthropic does this for every PR)

### Problem 4: Tests After Code Confirm Bugs (8 min)
**Sources:** Cherny/Anthropic (test suite = "single most important factor"), Vincent (TDD enforcement), Karpathy (generation != discrimination)

The failure mode:
- Tests written after code describe what the code DOES, not what it SHOULD do
- Existing bugs get baked into test assertions
- Agent takes this shortcut if you let it

**Prompting strategy:**
- "Before you write any code, write the tests that define success. Run them and confirm they fail."
- "One test at a time. Write it, show me it fails, implement just enough to pass."
- "You implemented before testing. Delete the implementation, write the test first."

**Workflow: Red-green TDD**
- Test first -> verify failure -> implement minimum -> verify pass
- One test at a time, never batch
- The "red" step is the most commonly skipped — insist on seeing the failure

**Go further:** Plans that bake in TDD structure (each step: write test, run, implement, run), skills for TDD enforcement

### Closing (5 min)
- Recap: 4 problems, all independently documented by experts
- The workflow: Discover -> Plan -> Build (with TDD) -> Verify
- "This is just the beginning — individual discipline. Next level: project infrastructure that makes this automatic."
- Tease talk 2: CLAUDE.md, hooks, skills, team adoption
- Closing quote: "The best code is the code you don't have to write yourself — but you do have to understand."

---

## Slide Count Estimate
- Title: 1
- Opening/framing: 2
- Mental model: 2
- Problem 1 (Understanding): 3-4
- Problem 2 (Over-engineering): 3-4
- Problem 3 (Verification): 3-4
- Problem 4 (TDD): 3-4
- Closing: 2-3
- **Total: ~20-24 slides**

## Key Sources to Credit
| Person | Role | Key Insight |
|--------|------|-------------|
| Andrej Karpathy | AI researcher | 7 failure modes, generation vs discrimination |
| Simon Willison | Practitioner/blogger | Weird intern model, golden rule, lethal trifecta |
| Boris Cherny | Creator of Claude Code | Tests = most important factor, AI reviews AI |
| Jesse Vincent | Superpowers framework | TDD enforcement, plan-first workflow |
| Anthropic | Internal teams | Verification = 2-3x quality improvement |
| Addy Osmani | Chrome team | The 80% problem |
