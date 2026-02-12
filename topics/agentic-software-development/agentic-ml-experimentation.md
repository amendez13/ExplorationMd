[← Back to Topic](README.md) | [← Home](../../README.md)

# Agentic ML Experimentation (Human-in-the-Loop)

An example of a coding agent operating as a “lab assistant” for ML experimentation: implementing changes, creating/debugging tests, launching training runs, monitoring metrics, and maintaining lightweight research documentation—while a human stays responsible for direction, design quality, and catching subtle errors [1].

## Index

- [What The Agent Did](#what-the-agent-did)
- [Artifacts That Made It Work](#artifacts-that-made-it-work)
- [Human-in-the-Loop Responsibilities](#human-in-the-loop-responsibilities)
- [Failure Modes Observed](#failure-modes-observed)
- [Practical Playbook](#practical-playbook)
- [Sources](#sources)

---

## What The Agent Did

In one reported workflow, the agent ran a full loop across code, experimentation, and operations: implementing features, debugging with toy examples, writing tests (including intentionally failing/passing cycles), launching training runs, and “babysitting” those runs by tailing logs and pulling stats from Weights & Biases (wandb) [1].

Beyond execution, the agent also maintained an ongoing record of highlights and experiment history (tables of runs/results), performed profiling, identified optimizer inefficiencies, implemented fixes, and measured the improvements [1].

It also handled repo hygiene tasks like scanning open PRs, categorizing and prioritizing them, and making commits on selected items [1].

## Artifacts That Made It Work

This workflow depends less on any single capability and more on a set of durable artifacts that an agent can update continuously: a running markdown “lab notebook” of highlights, a structured registry of runs/results, reproducible toy examples for debugging, and an experiment tracking system (wandb) as the source of truth for metrics and comparisons [1].

If you already use “overnight” or long-running agent loops, this is the same pattern applied to research and training operations: keep the loop tight, keep the state externalized, and let the agent coordinate the repetitive steps (see [Overnight Agents](overnight-agents.md)) [1].

## Human-in-the-Loop Responsibilities

The report emphasizes that the human stayed “very much in the loop”, repeatedly providing oversight and correction [1]. In practice, this means the person remains responsible for:

- Research direction and prioritization (which hypotheses to test next) [1]
- Design and architecture decisions (preventing unnecessary coupling and bloat) [1]
- Reviewing subtle correctness issues the agent may miss [1]
- Injecting new ideas the agent doesn’t propose on its own [1]

This aligns with broader “anti-slop” guidance: treat agents as high-throughput implementers, not as final arbiters of correctness or design quality (see [Agent Limitations and Failure Modes](agent-limitations.md)) [1].

## Failure Modes Observed

The agent made “subtle mistakes” that required pointed correction, got confused at times, missed ideas that the human had to introduce, and sometimes produced bloated designs with coupled abstractions that were reverted [1].

These are not just “bugs”; they’re workflow hazards:

- Confidently wrong assumptions that only show up under careful review [1]
- Local optimizations that degrade system design (abstraction bloat) [1]
- Confusion spirals mid-task, even while continuing to produce plausible output [1]

## Practical Playbook

If you want to replicate this style of agentic experimentation, the key is to structure the work so the agent can loop safely and you can audit quickly [1]:

1. **Require a small reproducer** for each bug/behavior change (toy example or minimal script) [1].
2. **Use tests as gates**, including “make it fail first” when debugging regressions [1].
3. **Treat experiment tracking as state**, not a convenience (wandb dashboards + consistent run naming) [1].
4. **Keep a single source of truth for results** (a runs/results table updated after each run) [1].
5. **Add periodic architecture checkpoints** to prevent bloat: explicit constraints and a willingness to revert over-coupled designs [1].

## Sources

1. [x.com](https://x.com/i/status/2005421816110862601)

