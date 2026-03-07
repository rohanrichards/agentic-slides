# Talk Notes & Ideas

## Lessons from building a SaaS with agents
- Could talk about lessons learned while building BetterStreams with Claude Code
- Real production examples, not hypotheticals

## Claude is great at meta tasks
- Claude Code is very good at helping you understand how to do meta tasks like making new skills
- It has a built-in Claude Code guide that can read its own docs
- Claude Code is open source — it can actually read its own source code
- "Help me make a new skill local to this repository that will do X" is a great starting point
- This is a practical tip for the audience: use the agent to learn the agent's own tooling

## The "confidence spiral" anti-pattern
- Without Discover, the agent jumps straight to fixing and enters a loop:
  - "This is the solution" → still broken
  - "OK now I have it, THIS is the fix" → still broken
  - "Wait, HERE'S THE SMOKING GUN" → still broken
- Each iteration it gets MORE confident, makes smaller adjustments, but never actually understands the system
- It's pattern-matching on symptoms instead of understanding the architecture
- This is the clearest example of why Discover matters — if it understood the flow first, it wouldn't be guessing
- Could show this as a TerminalBlock "bad example" vs the Discover approach side by side
- Relatable for anyone who's watched an agent spin its wheels for 10 minutes

## Why Claude Code over IDEs / Cursor

### Claude Code advantages
- Native terminal tool, close to the metal, made by Anthropic themselves
- Developing at lightning speed — new features constantly
- Open source — can read its own source code
- Claude subscription (Max) includes Opus 4.6 at same limits as other models — incredible value
- Opus 4.6 is genuinely that good

### Why Cursor is worse
- API pricing model — you're paying per token on top of subscription
- Limited by Cursor's own features, resources, and capability
- Their agent harness is a bottleneck — you're constrained by what they've built
- IDE integrations for Claude Code are lagging behind the terminal version in features

### Claude Code gaps to acknowledge
- Claude Code Cloud requires GitHub — can't use it at Portable (or similar orgs) without that integration
- IDE integration features lag behind the native terminal experience
- Be honest about these gaps while making the case

## CLAUDE.md as a living document
- CLAUDE.md is the single most important file in an agentic repository — it's the agent's onboarding doc
- Should be treated as a living document, not a one-time setup — the whole team contributes to it
- Every time the agent does something wrong, you add a rule. Every time it does something right, you reinforce the pattern
- It's institutional knowledge for the AI — the same way a team wiki captures human conventions
- Teams should review and update CLAUDE.md the same way they review code — it's part of the codebase
- Without it, every session starts from zero. With it, the agent inherits the team's hard-won lessons
- Practical tip: commit your CLAUDE.md updates alongside the code changes that prompted them

## Spectrum of development & spec-driven development
- There's a spectrum of how much structure you bring to agentic development — from zero-shot prompting to fully spec-driven
- Spec-driven development could slot in *before* the first design phase in our phase development plan
- Before you even start Phase 1 (Discover), you write a spec — the agent then has a contract to work against
- Specs only improve the phase process: Discover is more targeted, Brainstorm is more grounded, Build has clearer acceptance criteria
- This is additive, not a replacement — the phases still run, but with a spec as the foundation
- Could frame this as "the missing Phase 0" or "the pre-phase" that makes everything downstream better
- Practical angle: specs reduce the confidence spiral because the agent has something concrete to verify against instead of guessing
