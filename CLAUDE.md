# Agentic Slides — Claude Code Configuration

## Project

Slidev presentation series on agentic coding best practices. Four talks (talk1.md - talk4.md) plus an original combined deck (slides.md). Custom geekpunk theme in `theme/`.

## Commands

```bash
npm run dev:talk1    # Preview Talk 1
npm run dev:talk2    # etc.
npm install          # Install dependencies
npx slidev build talk1.md  # Build specific talk
```

## File structure

- `talk1.md` - `talk4.md` — Individual talk decks (Slidev markdown)
- `slides.md` — Original combined deck (preserved, don't delete)
- `theme/` — Custom Slidev theme (components, layouts, styles)
- `resources/` — Research notes, failure snippets, workshop drafts
- `docs/plans/` — Design docs and implementation plans

## Slidev conventions

- Use `<v-clicks>` for progressive reveal on bullet slides
- Use `layout: section` dividers between major sections
- Use `<TerminalBlock title="...">` for terminal/CLI examples (wrap a code block inside)
- Use `<GlowCard>` for highlighted cards
- Slides are separated by `---` (with optional frontmatter below)
- Keep slides concise — one idea per slide, punchy language
- Bold for emphasis in bullet points

## Content rules

- This is a presentation, not documentation. Slides should be scannable, not paragraphs.
- Existing polished slide content has been through multiple review iterations. Do not rewrite or rephrase it without explicit approval — modify surgically.
- New slides must match the existing voice: direct, practitioner-focused, no filler.
- Real examples are preferred over fabricated ones. Check `resources/agent-failure-snippets.md` for collected examples.
- All claims should be attributable to sources in the research files under `resources/`.

## Research

All research materials are in `resources/`. Key files:
- `Claude Code Best Practices - Research Notes.md` — Primary research (Karpathy, Willison, Cherny, Vincent, Anthropic)
- `Real World Failure Examples.md` — 32 sourced examples across 10 failure categories
- `Claude Code 101 Research.md` — Official docs, courses, setup tips
- `Workshop Draft.md` — Extended prose versions of talk content
- `Zero to Hero Guide.md` — Progressive adoption guide (Levels 0-4)
- `agent-failure-snippets.md` — Collected real failure transcripts with annotations

## Design docs

Plans and design docs are in `docs/plans/`. Read these before making structural changes:
- `2026-03-10-talk-series-design.md` — Series arc, talk outlines, content mapping
- `2026-03-10-restructure-design.md` — slides.md three-part structure design

## Git workflow

- Work on feature branches, not main
- Commit messages: conventional commits (feat:, fix:, docs:, refactor:)
- Keep commits atomic — one logical change per commit
