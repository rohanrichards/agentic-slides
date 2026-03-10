---
theme: ./theme
title: "Getting Started Right"
info: |
  Talk 1 of the Agentic Coding Best Practices series.
  What agents are, why Claude Code, and how to set yourself up right from day one.
transition: slide-left
mdc: true
---

# Getting Started Right

Agentic Coding Best Practices — Talk 1

---
layout: section
---

# Anatomy of an Agent

What's actually happening under the hood

---

# What Is an Agent?

Not autocomplete. An autonomous tool that reads your codebase, writes code, runs commands, and iterates on the results.

<v-clicks>

- It has **tools** — file reads, edits, bash commands, web search, subagents
- It calls those tools in a loop — act, observe, decide, act again
- It manages its own context — decides what to read, what to compact, when to delegate
- Left unsupervised, it will **confidently build the wrong thing**

</v-clicks>

---

# How It Works

<div style="display: flex; gap: 2rem; align-items: stretch; margin-top: 1rem;">
  <div style="flex: 1.3; border: 1px solid rgba(0, 212, 255, 0.3); border-radius: 10px; padding: 1.25rem; background: rgba(0, 212, 255, 0.05);">
    <div style="font-size: 0.75rem; text-transform: uppercase; letter-spacing: 0.1em; color: #00d4ff; margin-bottom: 1rem;">Your Machine</div>
    <div style="background: #161b22; border-radius: 8px; padding: 0.75rem 1rem; margin-bottom: 0.75rem; border-left: 3px solid #00d4ff;">
      <div style="font-size: 0.85rem; font-weight: 600; color: #e6edf3;">Context Window</div>
      <div style="font-size: 0.7rem; color: #8b949e; line-height: 1.6; margin-top: 0.25rem;">System prompt · CLAUDE.md · Conversation · Tool results</div>
    </div>
    <div style="background: #161b22; border-radius: 8px; padding: 0.75rem 1rem; margin-bottom: 0.75rem; border-left: 3px solid #7c3aed;">
      <div style="font-size: 0.85rem; font-weight: 600; color: #e6edf3;">Tools</div>
      <div style="display: flex; gap: 0.4rem; flex-wrap: wrap; margin-top: 0.35rem;">
        <span style="font-size: 0.65rem; background: rgba(124, 58, 237, 0.15); color: #a78bfa; padding: 0.15rem 0.5rem; border-radius: 4px;">Read</span>
        <span style="font-size: 0.65rem; background: rgba(124, 58, 237, 0.15); color: #a78bfa; padding: 0.15rem 0.5rem; border-radius: 4px;">Edit</span>
        <span style="font-size: 0.65rem; background: rgba(124, 58, 237, 0.15); color: #a78bfa; padding: 0.15rem 0.5rem; border-radius: 4px;">Bash</span>
        <span style="font-size: 0.65rem; background: rgba(124, 58, 237, 0.15); color: #a78bfa; padding: 0.15rem 0.5rem; border-radius: 4px;">Grep</span>
        <span style="font-size: 0.65rem; background: rgba(124, 58, 237, 0.15); color: #a78bfa; padding: 0.15rem 0.5rem; border-radius: 4px;">Glob</span>
        <span style="font-size: 0.65rem; background: rgba(124, 58, 237, 0.15); color: #a78bfa; padding: 0.15rem 0.5rem; border-radius: 4px;">Subagent</span>
      </div>
    </div>
    <div style="font-size: 0.7rem; color: #8b949e; line-height: 1.5;">Runs the loop · Executes tools · Manages context</div>
  </div>

  <div style="display: flex; flex-direction: column; justify-content: center; gap: 0.5rem; flex-shrink: 0;">
    <div style="text-align: center;">
      <div style="font-size: 0.65rem; color: #8b949e;">full context</div>
      <div style="color: #00d4ff; font-size: 1.5rem;">→</div>
    </div>
    <div style="text-align: center;">
      <div style="color: #7c3aed; font-size: 1.5rem;">←</div>
      <div style="font-size: 0.65rem; color: #8b949e;">tool call or reply</div>
    </div>
  </div>

  <div style="flex: 1; border: 1px solid rgba(124, 58, 237, 0.3); border-radius: 10px; padding: 1.25rem; background: rgba(124, 58, 237, 0.05);">
    <div style="font-size: 0.75rem; text-transform: uppercase; letter-spacing: 0.1em; color: #7c3aed; margin-bottom: 1rem;">Anthropic API</div>
    <div style="background: #161b22; border-radius: 8px; padding: 0.75rem 1rem; margin-bottom: 0.75rem; border-left: 3px solid #7c3aed;">
      <div style="font-size: 0.85rem; font-weight: 600; color: #e6edf3;">LLM Thinks</div>
      <div style="font-size: 0.7rem; color: #8b949e; line-height: 1.6; margin-top: 0.25rem;">Reads the full context.<br/>Decides: respond or call a tool?</div>
    </div>
    <div style="background: #161b22; border-radius: 8px; padding: 0.75rem 1rem; margin-bottom: 0.75rem; border-left: 3px solid #7c3aed;">
      <div style="font-size: 0.85rem; font-weight: 600; color: #e6edf3;">Returns</div>
      <div style="font-size: 0.7rem; color: #8b949e; line-height: 1.6; margin-top: 0.25rem;"><code style="color: #a78bfa; font-size: 0.65rem;">"call Bash: npm test"</code><br/><em>or</em> a text response to you</div>
    </div>
    <div style="font-size: 0.7rem; color: #8b949e; line-height: 1.5;"><strong style="color: #e6edf3;">Stateless.</strong> No memory between calls.</div>
  </div>
</div>

---

# The Context Window

Everything the agent knows lives in one context window. Here's what fills it:

<v-clicks>

- **System prompt** — built-in instructions from Anthropic (~2,800 tokens)
- **Tool definitions** — every tool the agent can call (~9,400 tokens)
- **CLAUDE.md** — your project rules, injected every session
- **Conversation** — your prompts + its responses + tool calls + file contents
- Performance **degrades as context fills** — Claude literally gets worse the more it's holding

</v-clicks>

---

# What That Looks Like

<div style="display: flex; gap: 2rem; align-items: center; margin-top: 0.5rem;">
  <div style="flex: 1;">
    <img src="/context-usage.png" style="border-radius: 10px; border: 1px solid rgba(139, 148, 158, 0.2); width: 100%;" />
    <div style="font-size: 0.65rem; color: #8b949e; margin-top: 0.4rem;"><code>/context</code> — real-time usage breakdown</div>
  </div>

  <div style="flex: 1.1;">
    <div style="font-size: 0.7rem; text-transform: uppercase; letter-spacing: 0.1em; color: #8b949e; margin-bottom: 0.5rem;">Where it comes from</div>
    <div style="display: flex; flex-direction: column; gap: 0.3rem;">
      <div style="display: flex; align-items: center; gap: 0.5rem;">
        <div style="width: 8px; height: 8px; border-radius: 2px; background: #8b949e; flex-shrink: 0;"></div>
        <div style="font-size: 0.7rem; color: #8b949e;">System prompt</div>
        <div style="flex: 1; border-bottom: 1px dashed rgba(139,148,158,0.2);"></div>
        <div style="font-size: 0.6rem; color: #8b949e;">Built-in</div>
      </div>
      <div style="display: flex; align-items: center; gap: 0.5rem;">
        <div style="width: 8px; height: 8px; border-radius: 2px; background: #8b949e; flex-shrink: 0;"></div>
        <div style="font-size: 0.7rem; color: #8b949e;">System tools</div>
        <div style="flex: 1; border-bottom: 1px dashed rgba(139,148,158,0.2);"></div>
        <div style="font-size: 0.6rem; color: #8b949e;">Built-in</div>
      </div>
      <div style="display: flex; align-items: center; gap: 0.5rem;">
        <div style="width: 8px; height: 8px; border-radius: 2px; background: #e65100; flex-shrink: 0;"></div>
        <div style="font-size: 0.7rem; color: #e6edf3;">Memory files</div>
        <div style="flex: 1; border-bottom: 1px dashed rgba(139,148,158,0.2);"></div>
        <div style="font-size: 0.6rem; color: #00d4ff;">~/.claude/CLAUDE.md</div>
      </div>
      <div style="display: flex; align-items: center; gap: 0.5rem;">
        <div style="width: 8px; height: 8px; border-radius: 2px; background: #e65100; flex-shrink: 0;"></div>
        <div style="font-size: 0.7rem; color: #e6edf3;">CLAUDE.md</div>
        <div style="flex: 1; border-bottom: 1px dashed rgba(139,148,158,0.2);"></div>
        <div style="font-size: 0.6rem; color: #00d4ff;">./CLAUDE.md</div>
      </div>
      <div style="display: flex; align-items: center; gap: 0.5rem;">
        <div style="width: 8px; height: 8px; border-radius: 2px; background: #c62828; flex-shrink: 0;"></div>
        <div style="font-size: 0.7rem; color: #e6edf3;">Skills</div>
        <div style="flex: 1; border-bottom: 1px dashed rgba(139,148,158,0.2);"></div>
        <div style="font-size: 0.6rem; color: #00d4ff;">.claude/skills/</div>
      </div>
      <div style="display: flex; align-items: center; gap: 0.5rem;">
        <div style="width: 8px; height: 8px; border-radius: 2px; background: #7c3aed; flex-shrink: 0;"></div>
        <div style="font-size: 0.7rem; color: #e6edf3;">Messages</div>
        <div style="flex: 1; border-bottom: 1px dashed rgba(139,148,158,0.2);"></div>
        <div style="font-size: 0.6rem; color: #8b949e;">Your conversation</div>
      </div>
      <div style="display: flex; align-items: center; gap: 0.5rem;">
        <div style="width: 8px; height: 8px; border-radius: 2px; background: transparent; border: 1px solid #8b949e; flex-shrink: 0;"></div>
        <div style="font-size: 0.7rem; color: #8b949e;">Free space</div>
        <div style="flex: 1; border-bottom: 1px dashed rgba(139,148,158,0.2);"></div>
        <div style="font-size: 0.6rem; color: #8b949e;">74% here</div>
      </div>
    </div>
    <div style="margin-top: 0.6rem; padding: 0.4rem 0.6rem; background: rgba(124, 58, 237, 0.1); border-radius: 6px; border-left: 3px solid #7c3aed;">
      <div style="font-size: 0.65rem; color: #a78bfa;">~26% consumed before you send a single message</div>
    </div>
  </div>
</div>

---

# Where Things Live on Disk

<div style="display: flex; gap: 2rem; margin-top: 0.75rem;">
  <div style="flex: 1; border: 1px solid rgba(0, 212, 255, 0.3); border-radius: 10px; padding: 1rem 1.25rem; background: rgba(0, 212, 255, 0.05);">
    <div style="font-size: 0.7rem; text-transform: uppercase; letter-spacing: 0.1em; color: #00d4ff; margin-bottom: 0.75rem;">~/.claude/ <span style="color: #8b949e; text-transform: none; letter-spacing: 0;">— your home directory</span></div>
    <div style="display: flex; flex-direction: column; gap: 0.4rem;">
      <div style="font-size: 0.8rem; color: #e6edf3;"><code style="color: #00d4ff; font-size: 0.7rem;">CLAUDE.md</code> — personal preferences, all projects</div>
      <div style="font-size: 0.8rem; color: #e6edf3;"><code style="color: #00d4ff; font-size: 0.7rem;">settings.json</code> — global permission rules</div>
      <div style="font-size: 0.8rem; color: #e6edf3;"><code style="color: #00d4ff; font-size: 0.7rem;">commands/</code> — your personal slash commands</div>
      <div style="font-size: 0.8rem; color: #e6edf3;"><code style="color: #00d4ff; font-size: 0.7rem;">memory/</code> — auto-memory across sessions</div>
    </div>
    <div style="margin-top: 0.6rem; font-size: 0.65rem; color: #8b949e;">Just you. Not checked in. Follows you across projects.</div>
  </div>

  <div style="flex: 1; border: 1px solid rgba(124, 58, 237, 0.3); border-radius: 10px; padding: 1rem 1.25rem; background: rgba(124, 58, 237, 0.05);">
    <div style="font-size: 0.7rem; text-transform: uppercase; letter-spacing: 0.1em; color: #7c3aed; margin-bottom: 0.75rem;">your-project/ <span style="color: #8b949e; text-transform: none; letter-spacing: 0;">— checked into git</span></div>
    <div style="display: flex; flex-direction: column; gap: 0.4rem;">
      <div style="font-size: 0.8rem; color: #e6edf3;"><code style="color: #a78bfa; font-size: 0.7rem;">CLAUDE.md</code> — team standards, shared context</div>
      <div style="font-size: 0.8rem; color: #e6edf3;"><code style="color: #a78bfa; font-size: 0.7rem;">.claude/settings.json</code> — project permission rules</div>
      <div style="font-size: 0.8rem; color: #e6edf3;"><code style="color: #a78bfa; font-size: 0.7rem;">.claude/commands/</code> — team slash commands</div>
      <div style="font-size: 0.8rem; color: #e6edf3;"><code style="color: #a78bfa; font-size: 0.7rem;">CLAUDE.local.md</code> — personal overrides <span style="font-size: 0.65rem; color: #8b949e;">(gitignored)</span></div>
    </div>
    <div style="margin-top: 0.6rem; font-size: 0.65rem; color: #8b949e;">Shared with your team. Evolves with the codebase.</div>
  </div>
</div>

---

# Not All Agents Are Equal

Same model, different wrapper, different results. Here's what actually varies:

<v-clicks>

- **Tool design** — dedicated tools for file editing, search, etc. vs. piping everything through a shell
- **Context management** — compaction, subagents, smart routing vs. filling the window until quality degrades
- **Persistent context** — CLAUDE.md, memory files, rules that survive across sessions vs. starting blank every time
- **Autonomy and self-correction** — can the agent see errors and retry, or is it one-shot?
- **Extensibility** — skills, hooks, custom commands that encode your workflows vs. just a chat box
- **Ecosystem** — MCP support, GitHub Actions, SDK access vs. closed and proprietary

</v-clicks>

---
layout: section
---

# Why Claude Code

---

# Why Claude Code

<v-clicks>

- **Opus 4.6 on subscription** — best model available, best value. Every other provider charges API pricing
- **Terminal version leads IDE** in features — IDE integrations are always lagging behind
- **Not the desktop app** — desktop Claude lacks skills, hooks, CLI power, and subagents
- **Open source** — you can read its source code. Use the agent to learn the agent's tooling
- **Developing at lightning speed** — major features shipping monthly
- Honest about gaps: Cloud needs GitHub, IDE integration still catching up

</v-clicks>

---
layout: section
---

# Claude Code 101

The things I wish I knew on day one

---

# Essential Commands

<v-clicks>

- **`/init`** — analyzes your codebase, generates a starter CLAUDE.md. Run it first
- **Plan mode** (`Shift+Tab` twice) — read-only research and planning. Start here for non-trivial tasks
- **`/clear`** — flush context between unrelated tasks. "The single most underused command"
- **`/compact`** — compress conversation when context grows. Add focus: `/compact Focus on the API changes`
- **`@`** — reference files directly in prompts. `@src/auth.ts` beats describing where code lives
- **Subagents** — isolated sub-sessions for investigation. Keeps your main context clean

</v-clicks>

---

# Plan Mode

`Shift+Tab` cycles through three modes:

<div style="display: flex; gap: 1rem; margin-top: 1rem;">
  <div style="flex: 1; border: 1px solid rgba(139, 148, 158, 0.2); border-radius: 10px; padding: 1rem; background: rgba(139, 148, 158, 0.05);">
    <div style="font-size: 0.9rem; font-weight: 700; color: #e6edf3;">Normal</div>
    <div style="font-size: 0.7rem; color: #8b949e; margin-top: 0.3rem;">You approve each action before it runs</div>
  </div>
  <div style="flex-shrink: 0; display: flex; align-items: center; color: #8b949e;">→</div>
  <div style="flex: 1; border: 1px solid rgba(139, 148, 158, 0.2); border-radius: 10px; padding: 1rem; background: rgba(139, 148, 158, 0.05);">
    <div style="font-size: 0.9rem; font-weight: 700; color: #e6edf3;">Auto-Accept</div>
    <div style="font-size: 0.7rem; color: #8b949e; margin-top: 0.3rem;">Agent runs autonomously — you watch</div>
  </div>
  <div style="flex-shrink: 0; display: flex; align-items: center; color: #8b949e;">→</div>
  <div style="flex: 1; border: 1px solid rgba(0, 212, 255, 0.3); border-radius: 10px; padding: 1rem; background: rgba(0, 212, 255, 0.08);">
    <div style="font-size: 0.9rem; font-weight: 700; color: #00d4ff;">Plan Mode</div>
    <div style="font-size: 0.7rem; color: #8b949e; margin-top: 0.3rem;">Read-only. No changes. Thinking only.</div>
  </div>
</div>

<div style="margin-top: 1rem; font-size: 0.95rem;">Start most sessions in <strong style="color: #00d4ff;">Plan Mode</strong>. Iterate on the plan. Then switch to execute.</div>

---
layout: section
---

# The #1 Tip

The one thing every source agrees on

---

# Give Claude a Way to Verify Its Work

<div style="display: flex; gap: 2rem; margin-top: 0.5rem;">
  <div style="flex: 1; display: flex; flex-direction: column; gap: 0.5rem;">
    <div style="font-size: 1rem; color: #e6edf3; line-height: 1.7;">Anthropic internal data: <strong>2-3x quality improvement</strong> with a verification loop.</div>
    <div style="font-size: 0.9rem; color: #8b949e; line-height: 1.7;">Appeared in <strong style="color: #e6edf3;">every source</strong> we researched — consensus #1.</div>
    <div style="font-size: 0.9rem; color: #8b949e; line-height: 1.7;">Without it, "the tests pass" is a <strong style="color: #e6edf3;">prediction</strong>, not a report.</div>
    <div style="font-size: 0.9rem; color: #8b949e; line-height: 1.7;">Tests, bash commands, screenshots — the more ways it can <strong style="color: #e6edf3;">see its own work</strong>, the better.</div>
  </div>

  <div style="flex: 1;">
    <div style="font-size: 0.65rem; text-transform: uppercase; letter-spacing: 0.1em; color: #8b949e; margin-bottom: 0.5rem;">The feedback loop in action:</div>
    <div style="background: #161b22; border-radius: 8px; padding: 0.7rem 0.9rem; border-left: 3px solid #00d4ff; font-family: 'JetBrains Mono', monospace; font-size: 0.65rem; line-height: 1.8;">
      <div style="color: #8b949e;">⠋ Writing tests...</div>
      <div style="color: #e6edf3;">$ npm test</div>
      <div style="color: #f87171;">✗ 3 failing</div>
      <div style="color: #8b949e; margin-top: 0.3rem;">⠋ Implementing...</div>
      <div style="color: #e6edf3;">$ npm test</div>
      <div style="color: #4ade80;">✓ 3 passing</div>
      <div style="color: #8b949e; margin-top: 0.3rem;">⠋ Checking the page...</div>
      <div style="color: #e6edf3;">$ browser screenshot localhost:3000</div>
      <div style="color: #e6edf3;">📸 Form renders, but submit button is</div>
      <div style="color: #e6edf3;">   clipped below the fold</div>
      <div style="color: #8b949e; margin-top: 0.3rem;">⠋ Fixing layout...</div>
      <div style="color: #e6edf3;">$ browser screenshot localhost:3000</div>
      <div style="color: #4ade80;">✓ Form visible, button accessible</div>
    </div>
  </div>
</div>

---
layout: section
---

# Set Yourself Up Right

---

# CLAUDE.md

A shared configuration file committed to your repository.

<!-- TODO:
  - Run /init to generate starter
  - Prune ruthlessly — under 200 lines. For each line: "Would removing this cause mistakes?"
  - Check it into git — this is team infrastructure, not personal config
  - What goes in: build/test/lint commands, code style, architectural patterns, libraries to use/avoid
  - What doesn't: personal preferences (those go in CLAUDE.local.md, gitignored), rules the linter enforces -->

---

# Work On It As a Team

<!-- TODO: Cherny's team practices:
  - After every correction: "Update your CLAUDE.md so you don't make that mistake again"
  - Tag coworkers' PRs with @.claude to add learnings
  - Claude suggests CLAUDE.md improvements after sessions
  - The correction → rule loop: institutional memory that compounds
  - Commit CLAUDE.md updates alongside the code changes that prompted them -->

---

# Permission Whitelist

The agent runs shell commands by default. You decide which ones.

<!-- TODO:
  - Deny rules: .env, ~/.ssh/, credentials, secrets
  - Approve explicitly: Deny > Allow > Ask
  - Audit frequently — review as the project evolves
  - Never run with --dangerously-skip-permissions outside containers
  - This is your security baseline — set it up on day one, not after an incident -->

---

# Your First Hour Checklist

<!-- TODO:
  1. Install Claude Code
  2. cd your-project && claude
  3. Run /init
  4. Prune CLAUDE.md to under 200 lines
  5. Learn Shift+Tab (Normal → Auto-Accept → Plan Mode)
  6. Run /permissions to configure your whitelist
  7. Start in Plan Mode for your first real task
  8. After each task: /clear
  9. When Claude makes a mistake: correct it, then "Update your CLAUDE.md"
  10. Always give Claude verification: "Run the tests after implementing" -->

---

# Go Try It

Your homework for the next two weeks:

- Install Claude Code and set up your project
- Create your CLAUDE.md and check it in
- Configure your permission whitelist
- Try plan mode on a real task
- **Come back ready to share what you noticed**

---

# Sources

<!-- TODO: Sources table -->

---
layout: section
---

# Questions?

Talk 1 of the Agentic Coding Best Practices series
