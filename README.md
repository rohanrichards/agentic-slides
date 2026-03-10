# Agentic Coding Best Practices — Talk Series

A series of talks on working effectively and safely with AI coding agents. Built with [Slidev](https://sli.dev/) and a custom geekpunk theme.

## Talks

| File | Title | Description |
|------|-------|-------------|
| `talk1.md` | Getting Started Right | Anatomy of an agent, why Claude Code, 101 tips, setting up CLAUDE.md and permissions |
| `talk2.md` | The Pitfalls | Interactive discussion of problems, named failure patterns, first defenses (explore-first, plan mode, TDD) |
| `talk3.md` | The Workflow | Full Discover → Design → Plan → Build workflow, security practices, best practices in depth |
| `talk4.md` | Levelling Up | Skills, hooks, advanced patterns, the self-improving system |
| `slides.md` | Combined Draft | Original single-deck version (preserved for reference) |

## Getting started

```bash
npm install
npm run dev:talk1    # Preview Talk 1
npm run dev:talk2    # Preview Talk 2
npm run dev:talk3    # Preview Talk 3
npm run dev:talk4    # Preview Talk 4
npm run dev          # Preview slides.md (original combined deck)
```

Navigate slides with arrow keys. Press `o` for slide overview, `d` for dark mode toggle.

## Project structure

```
├── talk1.md - talk4.md      # Individual talk decks
├── slides.md                # Original combined deck
├── theme/                   # Custom geekpunk Slidev theme
│   ├── components/          #   TerminalBlock, GlowCard, AnimatedCode
│   ├── layouts/             #   cover, section, quote, two-cols, etc.
│   └── styles/              #   CSS variables, animations, typography
├── resources/               # Research and reference materials
│   ├── agent-failure-snippets.md  # Collected real examples of agent failures
│   ├── Claude Code Best Practices - Research Notes.md
│   ├── Real World Failure Examples.md
│   ├── Claude Code 101 Research.md
│   ├── Workshop Draft.md
│   ├── Zero to Hero Guide.md
│   └── notes-and-ideas.md
├── docs/plans/              # Design docs and implementation plans
└── package.json
```

## Custom theme components

The geekpunk theme provides these components for use in slides:

- **`<TerminalBlock title="...">`** — Terminal window chrome with a title bar. Wrap a code block inside it.
- **`<GlowCard>`** — Card with animated border glow effect.
- **`<AnimatedCode>`** — Code block with typewriter animation.

## Contributing content

### Adding slides

Each talk is a standalone Slidev markdown file. Follow the existing patterns:
- Use `<v-clicks>` for progressive reveal on bullet slides
- Use `layout: section` for section dividers
- Use `<TerminalBlock>` for terminal/CLI examples
- Keep slides concise — one idea per slide

### Adding failure snippets

See `resources/agent-failure-snippets.md` for instructions on adding real examples of agents getting things wrong. Three options: paste raw, ask your agent to annotate, or fill in the template manually.

### Research materials

All research is in `resources/`. If you find a useful article, blog post, or example, add it there. The existing research files cover Karpathy, Willison, Cherny, Vincent, Anthropic official guidance, and real-world failure examples.

## Building for production

```bash
npm run build              # Build slides.md
npx slidev build talk1.md  # Build a specific talk
npm run build:all          # Build all talks
npm run export             # Export to PDF
```
