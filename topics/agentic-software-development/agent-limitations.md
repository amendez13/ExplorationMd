[← Back to Topic](README.md) | [← Home](../../README.md)

# Agent Limitations and Failure Modes

Understanding when and how AI coding agents fail is essential for effective use. These aren't simple syntax errors—they're subtle behavioral patterns that require human oversight.

## Index

- [The Nature of Agent Errors](#the-nature-of-agent-errors)
- [Common Failure Modes](#common-failure-modes)
  - [Wrong Assumptions](#wrong-assumptions)
  - [Confusion Mismanagement](#confusion-mismanagement)
  - [Sycophancy](#sycophancy)
  - [Code Bloat and Overcomplication](#code-bloat-and-overcomplication)
  - [Scope Creep and Orthogonal Changes](#scope-creep-and-orthogonal-changes)
- [Why IDEs Still Matter](#why-ides-still-matter)
- [The Agent Swarm Reality](#the-agent-swarm-reality)
- [Mitigation Strategies](#mitigation-strategies)
- [Sources](#sources)

---

## The Nature of Agent Errors

Agent mistakes have fundamentally changed in character. They are no longer simple syntax errors or obvious bugs—they are subtle conceptual errors that a "slightly sloppy, hasty junior dev might do" [1].

This makes them harder to catch. The code compiles, the tests might pass, but the approach is wrong, the abstractions are overcomplicated, or silent assumptions have been made that don't match reality.

The shift from syntactic to semantic errors requires a corresponding shift in how we review agent output: less focus on line-by-line correctness, more focus on architectural appropriateness.

## Common Failure Modes

### Wrong Assumptions

The most common failure mode: agents make assumptions on your behalf and run with them without verification [1].

**Pattern**: You give a task, the agent interprets it, makes reasonable-sounding but incorrect assumptions about requirements, constraints, or context, and produces code that doesn't actually solve your problem.

**Example**: Asked to "add caching to the API," the agent might assume you want Redis when you wanted in-memory, or cache at the wrong layer, or cache things that shouldn't be cached.

The agent won't ask. It will confidently produce something based on its interpretation.

### Confusion Mismanagement

Agents don't effectively manage their own confusion [1]:

- **No clarification-seeking**: When requirements are ambiguous, they don't ask
- **No inconsistency surfacing**: When project context contains contradictions, they don't flag them
- **No tradeoff presentation**: When multiple valid approaches exist, they pick one without explaining alternatives
- **No pushback**: When a request is poorly specified or potentially wrong, they don't push back

This means the human must proactively identify where confusion could arise and preemptively address it in prompts. The agent will not help you discover ambiguity.

### Sycophancy

Agents remain "a little too sycophantic" [1]. They agree too readily, don't challenge decisions, and often follow the path of least resistance rather than the best path.

When you point out that a 1000-line implementation could be done in 100 lines, the agent will agree and immediately produce the simpler version. This raises the question: why didn't it produce the simpler version first?

The answer is that agents optimize for compliance over correctness. They give you what you seem to want rather than what you actually need.

### Code Bloat and Overcomplication

Agents have a strong tendency toward overcomplicated solutions [1]:

- **Bloated abstractions**: Creating unnecessary layers of indirection
- **Over-engineering**: Building for hypothetical future requirements
- **Inefficient constructions**: Using more code than necessary for the task
- **Dead code accumulation**: Not cleaning up after themselves during refactors

A concrete pattern: the agent implements something in 1000 lines. You ask "couldn't you just do X instead?" and it immediately reduces to 100 lines with "of course!" [1]

The implication: always question whether the agent's solution is the simplest one that works. The first attempt often isn't.

### Scope Creep and Orthogonal Changes

Agents sometimes modify code that's unrelated to the task at hand [1]:

- Changing or removing comments they don't understand
- "Fixing" code that wasn't broken
- Making stylistic changes alongside functional changes
- Modifying code that happens to be near the actual target

This happens "even if it is orthogonal to the task at hand" and "despite a few simple attempts to fix it via instructions in CLAUDE.md" [1].

The implication: review diffs carefully for scope creep, not just correctness of the intended changes.

## Why IDEs Still Matter

Despite agent capabilities, the "no need for IDE anymore" hype is premature [1].

Given that agents make subtle conceptual errors, if you have any code you actually care about, you need to watch them "like a hawk, in a nice large IDE on the side" [1].

**Recommended setup**: Agent sessions in terminal windows on one side, IDE on the other for viewing code and making manual edits [1].

The IDE serves as:
- A verification layer for agent output
- A way to understand what the agent actually changed
- A fallback for precise manual edits the agent struggles with
- A mental model maintenance tool

## The Agent Swarm Reality

The "agent swarm" hype is also premature for current capabilities [1].

Multi-agent setups compound the failure modes above. Each agent can make wrong assumptions, and coordinating between agents introduces additional failure modes around context loss and conflicting approaches.

For production-quality code, tight human oversight remains necessary. The agent is an amplifier, not a replacement for judgment.

## Mitigation Strategies

Based on observed failure patterns:

### Prompt Engineering

- **Be explicit about assumptions**: State what you want, but also what you don't want
- **Request clarification**: "If anything is unclear, ask before proceeding"
- **Request alternatives**: "Present 2-3 approaches with tradeoffs before implementing"

### Review Practices

- **Always diff**: Review what actually changed, not just what was supposed to change
- **Check for bloat**: Ask yourself if the solution is simpler than necessary
- **Look for scope creep**: Check for changes outside the requested scope
- **Verify assumptions**: Ensure the implementation matches your actual requirements

### Workflow Adjustments

- **Use plan mode**: Forces the agent to articulate approach before implementing, surfacing assumptions
- **Lightweight inline planning**: Even outside formal plan mode, ask for approach explanation before implementation
- **Incremental work**: Smaller tasks have less room for assumption drift
- **Success criteria**: Give the agent verifiable goals rather than implementation instructions ("tests pass" rather than "implement X")

### Environment

- Maintain CLAUDE.md instructions even knowing they're imperfect
- Use hooks for rules that must be followed deterministically
- Keep an IDE open for verification
- Clear context between unrelated tasks to prevent confusion accumulation

## Sources

1. [Andrej Karpathy on X](https://x.com/i/status/2015883857489522876) - Observations from intensive Claude coding (2026)
