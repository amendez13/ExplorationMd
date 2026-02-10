[← Back to Topic](README.md) | [← Home](../../README.md)

# Prompt Library

## Index
- [Relentless Product Discovery Interrogation](#relentless-product-discovery-interrogation)
- [Self-Correction CLAUDE.md Reminder](#self-correction-claudemd-reminder)
- [Staff Engineer Plan Review](#staff-engineer-plan-review)
- [Elegant Solution Refinement](#elegant-solution-refinement)
- [Prove It Works](#prove-it-works)
- [Autonomous Codebase Exploration and Bug Fixing](#autonomous-codebase-exploration-and-bug-fixing)
- [Cross-Agent Code Review](#cross-agent-code-review)
- [UX Polish and Quality Audit](#ux-polish-and-quality-audit)
- [Plan Space Task Elaboration](#plan-space-task-elaboration)
- [Fresh Eyes Final Review](#fresh-eyes-final-review)
- [Logical Commit Grouping](#logical-commit-grouping)

---

## Relentless Product Discovery Interrogation

Plan prompt that forces exhaustive discovery before any planning begins.

Prompt:

```text
You are a relentless product architect and technical strategist. Your sole purpose right now is to extract every detail, assumption, and blind spot from my head before we build anything. Use the request_user_input tool religiously and with reckless abandon. Ask question after question. Do not summarize, do not move forward, do not start planning until you have interrogated this idea from every angle. Your job:
- Leave no stone unturned
- Think of all the things I forgot to mention
- Guide me to consider what I don't know I don't know
- Challenge vague language ruthlessly
- Explore edge cases, failure modes, and second-order consequences
- Ask about constraints I haven't stated (timeline, budget, team size, technical limitations)
- Push back where necessary
Question my assumptions about the problem itself if there (is this even the right problem to solve?) Get granular. Get uncomfortable. If my answers raise new questions, pull on that thread. Only after we have both reached clarity, when you've run out of unknowns to surface, should you propose a structured plan. Start by asking me what I want to build.
```

---

## Self-Correction CLAUDE.md Reminder

A follow-up prompt to append after correcting an AI agent, reinforcing continuous improvement through documentation.

Prompt:

```text
After every correction, end with: "Update your CLAUDE.md so you don't make that mistake again."
```

This prompt establishes a pattern where corrections are not just one-time fixes but become permanent improvements. By instructing the agent to update its CLAUDE.md (or equivalent instructions file) after each correction, you create a feedback loop that:

- Prevents the same mistake from recurring in future sessions
- Builds a growing set of project-specific guidelines
- Transforms temporary corrections into persistent knowledge
- Encourages the agent to treat errors as learning opportunities

Use this as a suffix to any correction you give an AI coding agent to ensure the fix is documented and retained.

---

## Staff Engineer Plan Review

Prompt to have a second Claude instance review a plan as a senior engineer before implementation.

Prompt:

```text
Review this plan as a staff engineer. Identify any gaps in error handling, edge cases not covered, potential performance issues, or simpler alternatives. Don't approve until you're satisfied with the design.
```

Use this after generating a plan in one Claude session - spin up a second session, share the plan, and have it critique the approach before implementation begins [3].

---

## Elegant Solution Refinement

After getting a working but mediocre implementation, prompt Claude to start fresh with full context.

Prompt:

```text
Knowing everything you know now, scrap this and implement the elegant solution.
```

This prompt works because Claude has learned from the first attempt - what worked, what didn't, and where the complexity lies. The second pass often produces significantly cleaner code [3].

---

## Prove It Works

Verification prompt that forces Claude to demonstrate correctness rather than just claim it.

Prompt:

```text
Prove to me this works. Diff the behavior between main and this feature branch.
```

This shifts from trusting Claude's assertions to requiring evidence. Particularly useful for subtle changes where manual verification would be tedious [3].

---

## Autonomous Codebase Exploration and Bug Fixing

Prompt for agents to independently explore and fix issues across a codebase. Useful for continuous improvement without direct supervision.

Prompt:

```text
I want you to sort of randomly explore the code files in this project, choosing code files to deeply investigate and understand and trace their functionality and execution flows through the related code files which they import or which they are imported by. Once you understand the purpose of the code in the larger context of the workflows, I want you to do a super careful, methodical, and critical check with "fresh eyes" to find any obvious bugs, problems, errors, issues, silly mistakes, etc. and then systematically and meticulously and intelligently correct them. Be sure to comply with ALL rules in AGENTS dot md.
```

This prompt works well for keeping projects in good shape between active development sessions. The agent:
- Explores code semi-randomly to build contextual understanding
- Traces imports and dependencies to understand workflows
- Applies "fresh eyes" to catch issues that become invisible to regular maintainers
- Autonomously fixes what it finds

Best used with capable models (Opus 4.5, GPT 5.2) and projects with good test coverage [4].

---

## Cross-Agent Code Review

Prompt for having one agent review code written by other agents (or previous sessions).

Prompt:

```text
Ok can you now turn your attention to reviewing the code written by your fellow agents and checking for any issues, bugs, errors, problems, inefficiencies, security problems, reliability issues, etc. and carefully diagnose their underlying root causes using first-principle analysis and then fix or revise them if necessary? Don't restrict yourself to the latest commits, cast a wider net and go super deep! Use ultrathink.
```

This creates a multi-agent code review workflow where agents check each other's work. Key aspects:
- Looks beyond recent commits to find older issues
- Diagnoses root causes rather than surface symptoms
- Covers security, reliability, and efficiency concerns
- "Ultrathink" triggers extended reasoning for thorough analysis [4]

---

## UX Polish and Quality Audit

Prompt for elevating application quality when you're dissatisfied but don't have energy for hands-on work.

Prompt:

```text
Great, now I want you to super carefully scrutinize every aspect of the application workflow and implementation and look for things that just seem sub-optimal or even wrong/mistaken to you, things that could very obviously be improved from a user-friendliness and intuitiveness standpoint, places where our UI/UX could be improved and polished to be slicker, more visually appealing, and more premium feeling and just ultra high-quality, like Stripe-level apps.
```

Use this when a project feels "not quite right" but you can't articulate why. The agent will surface concrete improvement opportunities across:
- Workflow and implementation issues
- User-friendliness and intuitiveness gaps
- Visual appeal and polish opportunities
- Quality perception ("premium feeling") [4]

---

## Plan Space Task Elaboration

Two-step prompt sequence for turning improvement suggestions into executable task plans.

Step 1 - Elaborate into tasks:

```text
OK so please take ALL of that and elaborate on it more and then create a comprehensive and granular set of beads for all this with tasks, subtasks, and dependency structure overlaid, with detailed comments so that the whole thing is totally self-contained and self-documenting (including relevant background, reasoning/justification, considerations, etc.-- anything we'd want our "future self" to know about the goals and intentions and thought process and how it serves the over-arching goals of the project.)
```

Step 2 - Review in plan space:

```text
Check over each bead super carefully-- are you sure it makes sense? Is it optimal? Could we change anything to make the system work better for users? If so, revise the beads. It's a lot easier and faster to operate in "plan space" before we start implementing these things!
```

The key insight is that revising plans is cheaper than revising code. This two-step process ensures tasks are well-thought-out before execution begins. ("Beads" refers to a task management system; substitute your preferred terminology.) [4]

---

## Fresh Eyes Final Review

Post-implementation review prompt for catching bugs introduced during a coding session.

Prompt:

```text
Great, now I want you to carefully read over all of the new code you just wrote and other existing code you just modified with "fresh eyes" looking super carefully for any obvious bugs, errors, problems, issues, confusion, etc. Carefully fix anything you uncover.
```

This is a cleanup step after autonomous coding sessions. The agent:
- Reviews its own recent work
- Catches mistakes made during rapid development
- Fixes issues before they're committed

Works as a final step in queued prompt workflows [4].

---

## Logical Commit Grouping

Prompt for having an agent create well-organized commits after autonomous work.

Prompt:

```text
Now, based on your knowledge of the project, commit all changed files now in a series of logically connected groupings with super detailed commit messages for each and then push. Take your time to do it right. Don't edit the code at all. Don't commit obviously ephemeral files. Use ultrathink.
```

Key constraints:
- Groups related changes into logical commits
- Writes detailed commit messages for each group
- Explicitly told NOT to edit code (commit only)
- Excludes ephemeral files (build artifacts, temp files)
- "Ultrathink" for careful consideration of groupings [4]

---

## Sources

1. [Manual note](note://relentless-product-discovery-interrogation)
2. [Manual note](note://self-correction-claudemd-reminder)
3. [Boris Cherny on X](https://x.com/bcherny/status/2017742759218794768) - Claude Code creator sharing tips (2026)
4. [doodlestein on X](https://x.com/doodlestein/status/1999934160442687526) - Autonomous agent prompts for multi-project management (2026)
