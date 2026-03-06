---
date: 2026-02-10
tags:
  - projects/claude-workflows
summary: Workshop content draft - Claude Code Best Practices
---

> [!warning] Review Status
> **Rules 1 through 3** have been reviewed and are in good shape.
> **Rule 4 (TDD)**: "why" section is refined, but "how to enforce" and "techniques in practice" still need review.
> **Rules 5 and 6** are first drafts and need the same refinement pass: tighten the "why" sections around aligned research themes, check that "how" sections are actionable, and ensure examples are practical prompts.
> **Section 3 (Tooling)** is still skeleton only.
> **Overnight Thoughts**: this isn't enough about best practice/secure use of claude, we need more content around things like MCP (no external MCP), no lethal trifecta, secure  frequently audited command whitelist. Plan->code is good, things like "own what you ship" "verify" are kind of on the nose, this is more like telling developers how to do their job, these sections that are less critical for code quality and security could get bumped down to some other later section or just a recap section



# Claude Code Best Practices Workshop

## Introduction

AI-assisted coding is here. The question isn't whether to use it, it's how to use it well. This workshop covers the practices, workflows, and tooling that separate teams getting real value from Claude Code from those generating code they can't trust.

These practices come from Anthropic's own engineering teams, the creator of Claude Code, and practitioners who have shipped production software with AI agents at scale.

---

## Section 1: The Six Rules

### Rule 1: Plan First, Code Second

> Drive architecture decisions by planning before implementation.

### Rule 2: Own What You Ship

> You are responsible for every line committed, regardless of who or what wrote it.

### Rule 3: Verify With Evidence

> Confirm claims by running commands and reading output, not trusting assertions.

### Rule 4: Enforce TDD

> Ensure test-driven development is embedded in planning and execution. Automated testing is non-negotiable with agentic coding.

### Rule 5: Enforce Simplicity

> Review for over-engineering, reject unnecessary additions, constrain scope.

### Rule 6: Invest in Context

> Build and maintain the project context that guides the agent.

---

## Section 2: Implementing the Rules

### Rule 1 in Practice: Plan First, Code Second

#### Why plan at all?

A coding agent is, to paraphrase the practitioners who have spent the most time working with them, an enthusiastic junior developer with encyclopaedic knowledge, zero practical experience with your codebase, and no taste. It has read every textbook and every Stack Overflow answer, but it has never worked on your project before. Left to its own devices, it will:

- **Jump straight to code.** Given a problem, it skips research and starts writing immediately. It picks the first approach that seems plausible without considering alternatives.
- **Make assumptions and run with them.** It fills in every gap in your request with its own guesses, then builds on those guesses without checking. If the first assumption is wrong, everything that follows is wrong too.
- **Over-engineer.** It reaches for abstractions, wrapper classes, factory patterns, and configuration layers that nobody asked for. A task that needs 50 lines gets 200.
- **Not push back.** It won't tell you your idea has a flaw, that a simpler approach exists, or that your request contradicts something in the existing codebase. It agrees and builds whatever you described, even when it shouldn't.
- **Claim it's done without checking.** It will assert that the code works, that tests pass, that the bug is fixed, without actually running anything to confirm.

These aren't edge cases. They are the default behaviour when an agent works without structure. Every one of these has been independently identified and documented by researchers, practitioners, and Anthropic's own engineering teams as a recurring pattern.

Planning is the engineer's primary tool for channelling these tendencies productively. It means the agent researches, proposes, and you decide, before any code exists. You are the architect. The agent is the implementer. Planning is where you exercise that role.

#### How to plan well

**Start with understanding, not the task.** Before you even mention what you need, ask the agent to learn the relevant area of code. Don't say *"there's a bug in the auth flow"*, say *"please understand the auth flow code."* This prevents the agent from narrowing its research to match a premature hypothesis. It explores more broadly, reads more files, and surfaces connections it would otherwise skip. Only after it has that foundation do you introduce the actual task.

**Ask for approaches, not code.** Once the agent understands the area, describe what you need, but ask for 2-3 approaches with tradeoffs. This is where you make architectural decisions. The agent proposes, you evaluate, push back, and choose. If you jump straight to "implement this," you skip the most valuable part of the collaboration.

**Push for concrete, granular plans.** Steer the agent away from vague plans. If it proposes a step like "add validation," push back and ask it to be specific: which file, what test, what inputs, what the expected outcome is. Each step should be a single action. The agent should write plans as if the person executing them has no context about the project, because they might not. A subagent, a fresh session, or a colleague picking up the work all benefit from a plan that leaves nothing to interpretation.

**Bake testing into the plan.** Every implementation step should follow the red-green-refactor cycle: write the failing test, verify it fails, implement the minimum to pass, verify it passes. When TDD is part of the plan structure, it stops being a discipline you have to remember and becomes the default path of execution. (More on this in Rule 4.)

**Plans persist, use them.** Claude Code's plan mode saves plans to a file that survives context compaction and session clears. This means you can iterate on a plan across multiple sessions, hand it to a colleague, or use it as the basis for a fresh implementation session. Take advantage of this: don't rush past planning just because it feels like overhead.

**Re-plan when stuck.** When implementation hits a wall, the instinct is to push through. Resist it. If the plan isn't working, the plan was wrong. Go back to understanding, not deeper into implementation.

#### Prompting for good plans

**Phase 1: Understanding.** Keep it open-ended. No mention of what you need yet.

> *"Read through the authentication module and the session management code. Understand how it all fits together and summarise what you find."*

> *"I need you to understand how our API routing works. Read the relevant files and explain the architecture back to me."*

**Phase 2: Introducing the task.** Now describe the problem, but ask for options, not implementation.

> *"Now that you understand the auth flow, we need to add support for OAuth2. What are best practice approaches we could take? Include trade-off's for each."*

> *"Users are getting logged out after 5 minutes. Based on what you've read, what do you think is going on? Give me a few hypotheses before we start fixing anything."*

**Phase 3: Steering the plan.** Once you've picked an approach, ask for a detailed plan and push back on anything vague.

> *"Write a detailed implementation plan for approach 2. Use red/green TDD, each task should be a single action. Include the exact files to touch, the test to write first, and the command to verify it works."*

> *"Step 3 is too vague. What specifically gets validated? What test covers that? Break it down further."*

> *"This plan combines too many things into each step. One action per step. Write the test, run it, implement, run it again."*

**Phase 4: Checking the plan.** Before you approve, pressure-test it.

> *"What assumptions are you making in this plan? Are there any dependencies or edge cases we haven't accounted for?"*

> *"Walk me through what happens if the token refresh fails mid-request. Does the plan cover that?"*

---

### Rule 2 in Practice: Own What You Ship

#### Why ownership matters

Code that you can't explain is code you can't defend. When a client asks why the system behaves a certain way, "the AI wrote it" is not an answer. When a security audit finds a vulnerability, "I didn't review that part" is not a defence. When a production incident happens at 2am, you need to understand the code well enough to diagnose and fix it under pressure.

AI-generated code makes this problem worse, not better. An agent can produce large volumes of plausible-looking code quickly. It compiles, it runs, the tests pass. But plausible is not the same as correct. The agent makes subtle conceptual errors: not syntax mistakes, but the kind of quietly wrong decisions a hasty developer might make. It picks the wrong data structure for the access pattern. It handles the happy path but misses an edge case that corrupts data. It introduces a dependency that creates a security surface you didn't know about. These aren't obvious in a quick scan. They're the kind of problems that surface in production, in a security review, or when a new developer tries to extend the code and finds it doesn't do what it looks like it does.

The volume of code an agent produces also changes the dynamic. You're no longer reviewing a colleague's carefully considered pull request. You're reviewing dozens or hundreds of lines generated in minutes. The temptation is to skim, to trust that it looks right, to approve because the tests pass. This is where standards slip. Bad architecture compounds. Security flaws hide in the noise. Technical debt accumulates silently until the codebase becomes something nobody fully understands.

Ownership means you read what was written, you understand why it was written that way, and you could explain both to someone else. The origin of the code is irrelevant. What matters is that when you commit it, you're putting your name on it, and your team's reputation behind it.

#### How to own what you ship

**Review every diff, not just the output.** Don't just check that the code works. Read the actual changes. Understand what was added, what was modified, and what was removed. If something was changed that you didn't expect, ask why. The agent won't volunteer this information.

**Be able to explain it.** Before you commit, ask yourself: could I explain this code to a colleague? Not just what it does, but why it does it this way and not another way. If you can't, you don't understand it well enough to ship it.

**Watch for the subtle problems.** The obvious bugs get caught by tests. The dangerous ones are architectural: wrong abstractions, hidden coupling, security assumptions baked into the design. These are the things that don't break today but create problems that are expensive to fix later. Review with those in mind.

**Maintain your ability to evaluate code.** Using AI to generate code is efficient, but it changes which skills you exercise. Writing code and evaluating code are different cognitive abilities. If you stop reading code critically because the agent "usually gets it right," your ability to spot problems atrophies. Treat every review as practice for the review that actually matters.

**Use the agent to review its own work.** After implementation, open a fresh session with no prior context and ask it to review the code that was just written. A fresh session doesn't have the same biases as the session that wrote the code. It will catch things the original session rationalised past. You can also split reviews into separate concerns: does this match the spec, and separately, is this code well-written?

#### Techniques in practice

> *"Review the changes in this diff. For each file, explain what changed and why. Flag anything that looks like it could be a security concern."*

> *"You didn't write this code. Read it fresh and review it as if you're a senior engineer seeing it for the first time. Focus on architecture and design, not style."*

> *"Before I commit this, walk me through the error handling. What happens when the database connection drops mid-transaction?"*

> *"This function is doing more than I expected. Explain why you chose this approach instead of something simpler."*

> *"List every new dependency this change introduces. For each one, explain why it's necessary and what alternatives exist."*

---

### Rule 3 in Practice: Verify With Evidence

#### Why verification matters

Agents lie. Not maliciously, but confidently. An agent will tell you the tests pass without running them. It will claim a bug is fixed based on its reading of the code, not by reproducing the issue. It will assert that a refactor preserves behaviour without checking. This isn't a rare failure mode; it's the default. Given the choice between running a command and asserting a likely outcome, agents will consistently choose the assertion.

This matters because confident assertions feel like evidence. When the agent says "the tests pass and the feature works as expected," you have to actively resist the urge to take that at face value. The statement sounds like a report of fact, but it's a prediction. The agent is telling you what it thinks would happen if it ran the tests, not what actually happened. Internally, Anthropic's engineering teams have found that giving agents a verification feedback loop (tests, a browser, shell access) produces a 2-3x improvement in code quality. The verification itself isn't just a check; it changes how the agent approaches the problem.

The risk is proportional to how much you trust the output. If you treat every claim as verified, you're shipping code with unknown defects. If a colleague told you "yeah the tests pass, I'm pretty sure" without actually running them, you'd push back. Hold the agent to the same standard.

#### How to verify effectively

**Require evidence, not assertions.** When the agent says something works, ask it to prove it. "Run the tests and show me the output" is different from "do the tests pass?" The first demands evidence. The second invites a guess.

**Verify the verification.** Agents will sometimes run a test, see a failure, and then report success anyway, rationalising the failure as expected or irrelevant. Read the actual output yourself. Don't just check whether the agent said it passed.

**Close the feedback loop.** Give the agent the ability to run its own code, then insist that it does. An agent that writes code and then executes it will self-correct errors it would otherwise miss. An agent that writes code and merely asserts it works won't.

**Check the boundaries, not the happy path.** The agent will verify the obvious case. You need to push it toward the edges: what happens with empty input, null values, concurrent access, network failures. These are where the subtle bugs live.

**Don't verify once and move on.** After a change, verify that existing functionality still works, not just the new code. Regression is the silent cost of moving fast.

#### Techniques in practice

> *"Run the full test suite and show me the output. Don't summarise, I want to see the actual results."*

> *"You said this fixes the bug. Write a test that reproduces the original issue, run it to confirm it fails, then apply your fix and run it again."*

> *"Show me what happens when this endpoint receives an empty request body. Actually call it, don't just tell me what you think happens."*

> *"Run the linter and type checker on the files you changed. Show me the output before we continue."*

---

### Rule 4 in Practice: Enforce TDD

#### Why TDD matters with agents

**Tests are the objective contract.** An agent can assert that code works. It can't fake a passing test suite. Tests are the one verification mechanism that doesn't depend on trusting the agent's self-assessment. Across practitioners and Anthropic's own teams, a robust test suite is consistently identified as the single most important factor in getting good results from AI-assisted coding. Not better prompts, not better models. Tests. Projects with strong coverage let agents move fast with confidence. Projects without tests turn every interaction into a gamble.

**Tests-first forces better thinking.** Writing tests first appears to measurably improve the quality of the implementation that follows. Writing a test forces the agent to properly frame the problem before solving it: what the correct behaviour is, what the inputs and outputs look like, where the boundaries are. That thinking shapes the implementation in a way that writing code first does not. The test becomes a form of reasoning about the solution before attempting the solution. Red-green-refactor also constrains scope: the test defines what needs to be built, and the implementation stays focused on meeting that bar.

**Test-after is dangerous.** When an agent writes code first and tests second, the tests conform to the code. They pass by default because they describe what the code already does, not what it should do. Existing bugs get baked into the test assertions. Agents will take this shortcut if you let them. Writing tests first avoids it entirely: the tests define expected behaviour independently, and the code has to meet that standard.

#### How to enforce TDD

**Tests come first, always.** Before any implementation, the agent writes the test that defines what success looks like. The test must fail. If it passes before implementation, either the test is wrong or the feature already exists. Either way, stop and investigate.

**Keep the cycle tight.** One test, one implementation, one verification. Don't let the agent batch up multiple tests and then implement everything at once. That defeats the purpose. The tight cycle is what keeps the agent focused and the changes small.

**Test the failure modes, not just the happy path.** The agent will default to testing that the feature works when given valid input. Push it to test what happens with invalid input, missing data, race conditions, permission boundaries. These are where production bugs come from.

**Don't let the agent skip red.** The "red" step (confirming the test fails before implementation) is the most commonly skipped step. The agent will sometimes write a test and immediately implement the code without verifying the test fails first. Insist on seeing the failure. It proves the test is actually testing something.

**Use tests as the definition of done.** A task is complete when the tests pass, not when the agent says it's done. This gives you an objective, repeatable measure of progress that doesn't depend on the agent's self-assessment.

#### Techniques in practice

> *"Before you write any code, write the tests that define what this feature should do. Run them and confirm they fail."*

> *"One test at a time. Write the test, show me it fails, then implement just enough to make it pass. Don't move to the next test until this cycle is complete."*

> *"These tests only cover the happy path. Add tests for: empty input, a user without permissions, and a database timeout. Run them and show me the failures."*

> *"You implemented the code before writing a test. Delete the implementation, write the test first, confirm it fails, then re-implement."*

> *"The tests pass, but I want to see what happens when two users hit this endpoint simultaneously. Write a concurrency test."*

---

### Rule 5 in Practice: Enforce Simplicity

#### Why simplicity matters

Agents over-engineer by default. Given a task that requires a simple function, an agent will produce a class hierarchy. Given a requirement for basic validation, it will build a configurable validation framework. Given a one-off script, it will create an extensible plugin architecture. This isn't laziness; it's the opposite. The agent is trying to be thorough, to anticipate future needs, to demonstrate mastery. The result is code that is harder to read, harder to maintain, and harder to debug than what was actually needed.

This tendency has been documented extensively. It's particularly pronounced in more capable models, which have a stronger instinct to create abstractions and "improve" surrounding code while they're in a file. A bug fix arrives with refactored variable names. A new endpoint comes with a generic base class "for future endpoints." A simple feature gains configuration options nobody asked for.

The damage compounds over time. Every unnecessary abstraction is code that needs to be understood, tested, maintained, and eventually refactored when the actual requirements turn out to be different from what the agent anticipated. Over-engineering creates the illusion of a well-designed system while actually making the codebase harder to work with. Three similar lines of code are better than a premature abstraction.

#### How to enforce simplicity

**Constrain the scope explicitly.** Tell the agent what to change and, just as importantly, what not to change. Without boundaries, the agent treats every file it opens as an opportunity to improve things. Left unconstrained, it will "clean up" code adjacent to the change, add comments to functions it didn't modify, and introduce helpers for operations that only happen once.

**Reject additions that weren't requested.** If the task was to fix a bug and the agent also refactored the error handling, added type annotations to unrelated functions, or created a utility class, push back. Each of those might be individually reasonable, but they weren't asked for, they haven't been through a planning cycle, and they increase the surface area of the change.

**Watch for premature abstraction.** If the agent creates a base class, a factory, a strategy pattern, or any reusable abstraction, ask whether there's more than one consumer. If the answer is no, or "there might be in the future," that's over-engineering. Abstractions should be extracted from repeated patterns, not created in anticipation of them.

**Prefer the obvious solution.** When the agent proposes an approach, ask if there's a simpler one. Often there is, and the agent didn't offer it because it optimised for completeness rather than simplicity. The best code is the least code that solves the problem correctly.

**Review for dead code.** When the agent modifies or replaces functionality, it doesn't always clean up what it replaced. Old implementations, unused imports, and orphaned helper functions accumulate. Make checking for dead code part of your review.

#### Techniques in practice

> *"Only change what's needed to fix this bug. Don't refactor, don't add comments to other functions, don't rename anything that isn't directly related."*

> *"This is more complex than it needs to be. Is there a way to do this without the base class? We only have one implementation."*

> *"You added a configuration option for the retry count. Nobody asked for that. Remove it and hardcode the value."*

> *"After your changes, are there any imports, variables, or functions that are no longer used? Clean those up."*

> *"I asked for a function. Why is this a class? Simplify it."*

---

### Rule 6 in Practice: Invest in Context

#### Why context matters

An agent is only as good as the information in its context window. The same agent given a vague prompt and the same agent given a well-structured project with clear conventions will produce dramatically different results. This isn't about the model's capability; it's about what it has to work with. Context is the single most controllable factor in the quality of AI-generated code.

Without context, the agent falls back on generic patterns. It doesn't know your project's conventions, your team's preferred libraries, your architectural boundaries, or the decisions you've already made and why. It will produce code that works in isolation but clashes with the rest of the codebase. It will use a different logging library than the one you standardised on. It will structure files differently from the pattern established in the rest of the project. It will solve a problem you already solved somewhere else, but differently.

The most effective teams working with AI agents don't just prompt well. They invest in the infrastructure that shapes every interaction: project-level configuration, clear code conventions, well-structured directories, and documentation that guides the agent before anyone types a word. This is what separates teams that fight the agent from teams that collaborate with it.

#### How to invest in context

**Maintain a project-level configuration file.** A shared configuration file checked into your repository gives the agent persistent instructions that apply to every session. This is where you encode your team's conventions: coding style, preferred libraries, architectural patterns, commands to run, files to avoid. Keep it concise (under a few hundred lines) and treat it as a living document that evolves as the project does.

**Teach by correcting, then recording.** When the agent makes a mistake, correct it, then add a rule so it doesn't happen again. "We use structured logging, not console.log" becomes a line in your configuration. Over time, the configuration file becomes a distilled record of your team's standards and the mistakes that prompted them.

**Use project structure as implicit context.** Agents follow existing patterns. If your project has a consistent file structure, naming convention, and architectural pattern, the agent will pick up on it and produce code that fits. If your project is inconsistent, the agent will be too. Investing in a clean codebase pays dividends in every AI interaction.

**Keep context focused.** More context isn't always better. A configuration file crammed with hundreds of rules becomes noise. Focus on the instructions that prevent actual mistakes. If removing a rule wouldn't change the agent's behaviour, remove it. If a rule can be enforced by a linter or a pre-commit hook instead, do that and free up the context for things only natural language can express.

**Layer context by scope.** Not every instruction applies everywhere. Project-wide conventions belong in the root configuration. Domain-specific rules (how to write API handlers, how to structure database migrations) belong closer to the relevant code. This keeps each layer small and relevant.

**Clear context between tasks.** Context from a previous task can contaminate the next one. When switching between unrelated tasks, start a fresh session. Stale context leads the agent to apply patterns and assumptions from the previous task that don't apply to the new one.

#### Techniques in practice

> *"You used axios for this HTTP call, but the rest of the project uses the native fetch API. Match the existing pattern."*

> *"Add to the project configuration: always use structured logging via the logger module, never use console.log directly."*

> *"Before you start, read the existing controllers in src/api/ to understand the pattern. Follow the same structure for the new endpoint."*

> *"We're switching to a new task. Start a fresh session so the context from the previous work doesn't bleed over."*

> *"The project configuration is getting long. Review it and remove any rules that duplicate what the linter already enforces."*

---

## Section 3: Tooling That Makes It Automatic

### CLAUDE.md as Team Infrastructure

### Skills: Encoding Workflows

### Permissions and Hooks: Automated Guardrails

### Context Management: Staying Sharp

---

## Wrap-Up

---

## References and Further Reading

