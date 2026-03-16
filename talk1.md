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

<div style="display: flex; gap: 1.5rem; margin-top: 0.4rem;">
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

# Starting a Session

<div style="display: flex; gap: 2rem; margin-top: 0.75rem;">
  <div style="flex: 1; display: flex; flex-direction: column; gap: 0.6rem;">
    <v-clicks>
      <div style="font-size: 0.9rem; color: #e6edf3; line-height: 1.6;"><strong><code style="color: #00d4ff;">/init</code></strong> — analyzes your codebase, generates a starter CLAUDE.md. Run it first</div>
      <div style="font-size: 0.9rem; color: #e6edf3; line-height: 1.6;"><strong>Plan mode</strong> (<code style="color: #00d4ff;">Shift+Tab</code> twice) — writes plans to special plan files that survive compaction. Removes write tools entirely so the agent can't make edits — pure thinking</div>
      <div style="font-size: 0.9rem; color: #e6edf3; line-height: 1.6;"><strong><code style="color: #00d4ff;">/clear</code></strong> — flush context between unrelated tasks. "The single most underused command"</div>
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

---

# Working Smart

<div style="display: flex; gap: 2rem; margin-top: 0.75rem;">
  <div style="flex: 1; display: flex; flex-direction: column; gap: 0.6rem;">
    <v-clicks>
      <div style="font-size: 0.9rem; color: #e6edf3; line-height: 1.6;"><strong><code style="color: #00d4ff;">@</code></strong> — add a specific file to context directly. Handy when you know exactly which file the agent needs to see</div>
      <div style="font-size: 0.9rem; color: #e6edf3; line-height: 1.6;"><strong><code style="color: #00d4ff;">/compact</code></strong> — compress conversation to free up context. Add focus: <code style="color: #a78bfa;">/compact Focus on the API changes</code></div>
      <div style="font-size: 0.9rem; color: #e6edf3; line-height: 1.6;"><strong>Subagents</strong> — isolated sub-sessions for investigation and implementation. Keeps your main context clean</div>
    </v-clicks>
  </div>
  <div style="flex: 1;">
    <TerminalBlock title="during work">
      <div style="font-size: 0.75rem; line-height: 1.8;">
        <span style="color: #e6edf3;">> @src/auth.ts explain the token</span><br/>
        <span style="color: #e6edf3;">  refresh logic</span><br/>
        <br/>
        <span style="color: #e6edf3;">> /compact Focus on the API changes</span><br/>
        <br/>
        <span style="color: #e6edf3;">> Use a subagent to implement the</span><br/>
        <span style="color: #e6edf3;">  validation layer</span>
      </div>
    </TerminalBlock>
  </div>
</div>

<!--
/compact is less critical now that everyone has 1M context window tokens. Still useful for long sessions but don't stress about it — mention as a "good to know" not a "must do."
-->

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
    <TerminalBlock title="the feedback loop in action">
      <div style="font-size: 0.65rem; line-height: 1.8;">
        <span style="color: #8b949e;">⠋ Writing tests...</span><br/>
        <span style="color: #e6edf3;">$ npm test</span><br/>
        <span style="color: #f87171;">✗ 3 failing</span><br/>
        <br/>
        <span style="color: #8b949e;">⠋ Implementing...</span><br/>
        <span style="color: #e6edf3;">$ npm test</span><br/>
        <span style="color: #4ade80;">✓ 3 passing</span><br/>
        <br/>
        <span style="color: #8b949e;">⠋ Checking the page...</span><br/>
        <span style="color: #e6edf3;">$ browser screenshot localhost:3000</span><br/>
        <span style="color: #e6edf3;">📸 Form renders, but submit button is</span><br/>
        <span style="color: #e6edf3;">   clipped below the fold</span><br/>
        <br/>
        <span style="color: #8b949e;">⠋ Fixing layout...</span><br/>
        <span style="color: #e6edf3;">$ browser screenshot localhost:3000</span><br/>
        <span style="color: #4ade80;">✓ Form visible, button accessible</span>
      </div>
    </TerminalBlock>
  </div>
</div>

---
layout: section
---

# Set Yourself Up Right

---

# CLAUDE.md

Persistent instructions loaded into every session. Under 200 lines — this is premium context.

<div style="display: flex; gap: 1.5rem; margin-top: 0.75rem;">
  <div style="flex: 1; border: 1px solid rgba(0, 212, 255, 0.3); border-radius: 10px; padding: 0.9rem 1.1rem; background: rgba(0, 212, 255, 0.05);">
    <div style="font-size: 0.7rem; text-transform: uppercase; letter-spacing: 0.1em; color: #4ade80; margin-bottom: 0.6rem;">What belongs</div>
    <div style="display: flex; flex-direction: column; gap: 0.35rem; font-size: 0.8rem; color: #e6edf3; line-height: 1.5;">
      <div><strong>Corrections</strong> — things Claude gets wrong for your project</div>
      <div><strong>Non-obvious conventions</strong> — "use pnpm not npm"</div>
      <div><strong>Architectural decisions</strong> — prevents competing patterns</div>
      <div><strong>Things to avoid</strong> — libraries, approaches that look right but aren't</div>
      <div><strong>Pointers</strong> — <code style="font-size: 0.7rem;">@README.md</code>, <code style="font-size: 0.7rem;">@docs/architecture.md</code></div>
    </div>
  </div>

  <div style="flex: 1; border: 1px solid rgba(139, 148, 158, 0.2); border-radius: 10px; padding: 0.9rem 1.1rem; background: rgba(139, 148, 158, 0.05);">
    <div style="font-size: 0.7rem; text-transform: uppercase; letter-spacing: 0.1em; color: #f87171; margin-bottom: 0.6rem;">What doesn't</div>
    <div style="display: flex; flex-direction: column; gap: 0.35rem; font-size: 0.8rem; color: #8b949e; line-height: 1.5;">
      <div>Build/test/lint commands in <code style="font-size: 0.7rem;">package.json</code></div>
      <div>Standard language conventions Claude already knows</div>
      <div>Rules your linter already enforces</div>
      <div>Detailed API docs — link with <code style="font-size: 0.7rem;">@</code> instead</div>
      <div>Personal preferences — use <code style="font-size: 0.7rem;">CLAUDE.local.md</code></div>
    </div>
  </div>
</div>

<div style="margin-top: 0.6rem; padding: 0.4rem 0.7rem; background: rgba(124, 58, 237, 0.1); border-radius: 6px; border-left: 3px solid #7c3aed; font-size: 0.75rem; color: #a78bfa;">
  For path-specific rules, use <code style="font-size: 0.65rem;">.claude/rules/</code> with <code style="font-size: 0.65rem;">paths</code> frontmatter — rules only load when Claude touches matching files
</div>

---

# The Correction → Rule Loop

CLAUDE.md isn't written upfront. It's built through use.

<v-clicks>

- Claude makes a mistake → you correct it → **"Now update CLAUDE.md so you don't do that again"**
- Claude is "eerily good" at writing rules for itself — let it draft the rule, you review it
- Commit the CLAUDE.md update **alongside the code change** that prompted it
- Over time, Claude's error rate **measurably drops** for your project
- Your whole team benefits — check it into git, review CLAUDE.md changes in PRs like any other code
- This is how the Anthropic team works on Claude Code itself

</v-clicks>

---

# Permissions

By default, Claude asks before every command. You'll build your allowlist naturally.

<v-clicks>

- Every permission dialogue offers **"yes, and always allow"** — your allowlist grows as you work
- Be sensible about what you approve permanently — test runners and linters, yes. Arbitrary curl, maybe not
- **Watch for wildcards**: `Bash(npm run *)` allows all npm scripts — that's probably fine. `Bash(ssh *)` allows all ssh commands — maybe not
- **Audit periodically** — run `/permissions` to review what's accumulated
- Store in `.claude/settings.json` and **check into git** — your team gets the same baseline

</v-clicks>

---

# Your First Session

<v-clicks>

1. `cd your-project && claude`
2. Run **`/init`** — generates your starter CLAUDE.md. Prune it
3. Learn **`Shift+Tab`** — cycle to Plan Mode before your first real task
4. When Claude asks to run a command you trust, **"yes, and always allow"**
5. After each task: **`/clear`**. Context contamination is real
6. When Claude makes a mistake: correct it, then **"update CLAUDE.md so you don't do that again"**
7. Always include verification: **"run the tests after each step"**

</v-clicks>

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

| Source | Key Insight |
|--------|------------|
| **Anthropic** | Verification = 2-3x quality (internal data) |
| **Boris Cherny** | Correction → rule loop, CLAUDE.md as team infra |
| **Andrej Karpathy** | Agent failure modes, generation vs discrimination |
| **Simon Willison** | Context engineering, the lethal trifecta |
| **Claude Code Docs** | Best practices, context management, plan mode |
| **MinusX** | Architecture analysis — why the harness matters |

---
layout: section
---

# Questions?

Talk 1 of the Agentic Coding Best Practices series
