# Agent Failure Snippets

Real examples of AI coding agents getting things wrong. Collected for the **Agentic Coding Best Practices** talk series — these become slide examples that show the audience what failure looks like in practice.

## Why we collect these

The talk series covers common agent failure patterns (over-engineering, assumption cascading, confidence spirals, etc). Real examples are more compelling than fabricated ones. When you catch your agent doing something dumb, save it here.

## How to add an entry

**Option 1: Paste it raw and let someone annotate later**

Just paste the transcript at the bottom of this file under a new heading. Don't worry about formatting or analysis — someone will annotate it later. Include enough context that a reader unfamiliar with your project can understand what happened.

**Option 2: Paste it and ask your agent to annotate it**

Copy this prompt and give it to your agent along with the transcript:

> Read `resources/agent-failure-snippets.md` and add a new snippet entry following the existing format. Here's the transcript: [paste transcript]. Add the annotation fields (Pattern, What went wrong, Talk, Key moment, What good looks like) and wrap the full transcript in a details/summary block.

**Option 3: Full manual entry**

Create a new `### Snippet N` heading with these fields:

```markdown
### Snippet N: [short title describing the failure]

**Pattern:** [which flaw — see pattern list below]
**What went wrong:** [1-2 sentence summary]
**Talk:** [which talk this fits — Talk 1/2/3/4 or "unsure"]

**Key moment:** [the specific line or action where things went wrong]

**What good looks like:** [what the agent should have done instead]

**The user's reaction:** [if there's a good quote from the human]

<details>
<summary>Full transcript</summary>

[paste the raw transcript here inside a code block]

</details>
```

## Pattern reference

These are the named failure patterns from our research. Use these labels in the **Pattern** field:

| Pattern | What it looks like |
|---------|-------------------|
| **Assumption cascading** | Fills gaps with guesses, builds on those guesses without checking |
| **Sycophancy** | Agrees with everything, won't push back or surface alternatives |
| **Over-engineering** | Class hierarchies, wrapper classes, config options nobody asked for |
| **Unverified claims** | "The tests pass" without running them. Asserts outcomes without evidence |
| **Cheats on tests** | Writes tests that conform to buggy code, assertion-free tests, lowers thresholds |
| **Confidence spiral** | Gets it wrong, tries again with more confidence, never understands the system |
| **Dead code accumulation** | Doesn't clean up after itself. Old implementations linger |
| **Subtle conceptual errors** | Not syntax bugs — quietly wrong decisions, wrong data structure, missed edge cases |

Multiple patterns can apply to one snippet — just list them all.

---

### Snippet 1: "Let me check" — after confidently asserting the opposite

**Pattern:** Assumption cascading, unverified claims
**What went wrong:** Agent confidently stated pyserver couldn't see the mount, proposed three workarounds (including a full rebuild), and started sizing files for a copy — all without checking whether the container already had access. When the user pushed back, it checked and immediately found the files were already there. The agent spent several turns solving a problem that didn't exist.
**Talk:** Talk 2 (The Pitfalls)

**Key moment:** The agent said "No — pyserver only has ./data:/mnt/data:ro" and immediately proposed three solutions. It had already read the compose file — it had the information to reason about shared mounts — but jumped to a conclusion instead of verifying.

**What good looks like:** Before proposing solutions, check the actual state. "Let me verify what pyserver can see" should have been the first response, not the fourth.

**The user's reaction:** "oh wow you were so confident pyserver couldn't see the files how odd"

<details>
<summary>Full transcript</summary>

```
● Clean cherry-pick, no conflicts. Both commits are now on feat/import-historical-data:

  - 05a8118 feat: add NetCDFMergeProcessor for historical merge files
  - 69d3077 feat: add PM25 polygon regeneration script for historical data

  Ready to run on staging. The script reads from /mnt/R/aqfx_done/merge_*_raw.nc — the same
  archives we populated with the import. We'd need to get these two files onto the pyserver
  container. Want to proceed?

❯ do we? are they not mounted or available to the container?

● Good point, let me check.

● Searched for 1 pattern, read 1 file (ctrl+o to expand)

● No — pyserver only has ./data:/mnt/data:ro and a named volume for /mnt/pm25_data. It
  doesn't mount /mnt/R/ where the merge archives live. That's the rserver's mount.

  The script defaults to --data-dir /mnt/R/aqfx_done which is only accessible from the
  rserver container. We have a few options:

  1. Add a volume mount to pyserver for /mnt/R/aqfx_done (read-only) and rebuild
  2. Copy the merge files from rserver into pyserver's existing mount
  3. Run the script from rserver — but it's an R container, no Python/pyserver deps

  Option 1 is cleanest. Want me to add a read-only mount of the aqfx_done directory to
  pyserver in the compose file?

❯ I dont want to do a rebuild that sounds terrible and would require a whole new deployment

● Right. Easiest option is just copy the merge files from rserver into pyserver. Let me
  check how big they are on staging.

  [... agent checks file sizes, proposes copying 4.8GB ...]

● Both containers mount the same EBS volume:

  - rserver: /mnt/ebs/data → /mnt/R
  - pyserver: /mnt/ebs/data → /mnt/data (read-only)

  So rserver's /mnt/R/aqfx_done/ is pyserver's /mnt/data/aqfx_done/. No copying needed —
  just pass --data-dir /mnt/data/aqfx_done to the script.

  [... verifies pyserver can see the files ...]

● Pyserver can already see the files. No copying, no rebuild needed.

❯ oh wow you were so confident pyserver couldn't see the files how odd

● Yeah, I should have checked the mounts instead of assuming. Sorry about that.
```

</details>

---

<!-- Add new snippets below -->
