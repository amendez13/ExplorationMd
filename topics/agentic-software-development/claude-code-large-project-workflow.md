[← Back to Topic](README.md) | [← Home](../../README.md)

# Claude Code for Real Engineering: A Large-Project Workflow

This article summarizes a practical Claude Code workflow for shipping substantial engineering work: start with plan mode for exploration and question-generation, then implement in focused phases while actively managing the context window. The emphasis is on preserving correctness and momentum across multi-hour / multi-session efforts. [1]

## Index

- [Workflow Overview](#workflow-overview)
- [Phase 0: Dump the Rough Prompt](#phase-0-dump-the-rough-prompt)
- [Phase 1: Plan Mode as Exploration](#phase-1-plan-mode-as-exploration)
- [Phase 2: Multi-Phase Plans for Big Work](#phase-2-multi-phase-plans-for-big-work)
- [Phase 3: Implementation with “Aggressive Auto-Accept”](#phase-3-implementation-with-aggressive-auto-accept)
- [Context Window Management](#context-window-management)
- [Persisting Plans Across Resets](#persisting-plans-across-resets)
- [Rules and Guardrails](#rules-and-guardrails)
- [Sources](#sources)

---

## Workflow Overview

The core loop is:
1. Start with an imperfect prompt (often dictated) to get moving. [1]
2. Use plan mode to explore the codebase and produce clarifying questions. [1]
3. Turn the result into a multi-phase plan that can survive context resets. [1]
4. Execute each phase with higher “autonomy”, but within the boundaries set by the plan. [1]

## Phase 0: Dump the Rough Prompt

Instead of trying to craft a perfect spec, begin by writing (or dictating) a rough description of what you want. Plan mode is used to tighten the ambiguity later. [1]

## Phase 1: Plan Mode as Exploration

Plan mode is used to:
- Explore the codebase and surface unknowns early. [1]
- Generate explicit clarifying questions before implementation begins. [1]
- Convert “I want X” into concrete steps you can evaluate for completeness. [1]

This is especially useful when the work spans multiple subsystems or requires understanding existing conventions.

## Phase 2: Multi-Phase Plans for Big Work

For large tasks, split the work into phases that are small enough to fit inside a single context window (including the code the model needs to inspect). This creates a handoff artifact that stays useful even when you have to restart the agent or move to a new session. [1]

## Phase 3: Implementation with “Aggressive Auto-Accept”

After the plan is solid, shift into execution where the agent does more of the heavy lifting quickly. The key is that “speed” comes from the plan having already locked down what “done” means. [1]

## Context Window Management

Long efforts fail when the agent loses track of constraints, progress, or prior decisions. The workflow explicitly includes monitoring context usage during implementation, and designing phases that can be completed before context pressure forces a reset. [1]

## Persisting Plans Across Resets

To avoid losing momentum between sessions (or after context resets), store the plan somewhere durable (for example, in a GitHub issue) so you can rehydrate the agent from the plan rather than from memory. [1]

## Rules and Guardrails

A small set of custom rules can improve consistency, such as:
- Keep plans concise.
- Track unresolved questions explicitly. [1]

This is a lightweight alternative to over-specifying prompts, and it creates a reliable checklist for review.

## Sources

1. [YouTube: “How I use Claude Code for real engineering”](https://youtu.be/kZ-zzHVUrO4?si=Ue6dee2iQmKy6tOu)

