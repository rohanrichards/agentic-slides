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

# This Series

Four talks over eight weeks. Each one builds on the last.

<v-clicks>

- **Talk 1** — Getting Started Right *(you are here)*
- **Talk 2** — The Pitfalls — And Your First Defenses
- **Talk 3** — The Workflow
- **Talk 4** — Levelling Up

</v-clicks>

<!--
Open with a quick audience check: "Who has used an AI coding tool? Who has used Claude Code specifically?"
Use this to calibrate the room. Mixed levels expected — some at zero, some tinkering.
This series is fortnightly. Between talks, you'll be using these tools on real work.
-->

---
layout: section
---

# Why Should You Care?

Left unsupervised, an AI coding agent will **confidently build the wrong thing**.

This series teaches you the guardrails.

<!--
This is the single most important framing for the whole series. Every source we researched — Anthropic, Karpathy, Willison, Cherny — agrees: agents are powerful but need human direction.
The payoff: Anthropic's internal data shows verification loops lead to 2-3x faster execution. That's the difference between the tool helping and the tool wasting your time.
-->

---
layout: section
---

# Anatomy of an Agent

What's actually happening under the hood

---

# What Is an Agent?

Not autocomplete. It reads, writes, runs, and iterates — autonomously.

<v-clicks>

- It has **tools** — reads, edits, runs commands
- It works in a **loop** — act, observe, decide, repeat
- It manages its own **context** — what to read, when to delegate
- Unsupervised, it will **confidently build the wrong thing**

</v-clicks>

<!--
Key distinction: autocomplete suggests the next line. An agent takes a goal and works toward it autonomously — reading files, running commands, checking results, and iterating.

"Tools" includes file reads/edits, bash commands, web search, and spawning sub-sessions (subagents).

"Context" is everything the agent knows — we'll explain this in the next few slides.

The last bullet is the reason this series exists. Agents are confident even when wrong.
-->

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
  </div>

  <div style="display: flex; flex-direction: column; justify-content: center; align-items: center; gap: 0.3rem; flex-shrink: 0; min-width: 5rem;">
    <div style="font-size: 0.65rem; color: #8b949e;">full context</div>
    <div style="color: #00d4ff; font-size: 1.5rem;">→</div>
    <div style="border: 2px solid rgba(124, 58, 237, 0.4); border-radius: 50%; width: 3.5rem; height: 3.5rem; display: flex; align-items: center; justify-content: center; margin: 0.3rem 0;">
      <div style="font-size: 0.55rem; font-weight: 700; color: #a78bfa; text-align: center; line-height: 1.2;">LOOP<br/>&#x21bb;</div>
    </div>
    <div style="color: #7c3aed; font-size: 1.5rem;">←</div>
    <div style="font-size: 0.65rem; color: #8b949e;">tool call or reply</div>
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

<!--
The diagram tells the story: your machine assembles all context, sends it to the API, gets back either a tool call or a text response. The loop repeats until the agent is done or you interrupt.

Key insight: the API is stateless. It has no memory between calls. The illusion of continuity comes from the client (your machine) replaying the full conversation each time.

"Runs the loop, executes tools, manages context" — that's what the left box does. The right box just thinks and returns.
-->

---

# The Context Window

Everything the agent knows lives in one window. Here's what fills it:

<v-clicks>

- **System prompt** — built-in instructions from Anthropic
- **Tool definitions** — every tool the agent can call
- **CLAUDE.md** — your project rules, loaded every session
- **Conversation** — your prompts, responses, file contents
- Performance **degrades** as it fills up

</v-clicks>

<!--
Token counts (for reference, don't show on slide):
- System prompt: ~2,600 tokens
- Tool definitions: ~17,600 tokens
- These numbers shift between versions

The key takeaway for the audience: there's a budget. The more stuff in the window, the worse the agent performs. This is why /clear and /compact exist — context hygiene matters.

With the 1M token context window (default since March 2026), overhead is only ~5% before your first message. But degradation still happens in long sessions.
-->

---

# What That Looks Like

<div style="display: flex; gap: 2rem; align-items: center; margin-top: 0.5rem;">
  <v-click>
  <div style="flex: 1;">
    <img src="/context-usage.png" style="border-radius: 10px; border: 1px solid rgba(139, 148, 158, 0.2); width: 100%;" />
    <v-click>
    <div style="font-size: 0.65rem; color: #8b949e; margin-top: 0.4rem;"><code>/context</code> — real-time usage breakdown</div>
    </v-click>
  </div>
  </v-click>

  <div style="flex: 1.1;">
    <v-click>
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
        <div style="width: 8px; height: 8px; border-radius: 2px; background: #7c3aed; flex-shrink: 0;"></div>
        <div style="font-size: 0.7rem; color: #e6edf3;">Messages</div>
        <div style="flex: 1; border-bottom: 1px dashed rgba(139,148,158,0.2);"></div>
        <div style="font-size: 0.6rem; color: #8b949e;">Your conversation</div>
      </div>
      <div style="display: flex; align-items: center; gap: 0.5rem;">
        <div style="width: 8px; height: 8px; border-radius: 2px; background: transparent; border: 1px solid #8b949e; flex-shrink: 0;"></div>
        <div style="font-size: 0.7rem; color: #8b949e;">Free space</div>
        <div style="flex: 1; border-bottom: 1px dashed rgba(139,148,158,0.2);"></div>
        <div style="font-size: 0.6rem; color: #8b949e;">Most of it</div>
      </div>
    </div>
    </v-click>
    <v-click>
    <div style="margin-top: 0.6rem; padding: 0.4rem 0.6rem; background: rgba(124, 58, 237, 0.1); border-radius: 6px; border-left: 3px solid #7c3aed;">
      <div style="font-size: 0.65rem; color: #a78bfa;">~5-10% consumed before you send a single message</div>
    </div>
    </v-click>
  </div>
</div>

<!--
The screenshot is from the /context command — you can run this anytime to see your context usage.

With the 1M token window (default since March 2026), overhead is only ~5-10% before your first message. The old 200K window had ~26% overhead. Either way, the principle holds: context is a budget.

The "Skills" row was removed from the legend because it's a Talk 4 topic. Keep it simple here.
-->

---

# Where Things Live on Disk

<div style="display: flex; gap: 1.5rem; margin-top: 0.4rem;">
  <v-click>
  <div style="flex: 1;">
    <div style="font-size: 0.65rem; text-transform: uppercase; letter-spacing: 0.1em; color: #00d4ff; margin-bottom: 0.5rem;">~/.claude/ <span style="color: #8b949e; text-transform: none; letter-spacing: 0;">— your home directory</span></div>
    <div style="display: flex; gap: 0.35rem; margin-bottom: 0.4rem;">
      <div style="flex: 1; background: #161b22; border: 1px solid rgba(0, 212, 255, 0.25); border-radius: 6px; padding: 0.35rem 0.5rem;">
        <div style="font-size: 0.55rem; color: #00d4ff;">CLAUDE.md</div>
        <div style="font-size: 0.5rem; color: #8b949e;">personal prefs</div>
      </div>
      <div style="flex: 1; background: #161b22; border: 1px solid rgba(0, 212, 255, 0.25); border-radius: 6px; padding: 0.35rem 0.5rem;">
        <div style="font-size: 0.55rem; color: #00d4ff;">settings.json</div>
        <div style="font-size: 0.5rem; color: #8b949e;">global permissions</div>
      </div>
    </div>
    <div style="display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 0.3rem;">
      <div style="background: rgba(0, 212, 255, 0.08); border: 1px solid rgba(0, 212, 255, 0.2); border-radius: 5px; padding: 0.3rem 0.45rem;">
        <div style="font-size: 0.5rem; color: #8b949e; margin-bottom: 0.1rem;">&#128193;</div>
        <div style="font-size: 0.55rem; color: #00d4ff; font-weight: 600;">commands/</div>
        <div style="font-size: 0.45rem; color: #8b949e;">slash commands</div>
      </div>
      <div style="background: rgba(0, 212, 255, 0.08); border: 1px solid rgba(0, 212, 255, 0.2); border-radius: 5px; padding: 0.3rem 0.45rem;">
        <div style="font-size: 0.5rem; color: #8b949e; margin-bottom: 0.1rem;">&#128193;</div>
        <div style="font-size: 0.55rem; color: #00d4ff; font-weight: 600;">rules/</div>
        <div style="font-size: 0.45rem; color: #8b949e;">personal rules</div>
      </div>
      <div style="background: rgba(0, 212, 255, 0.08); border: 1px solid rgba(0, 212, 255, 0.2); border-radius: 5px; padding: 0.3rem 0.45rem;">
        <div style="font-size: 0.5rem; color: #8b949e; margin-bottom: 0.1rem;">&#128193;</div>
        <div style="font-size: 0.55rem; color: #00d4ff; font-weight: 600;">skills/</div>
        <div style="font-size: 0.45rem; color: #8b949e;">personal skills</div>
      </div>
      <div style="background: rgba(0, 212, 255, 0.08); border: 1px solid rgba(0, 212, 255, 0.2); border-radius: 5px; padding: 0.3rem 0.45rem;">
        <div style="font-size: 0.5rem; color: #8b949e; margin-bottom: 0.1rem;">&#128193;</div>
        <div style="font-size: 0.55rem; color: #00d4ff; font-weight: 600;">agents/</div>
        <div style="font-size: 0.45rem; color: #8b949e;">custom subagents</div>
      </div>
      <div style="background: rgba(0, 212, 255, 0.08); border: 1px solid rgba(0, 212, 255, 0.2); border-radius: 5px; padding: 0.3rem 0.45rem;">
        <div style="font-size: 0.5rem; color: #8b949e; margin-bottom: 0.1rem;">&#128193;</div>
        <div style="font-size: 0.55rem; color: #00d4ff; font-weight: 600;">hooks/</div>
        <div style="font-size: 0.45rem; color: #8b949e;">automation</div>
      </div>
      <div style="background: rgba(0, 212, 255, 0.08); border: 1px solid rgba(0, 212, 255, 0.2); border-radius: 5px; padding: 0.3rem 0.45rem;">
        <div style="font-size: 0.5rem; color: #8b949e; margin-bottom: 0.1rem;">&#128193;</div>
        <div style="font-size: 0.55rem; color: #00d4ff; font-weight: 600;">memory/</div>
        <div style="font-size: 0.45rem; color: #8b949e;">cross-session</div>
      </div>
    </div>
    <div style="margin-top: 0.4rem; font-size: 0.55rem; color: #8b949e;">Just you. Not checked in. Follows you across projects.</div>
  </div>
  </v-click>

  <v-click>
  <div style="flex: 1;">
    <div style="font-size: 0.65rem; text-transform: uppercase; letter-spacing: 0.1em; color: #7c3aed; margin-bottom: 0.5rem;">your-project/ <span style="color: #8b949e; text-transform: none; letter-spacing: 0;">— checked into git</span></div>
    <div style="display: flex; gap: 0.35rem; margin-bottom: 0.4rem;">
      <div style="flex: 1; background: #161b22; border: 1px solid rgba(124, 58, 237, 0.25); border-radius: 6px; padding: 0.35rem 0.5rem;">
        <div style="font-size: 0.55rem; color: #a78bfa;">CLAUDE.md</div>
        <div style="font-size: 0.5rem; color: #8b949e;">team standards</div>
      </div>
      <div style="flex: 1; background: #161b22; border: 1px solid rgba(124, 58, 237, 0.25); border-radius: 6px; padding: 0.35rem 0.5rem;">
        <div style="font-size: 0.55rem; color: #a78bfa;">CLAUDE.local.md</div>
        <div style="font-size: 0.5rem; color: #8b949e;">personal <span style="color: #6b7280;">(gitignored)</span></div>
      </div>
      <div style="flex: 1; background: #161b22; border: 1px solid rgba(124, 58, 237, 0.25); border-radius: 6px; padding: 0.35rem 0.5rem;">
        <div style="font-size: 0.55rem; color: #a78bfa;">.mcp.json</div>
        <div style="font-size: 0.5rem; color: #8b949e;">MCP servers</div>
      </div>
    </div>
    <div style="border: 1px solid rgba(124, 58, 237, 0.25); border-radius: 7px; padding: 0.4rem; background: rgba(124, 58, 237, 0.04);">
      <div style="font-size: 0.55rem; color: #7c3aed; font-weight: 600; margin-bottom: 0.3rem; padding-left: 0.1rem;">&#128193; .claude/</div>
      <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 0.3rem;">
        <div style="background: #161b22; border: 1px solid rgba(124, 58, 237, 0.2); border-radius: 5px; padding: 0.3rem 0.45rem;">
          <div style="font-size: 0.55rem; color: #a78bfa; font-weight: 600;">settings.json</div>
          <div style="font-size: 0.45rem; color: #8b949e;">project permissions</div>
        </div>
        <div style="background: #161b22; border: 1px solid rgba(124, 58, 237, 0.2); border-radius: 5px; padding: 0.3rem 0.45rem;">
          <div style="font-size: 0.55rem; color: #a78bfa; font-weight: 600;">settings.local.json</div>
          <div style="font-size: 0.45rem; color: #8b949e;">local overrides</div>
        </div>
      </div>
      <div style="display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 0.25rem; margin-top: 0.3rem;">
        <div style="background: rgba(124, 58, 237, 0.08); border: 1px solid rgba(124, 58, 237, 0.2); border-radius: 5px; padding: 0.25rem 0.4rem;">
          <div style="font-size: 0.5rem; color: #8b949e; margin-bottom: 0.05rem;">&#128193;</div>
          <div style="font-size: 0.5rem; color: #a78bfa; font-weight: 600;">commands/</div>
        </div>
        <div style="background: rgba(124, 58, 237, 0.08); border: 1px solid rgba(124, 58, 237, 0.2); border-radius: 5px; padding: 0.25rem 0.4rem;">
          <div style="font-size: 0.5rem; color: #8b949e; margin-bottom: 0.05rem;">&#128193;</div>
          <div style="font-size: 0.5rem; color: #a78bfa; font-weight: 600;">rules/</div>
        </div>
        <div style="background: rgba(124, 58, 237, 0.08); border: 1px solid rgba(124, 58, 237, 0.2); border-radius: 5px; padding: 0.25rem 0.4rem;">
          <div style="font-size: 0.5rem; color: #8b949e; margin-bottom: 0.05rem;">&#128193;</div>
          <div style="font-size: 0.5rem; color: #a78bfa; font-weight: 600;">skills/</div>
        </div>
        <div style="background: rgba(124, 58, 237, 0.08); border: 1px solid rgba(124, 58, 237, 0.2); border-radius: 5px; padding: 0.25rem 0.4rem;">
          <div style="font-size: 0.5rem; color: #8b949e; margin-bottom: 0.05rem;">&#128193;</div>
          <div style="font-size: 0.5rem; color: #a78bfa; font-weight: 600;">agents/</div>
        </div>
        <div style="background: rgba(124, 58, 237, 0.08); border: 1px solid rgba(124, 58, 237, 0.2); border-radius: 5px; padding: 0.25rem 0.4rem;">
          <div style="font-size: 0.5rem; color: #8b949e; margin-bottom: 0.05rem;">&#128193;</div>
          <div style="font-size: 0.5rem; color: #a78bfa; font-weight: 600;">hooks/</div>
        </div>
      </div>
    </div>
    <div style="margin-top: 0.4rem; font-size: 0.55rem; color: #8b949e;">Shared with your team. Evolves with the codebase.</div>
  </div>
  </v-click>
</div>

<!--
Two locations to know:
1. ~/.claude/ — your personal config. Not checked into git. Follows you across every project.
2. your-project/.claude/ — project-level config. Checked into git. Shared with your team.

The key files to remember: CLAUDE.md (instructions) and settings.json (permissions). Everything else (commands, rules, skills, agents, hooks) is for later talks.

Don't worry about memorizing the folder structure — you'll learn it naturally as you use the tool.
-->

---

# Not All Agents Are Equal

Same model, different wrapper, different results.

<v-clicks>

- **Tool design** — dedicated tools vs. everything through a shell
- **Context management** — smart compaction vs. filling until quality drops
- **Persistent memory** — rules that survive across sessions
- **Self-correction** — can it see errors and retry?
- **Extensibility** — encode your workflows, not just chat
- **Ecosystem** — integrations, plugins, SDK access

</v-clicks>

<!--
This is why "just use ChatGPT" misses the point. The model matters, but the harness around it matters just as much. One coding agent improved from Top 30 to Top 5 on a benchmark by changing only the harness, not the model.

Tool design: Claude Code has dedicated Read, Edit, Grep tools. Some agents pipe everything through a single shell command.
Context management: Claude Code compacts automatically, spawns subagents for isolation.
Persistent memory: CLAUDE.md, memory files, rules — your project knowledge persists.
Self-correction: Claude Code sees errors, retries, iterates. Not one-shot.
Extensibility: Skills, hooks, custom commands — Talk 4 covers this.
Ecosystem: MCP servers, GitHub Actions, Agent SDK.
-->

---
layout: section
---

# Why Claude Code

---

# Why Claude Code

<v-clicks>

- **Opus 4.6 on subscription** — strong model, flat-rate pricing for heavy usage
- **Terminal-first** — CLI gets features first, IDE extensions follow
- **Source-available** — you can read the source code on GitHub
- **Shipping fast** — major features landing monthly
- **Not the desktop app** — desktop Claude is improving but lacks hooks and CLI extensibility
- Honest about gaps: Cloud needs GitHub, IDE integration still catching up

</v-clicks>

<!--
Competitive landscape (be honest if asked):
- OpenAI Codex CLI is included in ChatGPT Plus/Pro subscriptions
- Gemini CLI is free with a generous free tier and fully open source (Apache 2.0)
- GitHub Copilot has agent mode in Pro/Pro+ tiers
- Cursor offers Pro at $20/mo with frontier model access

Claude Code's edge: the extensibility model (skills, hooks, agents, MCP) is the most developed. The correction→rule loop is unique. But it's not the only option, and the others are catching up.

"Source-available" not "open source" — it's under Anthropic's commercial terms, not an OSI license. You can read every line of code, but it's not OSI open source. Gemini CLI and Codex CLI are actually OSI open source.
-->

---
layout: section
---

# Claude Code 101

The things I wish I knew on day one

---

# Starting a Session

<div style="display: flex; gap: 2rem; margin-top: 0.75rem;">
  <div style="flex: 1; display: flex; flex-direction: column; gap: 0.6rem;">
    <v-clicks>
      <div style="font-size: 0.9rem; color: #e6edf3; line-height: 1.6;"><strong><code style="color: #00d4ff;">/init</code></strong> — generates a starter CLAUDE.md for your project</div>
      <div style="font-size: 0.9rem; color: #e6edf3; line-height: 1.6;"><strong>Plan mode</strong> (<code style="color: #00d4ff;">Shift+Tab</code> twice) — read-only thinking, no edits</div>
      <div style="font-size: 0.9rem; color: #e6edf3; line-height: 1.6;"><strong><code style="color: #00d4ff;">/clear</code></strong> — flush context between tasks</div>
    </v-clicks>
  </div>
  <div style="flex: 1;">
    <TerminalBlock title="first session">
      <div style="font-size: 0.75rem; line-height: 1.8;">
        <span style="color: #8b949e;">$</span> <span style="color: #e6edf3;">cd my-project && claude</span><br/>
        <span style="color: #e6edf3;">> /init</span><br/>
        <span style="color: #8b949e;">⠋ Analyzing codebase...</span><br/>
        <span style="color: #4ade80;">✓ Generated CLAUDE.md (142 lines)</span><br/>
        <span style="color: #e6edf3;">> Shift+Tab → Plan Mode</span>
      </div>
    </TerminalBlock>
  </div>
</div>

<!--
/init: Analyzes your codebase structure, dependencies, and patterns. Generates a CLAUDE.md with build commands, conventions, and project context. Run it first, then prune — it tends to be verbose.

Plan mode: Removes all write tools so the agent literally can't make edits. Pure thinking. Plans are written to special plan files that survive compaction. Start here for anything non-trivial.

/clear: Flushes the entire conversation context. Use between unrelated tasks. Context contamination (leftover context from a previous task confusing the current one) is the most common source of weird agent behavior.
-->

---

# Working Smart

<div style="display: flex; gap: 2rem; margin-top: 0.75rem;">
  <div style="flex: 1; display: flex; flex-direction: column; gap: 0.6rem;">
    <v-clicks>
      <div style="font-size: 0.9rem; color: #e6edf3; line-height: 1.6;"><strong><code style="color: #00d4ff;">@</code></strong> — add a specific file to context directly</div>
      <div style="font-size: 0.9rem; color: #e6edf3; line-height: 1.6;"><strong><code style="color: #00d4ff;">/compact</code></strong> — compress conversation to free up context</div>
      <div style="font-size: 0.9rem; color: #e6edf3; line-height: 1.6;"><strong>Subagents</strong> — isolated sub-sessions that keep your main context clean</div>
    </v-clicks>
  </div>
  <div style="flex: 1;">
    <TerminalBlock title="during work">
      <div style="font-size: 0.75rem; line-height: 1.8;">
        <span style="color: #8b949e;">You type:</span><br/>
        <span style="color: #e6edf3;">> @src/auth.ts explain the token</span><br/>
        <span style="color: #e6edf3;">  refresh logic</span><br/>
        <br/>
        <span style="color: #8b949e;">You type:</span><br/>
        <span style="color: #e6edf3;">> /compact Focus on the API changes</span><br/>
        <br/>
        <span style="color: #8b949e;">You type:</span><br/>
        <span style="color: #e6edf3;">> Use a subagent to implement the</span><br/>
        <span style="color: #e6edf3;">  validation layer</span>
      </div>
    </TerminalBlock>
  </div>
</div>

<!--
@: Injects a specific file into the conversation. Useful when you know exactly what the agent needs to see. Faster than letting it search.

/compact: With the 1M context window, this is less critical than it used to be. Still useful for very long sessions. Add focus to tell it what to preserve: /compact Focus on the API changes.

Subagents: The agent can spawn isolated sub-sessions to investigate or implement something without polluting your main conversation. Think of it as delegation — the subagent does the work and reports back.
-->

---

# Plan Mode

`Shift+Tab` cycles through three modes:

<div style="display: flex; gap: 1rem; margin-top: 1rem;">
  <v-click>
  <div style="flex: 1; border: 1px solid rgba(139, 148, 158, 0.2); border-radius: 10px; padding: 1rem; background: rgba(139, 148, 158, 0.05);">
    <div style="font-size: 0.9rem; font-weight: 700; color: #e6edf3;">Normal</div>
    <div style="font-size: 0.7rem; color: #8b949e; margin-top: 0.3rem;">You approve each action before it runs</div>
  </div>
  </v-click>
  <v-click>
  <div style="flex-shrink: 0; display: flex; align-items: center; color: #8b949e;">→</div>
  <div style="flex: 1; border: 1px solid rgba(139, 148, 158, 0.2); border-radius: 10px; padding: 1rem; background: rgba(139, 148, 158, 0.05);">
    <div style="font-size: 0.9rem; font-weight: 700; color: #e6edf3;">Auto-Accept</div>
    <div style="font-size: 0.7rem; color: #8b949e; margin-top: 0.3rem;">Agent runs autonomously — you watch</div>
  </div>
  </v-click>
  <v-click>
  <div style="flex-shrink: 0; display: flex; align-items: center; color: #8b949e;">→</div>
  <div style="flex: 1; border: 1px solid rgba(0, 212, 255, 0.3); border-radius: 10px; padding: 1rem; background: rgba(0, 212, 255, 0.08);">
    <div style="font-size: 0.9rem; font-weight: 700; color: #00d4ff;">Plan Mode</div>
    <div style="font-size: 0.7rem; color: #8b949e; margin-top: 0.3rem;">Read-only. No changes. Thinking only.</div>
  </div>
  </v-click>
</div>

<v-click>
<div style="margin-top: 1rem; font-size: 0.95rem;">Start most sessions in <strong style="color: #00d4ff;">Plan Mode</strong>. Iterate on the plan. Then switch to execute.</div>
</v-click>

---
layout: section
---

# The #1 Tip

The one thing every source agrees on

---

# Give Claude a Way to Verify Its Work

<v-clicks>

<div style="display: flex; gap: 2rem; margin-top: 0.5rem;">
  <div style="flex: 1; display: flex; flex-direction: column; gap: 0.5rem;">
    <div style="font-size: 1rem; color: #e6edf3; line-height: 1.7;">Anthropic's single biggest quality multiplier.</div>
    <div style="font-size: 0.9rem; color: #8b949e; line-height: 1.7;">Without verification, <strong style="color: #e6edf3;">"the tests pass"</strong> is a prediction, not a report.</div>
  </div>

  <div style="flex: 1;">
    <TerminalBlock title="the feedback loop">
      <div style="font-size: 0.65rem; line-height: 1.8;">
        <span style="color: #8b949e;">⠋ Writing tests...</span><br/>
        <span style="color: #e6edf3;">$ npm test</span><br/>
        <span style="color: #f87171;">✗ 3 failing</span><br/>
        <br/>
        <span style="color: #8b949e;">⠋ Implementing...</span><br/>
        <span style="color: #e6edf3;">$ npm test</span><br/>
        <span style="color: #4ade80;">✓ 3 passing</span><br/>
      </div>
    </TerminalBlock>
  </div>
</div>

</v-clicks>

<!--
Every source we researched agrees: verification is the single highest-leverage practice.

Anthropic's internal data shows verification loops lead to 2-3x faster execution. Not quality — speed. Because when the agent can see its own mistakes, it fixes them in the same session instead of you discovering them later.

The more ways it can see its own work, the better: tests, bash commands, browser screenshots, linter output. Tell it "run the tests after each step" and quality jumps immediately.

The terminal example shows the core loop: write tests → they fail → implement → tests pass. The agent sees the failure and iterates. Without this, it would just claim "done" and you'd find the bugs yourself.
-->

---
layout: section
---

# Set Yourself Up Right

---

# CLAUDE.md

Persistent instructions loaded every session. Keep it concise.

<div style="display: flex; gap: 1.5rem; margin-top: 0.75rem;">
  <v-click>
  <div style="flex: 1; border: 1px solid rgba(0, 212, 255, 0.3); border-radius: 10px; padding: 0.9rem 1.1rem; background: rgba(0, 212, 255, 0.05);">
    <div style="font-size: 0.7rem; text-transform: uppercase; letter-spacing: 0.1em; color: #4ade80; margin-bottom: 0.6rem;">What belongs</div>
    <div style="display: flex; flex-direction: column; gap: 0.35rem; font-size: 0.8rem; color: #e6edf3; line-height: 1.5;">
      <div><strong>Corrections</strong> — things Claude gets wrong</div>
      <div><strong>Non-obvious conventions</strong></div>
      <div><strong>Architectural decisions</strong></div>
    </div>
  </div>
  </v-click>

  <v-click>
  <div style="flex: 1; border: 1px solid rgba(139, 148, 158, 0.2); border-radius: 10px; padding: 0.9rem 1.1rem; background: rgba(139, 148, 158, 0.05);">
    <div style="font-size: 0.7rem; text-transform: uppercase; letter-spacing: 0.1em; color: #f87171; margin-bottom: 0.6rem;">What doesn't</div>
    <div style="display: flex; flex-direction: column; gap: 0.35rem; font-size: 0.8rem; color: #8b949e; line-height: 1.5;">
      <div>Things Claude already knows</div>
      <div>Rules your linter enforces</div>
      <div>Detailed docs — link with <code style="font-size: 0.7rem;">@</code></div>
    </div>
  </div>
  </v-click>
</div>

<!--
CLAUDE.md is loaded into every session — it's premium context space. Anthropic recommends keeping it under 300 lines (shorter is better).

What belongs: Corrections (things Claude gets wrong for YOUR project), non-obvious conventions ("use pnpm not npm", "we use Zustand not Redux"), architectural decisions (prevents competing patterns), things to avoid (libraries that look right but aren't), and pointers to docs (@README.md, @docs/architecture.md).

What doesn't belong: Build/test/lint commands already in package.json, standard language conventions Claude already knows, rules your linter already enforces, detailed API docs (link with @ instead), personal preferences (use CLAUDE.local.md for those — it's gitignored).

For path-specific rules, use .claude/rules/ with paths frontmatter — rules only load when Claude touches matching files. That's an advanced topic for later.
-->

---

# The Correction → Rule Loop

Every mistake becomes a rule. CLAUDE.md is built through use.

<v-clicks>

- Claude makes a mistake → you correct it → **"update CLAUDE.md so you don't do that again"**
- Claude drafts the rule, you review it
- Commit alongside the code change
- Your whole team benefits — review CLAUDE.md changes in PRs

</v-clicks>

<!--
This is how the Anthropic team works on Claude Code itself. Boris Cherny (Claude Code creator) calls Claude "eerily good" at writing rules for itself.

The loop: mistake → correction → rule → commit. Over time, Claude's error rate noticeably drops for your project because it has a growing set of project-specific instructions.

The team benefit: CLAUDE.md is checked into git. When someone adds a rule, everyone gets it. Review CLAUDE.md changes in PRs like any other code.
-->

---

# Permissions

Claude asks before every command. Your allowlist grows as you work.

<v-clicks>

- **"Yes, and always allow"** — builds your allowlist naturally
- Be sensible — test runners yes, arbitrary curl maybe not
- **Watch for wildcards** — `Bash(npm run *)` is fine, `Bash(ssh *)` maybe not
- Store in `.claude/settings.json` and **check into git**

</v-clicks>

<!--
Every permission dialogue offers "yes, and always allow" — so your allowlist grows naturally as you work.

Audit periodically: run /permissions to review what's accumulated. You might be surprised.

The settings.json file stores your allowlist. Check it into git so your team gets the same baseline permissions. New team members start with sensible defaults instead of approving everything from scratch.

Deny rules: you can also explicitly deny dangerous patterns. Don't let the agent curl arbitrary URLs, ssh into servers, or modify files in sensitive directories.
-->

---

# Your First Session

<v-clicks>

1. `cd your-project && claude`
2. Run **`/init`** — generates your starter CLAUDE.md
3. **`Shift+Tab`** twice — enter Plan Mode
4. When Claude asks to run a command you trust, **"yes, and always allow"**
5. After each task: **`/clear`**
6. When Claude makes a mistake: **"update CLAUDE.md so you don't do that again"**
7. Always: **"run the tests after each step"**

</v-clicks>

---

# Go Try It

Your homework for the next two weeks:

<v-clicks>

- Install Claude Code and set up your project
- Create your CLAUDE.md and check it in
- Configure your permission whitelist
- Try plan mode on a real task
- **Come back ready to share what you noticed**

</v-clicks>

---
layout: section
---

# Questions?

Talk 1 of the Agentic Coding Best Practices series

<!--
Sources for this talk:
- Anthropic: Verification as biggest quality multiplier, 2-3x faster execution (internal practices)
- Boris Cherny: Correction → rule loop, CLAUDE.md as team infrastructure
- Andrej Karpathy: Agent failure modes, generation vs discrimination gap
- Simon Willison: Context engineering, the lethal trifecta
- Claude Code Docs: Best practices, context management, plan mode
- MinusX: Architecture analysis — why the harness matters
-->
