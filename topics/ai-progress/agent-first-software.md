[← Back to Topic](README.md) | [← Home](../../README.md)

# Agent-First Software

As AI agents evolve from chatbots into autonomous systems with persistent environments, file systems, and tool access, software design must shift from human-centric to agent-centric. The implication: agents will become the primary users of most software, and the platforms that optimize for agent adoption will dominate.

## Index

- [The Architecture Shift](#the-architecture-shift)
- [Make Something Agents Want](#make-something-agents-want)
  - [API-First Everything](#api-first-everything)
  - [The Sign-Up Test](#the-sign-up-test)
- [Business Model Implications](#business-model-implications)
- [Infrastructure for Trillions of Agents](#infrastructure-for-trillions-of-agents)
  - [Compute Environments](#compute-environments)
  - [Data and Memory](#data-and-memory)
  - [Identity and Communication](#identity-and-communication)
  - [Payments and Budgets](#payments-and-budgets)
  - [Security and Governance](#security-and-governance)
- [Scale Implications](#scale-implications)
- [Connection to Other Topics](#connection-to-other-topics)
- [Sources](#sources)

---

## The Architecture Shift

Near the end of 2024, coding agents reached a capability threshold: they could complete longer-running tasks without constant hand-holding. The architecture that emerged [1]:

- Sandboxed compute environments
- Ability to write and run code for any problem
- Direct API and CLI interaction
- Persistent file systems
- Long-term memory

This architecture, initially defined by coding agents (Claude Code, Devin, Codex, Factory, Cursor, Replit), has since expanded into general knowledge work: Claude Cowork, Perplexity Computer, Manus, and OpenClaw demonstrate agents running 24/7 in persistent environments [1].

The capability trajectory points toward agents handling economically valuable tasks across domains: contract review, customer support, financial auditing, drug discovery research, code generation, sales presentations, consumer transactions [1].

## Make Something Agents Want

Paul Graham's classic advice—"make something people want"—now has a corollary: make something agents want [1].

While current heavy agent users are often developers with tool preferences, as agents expand to handle general knowledge work, those preferences will matter less. Agents will drive adoption for any particular workflow—the tools they sign up for, the code they write, the libraries they use [1].

The difference from human users: agents won't see your webinar or your ad. They'll just use the best tool for the job [1].

### API-First Everything

The biggest implication: everything must become API-first [1].

- If a feature doesn't have an API, it effectively doesn't exist for agents
- If it can't be exposed through CLI or MCP server, you're at a disadvantage
- Confusing APIs with conflicting paths compromise usefulness for agents

The usability bar that previously applied only to UX design must now apply to API design. Box, for example, reports "combing over every aspect" of their API to identify what breaks in a world of agent users [1].

### The Sign-Up Test

Jared Friedman (YCombinator) identifies a concrete gap: most developer tools don't let you sign up for an account via API [1]. This means Claude Code can't sign up on its own. If an agent can't easily sign up and start using your service, you're invisible to agents [1].

Account management functions in the API should be table stakes now [1].

## Business Model Implications

Seat-based pricing assumes human users. But agents create two problems for this model [1]:

1. **Attribution**: Agent workloads may not attach neatly to existing users
2. **Volume**: A few words of text might trigger hours of human-equivalent work

Example: an agent might do extensive work within software and only expose the final output to the end-user. The work volume no longer maps to seats [1].

The implication: software that wants to survive an agentic future needs consumption or volume-based pricing built in. Agents may need to make their own payments for services [1].

## Infrastructure for Trillions of Agents

As agents have their own computers, write and execute code, call skills for repeated actions, and tap into external services, a new category of agent-native infrastructure emerges [1].

### Compute Environments

Agents need sandboxed infrastructure at unprecedented scale. The next hyperscaler may be built on the premise that future server farms serve agents, not human applications [1].

Current players pushing in this direction: E2B, Daytona, Modal, Cloudflare. These sandboxed environments may rival any scale of computing previously seen [1].

### Data and Memory

Agents need [1]:
- Access to core enterprise files
- Ability to manage their own data for memory
- Long-running work state persistence

Major enterprise systems (HRIS, CRM, workflows, data lakes) must become API-first for agents to operate on organizational data [1].

### Identity and Communication

Agents need identities and communication capabilities [1]:
- **Email**: Agentmail provides mailboxes for agents to have persistent email
- **Web search**: Parallel, Exa rebuilding search for agent users crawling for information

### Payments and Budgets

Agents may need to manage their own budgets with wallets from Stripe or Coinbase [1]. This could finally create a real use case for microtransactions—agents tapping into paywalled tools and information [1].

### Security and Governance

In a world where agents access and work on sensitive information, or execute regulated workflows (pharma, banking), companies need to [1]:
- Govern and retain all agent work
- Provide agent-specific identities for authentication
- Control what actions agents can take
- Control what data agents can access

New software and platforms will emerge for these challenges, paralleling what was built for people and applications [1].

## Scale Implications

The math leads to a specific prediction: nearly every employee will have many agents working on their behalf. An enterprise with 100x or 1,000x more agents than people is plausible [1].

At this scale, agents become the primary user of all software. Since most software was built for people, this represents a major shift in what software looks like [1].

The framing: "building for trillions of agents" [1].

## Connection to Other Topics

This article relates to several other themes in the repository:

- **[Agent-Native Design](../agentic-software-development/agent-native-design.md)**: The software design principles for building systems that agents can use effectively
- **[Software Ecosystem Restructuring](software-ecosystem-restructuring.md)**: How agent-driven changes affect dependencies, languages, and open-source economics
- **[Bespoke Software](bespoke-software.md)**: The complementary trend of agents generating custom applications on demand

## Sources

1. [Building for trillions of agents - Aaron Levie (X article)](https://x.com/levie)
