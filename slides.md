---
theme: ./theme
title: Agentic Coding
info: |
  Best practices for working with AI coding agents
transition: slide-left
mdc: true
---

# Agentic Coding

Best practices for working with AI coding agents

---

# What is Agentic Coding?

Using AI agents as collaborative coding partners

- Autonomous task execution
- Context-aware code generation
- Iterative refinement through conversation
- Tool use and environment interaction

---
layout: section
---

# The Principles

---
layout: two-cols
---

# Human + AI

::left::

**What humans do best:**
- Define the problem
- Set constraints
- Review and validate
- Make judgment calls

::right::

**What agents do best:**
- Search large codebases
- Apply patterns consistently
- Run tests iteratively
- Handle boilerplate

---

# The Workflow

<v-clicks>

- **Spec first** — Write what you want before how
- **Small commits** — Each change is atomic and reviewable
- **Test-driven** — Write failing tests, then implement
- **Review everything** — Trust but verify agent output

</v-clicks>

---
layout: code-focus
---

## Example: Agent-Driven TDD

```typescript
// Step 1: Agent writes the failing test
describe('parseCommand', () => {
  it('should extract action and target', () => {
    const result = parseCommand('deploy api')
    expect(result).toEqual({ action: 'deploy', target: 'api' })
  })
})

// Step 2: Agent implements to pass
function parseCommand(input: string) {
  const [action, target] = input.split(' ')
  return { action, target }
}
```

---

# Key Components

<GlowCard>

**Context is everything.** The agent's output quality is directly proportional to the context you provide. Good specs, clear constraints, and relevant examples make the difference between mediocre and exceptional results.

</GlowCard>

---

# Live Demo

<TerminalBlock title="claude-code">

```
$ claude "add user authentication to the API"

⠋ Reading codebase...
✓ Found 12 relevant files
⠋ Writing auth middleware...
✓ Created src/middleware/auth.ts
✓ Added 8 tests, all passing
✓ Committed: feat(auth): add JWT middleware
```

</TerminalBlock>

---

# The Agent SDK

<AnimatedCode :code="`const agent = new ClaudeAgent({\n  model: 'claude-sonnet-4-20250514',\n  tools: ['read', 'write', 'bash'],\n  maxTokens: 4096,\n})\n\nconst result = await agent.run({\n  task: 'Fix the failing test in auth.spec.ts',\n  context: await loadRelevantFiles(),\n})`" />

---
layout: quote
---

> The best code is the code you don't have to write yourself — but you do have to understand.

— Every senior engineer, eventually

---
layout: section
---

# Questions?

Built with Slidev + Geekpunk theme
