[← Back to Topic](README.md) | [← Home](../../README.md)

# We Should Be More Tired Than the Model

Why “better” models can increase human fatigue: the bottleneck shifts from writing to *judgment*, and the cost of oversight grows when outputs are plausible but wrong.

## Index

- [The Shift: From Producing to Judging](#the-shift-from-producing-to-judging)
- [Why Fatigue Increases](#why-fatigue-increases)
- [Failure Modes to Watch](#failure-modes-to-watch)
- [Practical Mitigations](#practical-mitigations)
- [Connections](#connections)
- [Sources](#sources)

---

## The Shift: From Producing to Judging

As models get better at generating code and prose, the “work” moves from producing artifacts to evaluating them: validating assumptions, checking edge cases, and deciding what to trust [1]. This creates a new kind of bottleneck: not the model’s latency, but the human’s sustained attention and skepticism.

In agentic software development, this shows up as a recurring pattern:
- Generation becomes cheap.
- Review becomes expensive.
- The limiting factor becomes *how many careful decisions a human can make per day* [1].

## Why Fatigue Increases

The “tiredness gap” comes from asymmetry: the model can attempt infinite drafts without exhaustion, while humans pay an attention tax for each additional pass of verification [1].

Several factors compound this:
- **Plausibility without reliability.** Better outputs can be more convincing even when wrong, increasing the effort needed to disconfirm errors [1].
- **Higher output volume.** When generation is near-instant, the throughput pressure shifts to the reviewer; the queue of “things to check” grows [1].
- **Emotional tax.** Repeated near-misses (“looks right, fails later”) can produce burnout faster than obvious failures, because each issue demands context rebuilding and careful debugging [1].

## Failure Modes to Watch

### Confident wrongness
Agents can produce coherent plans and clean-looking code that bakes in incorrect assumptions (APIs, invariants, environment constraints), creating late-stage failures that are costly to unwind [1].

### Skill atrophy from outsourcing
If humans stop practicing core skills (debugging, reading code, reasoning about systems) because the agent “handles it”, then verification gets worse over time even as generation gets better [1]. Research on AI coding assistance suggests a real tradeoff between immediate productivity and long-term skill development/retention, especially when users accept outputs without deep understanding [3].

### Over-automation of judgment
Teams may automate *production* first (code generation, refactors) while leaving *evaluation* implicit (manual spot-checks, vibes-based review). This creates brittle systems where correctness depends on reviewer stamina [1].

## Practical Mitigations

### Add friction in the right places
Intentionally slow down “ship-ready” pathways while keeping exploration fast: require checklists, tests, or structured prompts before merging changes, and reserve high-trust shortcuts for throwaway experiments [1].

### Turn judgment into artifacts
Write down success criteria and invariants so verification work compounds instead of repeating:
- “What must be true?” (interfaces, constraints, security boundaries)
- “How do we know?” (tests, repro steps, assertions, monitoring)
- “What changed?” (diff summaries and explicit risk notes) [1]

### Use agents for verification, not just generation
Delegate *parts* of the evaluation loop: have agents enumerate assumptions, produce test plans, generate adversarial cases, and attempt to falsify their own outputs (“prove this works”) [1]. This shifts humans toward supervising *verification output* rather than doing all verification manually.

### Treat adoption as a staged process
In early adoption, expect inefficiency and focus on learning workflows and guardrails before aiming for throughput. A staged approach—moving from “chatbots” to “end-of-day agents” to parallel/continuous agents—helps teams build the review and safety muscles required for scale [2].

## Connections

- Pairs naturally with [Agent Limitations and Failure Modes](agent-limitations.md) as a framing for why “plausible errors” are so costly.
- Reinforces [AI Adoption Journey](ai-adoption-journey.md): the workflow shift requires new habits and guardrails, not just better prompts.
- Complements [Skill Formation](skill-formation.md): skill atrophy increases the “tiredness gap” by making humans worse verifiers over time.

## Sources

1. [newsletter.vickiboykis.com](https://newsletter.vickiboykis.com/archive/we-should-be-more-tired-than-the-model/)
2. [mitchellh.com](https://mitchellh.com/writing/my-ai-adoption-journey)
3. [arxiv.org](https://arxiv.org/abs/2601.20245)
