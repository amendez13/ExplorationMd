[← Back to Topic](README.md) | [← Home](../../README.md)

# Agent-Native Design

A paradigm shift in software development: building software *for* agents as first-class users, not just using agents to build software.

## Index

- [The Digital Catalog Phase](#the-digital-catalog-phase)
- [The Distinction](#the-distinction)
- [The Delegation Test](#the-delegation-test)
- [Design Questions](#design-questions)
- [Tradeoffs and Boundaries](#tradeoffs-and-boundaries)
- [Why This Matters Now](#why-this-matters-now)
- [Sources](#sources)

---

## The Digital Catalog Phase

Most people building with AI are in the same position as early web merchants who replicated physical store layouts online instead of creating native e-commerce experiences [1]. They use AI to build software faster, but the software itself isn't designed for agent interaction.

The real shift will be designing for agents as first-class users of your software, not just builders of it. We're still very much in the "digital catalog" phase—replicating old paradigms with new tools rather than rethinking what we build.

## The Distinction

There are two different things people mean by "AI-native":

1. **AI-native builders**: Using AI to write code (Claude Code, Copilot, Codex)
2. **AI-native products**: Software designed for agents to consume and operate

Most practitioners are the first type. Few are building the second.

## The Delegation Test

A practical heuristic for deciding when to think agentically [1]:

> "Would I delegate this to a person? If so, think agentically."

If a task is something you'd hand to an assistant with context and expect them to figure out, it's a candidate for agent-first design. Instead of building a local research app with a UI, build skills and CLIs that an agent can use to do research.

## Design Questions

Two complementary questions to ask when designing:

| Question | Focus |
|----------|-------|
| "How would an agent use this?" | Designing human software with agent affordances |
| "How can an agent do this?" | Designing agent-native capabilities from scratch |

The first retrofits existing patterns. The second rethinks the approach entirely.

**Example**: Browser automation

- Traditional approach: Build a deterministic script for browser automation
- Agent-native approach: Give Claude browser access directly

Both are valid—the choice depends on the use case.

## Tradeoffs and Boundaries

Not everything should be agentic. Key considerations:

**Security and privacy**: Some tasks require deterministic, auditable behavior. Giving agents direct access to sensitive systems may not be appropriate even when technically possible.

**Determinism vs flexibility**: Authentication should remain deterministic. Research and exploration benefit from agent flexibility.

**The decision framework**:
- Use deterministic scripts when: auditability matters, security is critical, behavior must be reproducible
- Use agent-native design when: tasks benefit from adaptation, context-dependent decisions, or natural language interfaces

## Why This Matters Now

Part of the lag is historical: many developers have held ideas for years but lacked technical ability to ship them. Now that AI makes building easier, they execute those held ideas rather than reconsidering what to build in this new context [1].

The opportunity is in asking: what becomes possible when software's primary users are agents, not humans? What interfaces, APIs, and tools would we design differently?

## Sources

1. [X thread on AI-native building vs AI-native software](https://x.com/rauchg/status/example) - Guillermo Rauch, @kelseyhightower, and discussion
