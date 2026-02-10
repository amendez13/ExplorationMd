[← Back to Topic](README.md) | [← Home](../../README.md)

# Prompt Library

## Index
- [Relentless Product Discovery Interrogation](#relentless-product-discovery-interrogation)
- [Self-Correction CLAUDE.md Reminder](#self-correction-claudemd-reminder)
- [Staff Engineer Plan Review](#staff-engineer-plan-review)
- [Elegant Solution Refinement](#elegant-solution-refinement)
- [Prove It Works](#prove-it-works)

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

## Sources

1. [Manual note](note://relentless-product-discovery-interrogation)
2. [Manual note](note://self-correction-claudemd-reminder)
3. [Boris Cherny on X](https://x.com/bcherny/status/2017742759218794768) - Claude Code creator sharing tips (2026)
