[← Back to Topic](README.md) | [← Home](../../README.md)

# AI Adoption Journey

Mitchell Hashimoto's structured approach to adopting AI coding tools, from initial skepticism through continuous agent operation. The framework emphasizes deliberate phases of inefficiency before achieving workflow transformation.

## Index

- [The Three Universal Phases](#the-three-universal-phases)
- [Stage 1: Abandon Chatbots](#stage-1-abandon-chatbots)
- [Stage 2: Reproduce Your Work Manually](#stage-2-reproduce-your-work-manually)
- [Stage 3: End-of-Day Agents](#stage-3-end-of-day-agents)
- [Stage 4: Parallel Work Model](#stage-4-parallel-work-model)
- [Stage 5: Harness Engineering](#stage-5-harness-engineering)
- [Stage 6: Continuous Agent Operation](#stage-6-continuous-agent-operation)
- [Key Mental Models](#key-mental-models)
- [Concrete Recommendations](#concrete-recommendations)
- [Skill Formation Concerns](#skill-formation-concerns)
- [Sources](#sources)

---

## The Three Universal Phases

Hashimoto identifies that meaningful tool adoption follows three universal phases:

1. **Inefficiency period** - Forcing through awkwardness and frustration
2. **Adequacy phase** - Reaching baseline productivity
3. **Workflow transformation** - Achieving patterns impossible without the tool

The journey requires deliberate practice through inefficiency. Skipping stages leads to abandonment or shallow adoption.

## Stage 1: Abandon Chatbots

Pure chat interfaces proved inefficient for coding. Chatbots require repeated human corrections and manual context management.

**The shift**: Move to *agents*—LLMs capable of reading files, executing programs, and making HTTP requests in loops. Agents can observe, act, and iterate without constant human re-prompting.

## Stage 2: Reproduce Your Work Manually

Hashimoto deliberately completed tasks twice: first manually, then with agents. This painful process revealed three principles:

1. **Break sessions into clear, actionable tasks** - Vague requests lead to wandering
2. **Separate planning from execution** - For vague requests, make planning explicit before implementation
3. **Provide agents verification mechanisms** - Give them ways to self-correct without human intervention

The dual-execution approach builds intuition for what agents handle well versus poorly.

## Stage 3: End-of-Day Agents

Running 30-minute agent sessions before leaving for the day proved effective for:

- **Deep research** - Exploring codebases, understanding unfamiliar systems
- **Parallel exploration** - Trying multiple approaches to vague ideas
- **Issue triage** - Categorizing and prioritizing backlogs

The result: a "warm start" the next morning with context already prepared.

## Stage 4: Parallel Work Model

Once confident in agent capabilities, delegate high-confidence tasks while working on preferred projects.

**Critical practice**: Turn off notifications. Context-switching costs erase the productivity gains from parallel work. Maintain control over interruption timing.

The parallel model only works when you trust the agent enough to not watch it constantly.

## Stage 5: Harness Engineering

Engineer solutions to prevent recurring agent mistakes:

- **AGENTS.md files** - Document agent behaviors, patterns, and gotchas specific to each project
- **Custom programmatic tools** - Build verification mechanisms agents can invoke
- **Constraint specification** - Encode boundaries agents must respect

This is infrastructure investment—upfront cost that compounds over time as agents stop repeating the same mistakes.

## Stage 6: Continuous Agent Operation

Operating with persistent background agents, achieving 10-20% coverage of workdays.

**Current limits**: Not 100% coverage. One agent at a time performs better than multiple simultaneous agents. Use slower, higher-quality models for complex changes.

The target is sustained background operation, not maximum parallelism.

## Key Mental Models

### Understanding When Not to Use Agents

Recognizing unsuitable tasks creates efficiency gains by avoiding wasted effort. Some tasks remain faster to do manually.

### Context as Competitive Advantage

Interruptions destroy productivity even when agents are doing the work. The discipline to stay focused while delegating is itself a skill.

### Legacy Assumptions

Experienced practitioners must continuously recalibrate expectations. What didn't work six months ago may work now. Mental models formed with older models create friction.

## Concrete Recommendations

1. **Segment work** - Separate planning and execution sessions
2. **Create verification systems** - Let agents catch their own mistakes
3. **Document agent behaviors** - Maintain project-specific guides (AGENTS.md)
4. **Prefer quality over quantity** - One agent running well beats multiple running poorly
5. **Use slower models for complex work** - Speed matters less than correctness for significant changes

## Skill Formation Concerns

Hashimoto acknowledges a real tradeoff: delegating tasks means not developing skills in those areas.

**His approach**: Maintain manual work on preferred areas while delegating others. Accept the tradeoff consciously.

**Warning for junior developers**: Without sufficient fundamentals, you lack the judgment to know when agents are wrong. Build the foundation first, then delegate.

---

## Sources

1. [mitchellh.com](https://mitchellh.com/writing/my-ai-adoption-journey)
