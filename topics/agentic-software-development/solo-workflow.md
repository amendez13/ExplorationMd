[← Back to Topic](README.md) | [← Home](../../README.md)

# Solo Developer Workflow

## Index

- [The Workflow Shift](#the-workflow-shift)
- [The New Abstraction Layer](#the-new-abstraction-layer)
  - [The Mental Model Challenge](#the-mental-model-challenge)
    - [The Pellet Gun / Laser Beam Experience](#the-pellet-gun--laser-beam-experience)
  - [The Skill Gap Reality](#the-skill-gap-reality)
  - [Legacy Assumptions Paradox](#legacy-assumptions-paradox)
- [Mindset Over Tools](#mindset-over-tools)
- [Designer Notes](#designer-notes)
- [Session Workflow](#session-workflow)
- [Inference-Speed Shipping](#inference-speed-shipping)
- [Design Principles](#design-principles)
- [Speedup vs. Expansion](#speedup-vs-expansion)
- [Leverage Through Declarative Framing](#leverage-through-declarative-framing)
- [The Compression Factor](#the-compression-factor)
- [Agent Tenacity](#agent-tenacity)
- [The Fun Factor](#the-fun-factor)
- [Skill Atrophy](#skill-atrophy)
- [Open Questions](#open-questions)
- [Sources](#sources)

---

## The Workflow Shift

The transition to agent-first development happened with striking speed. Karpathy describes going from "about 80% manual+autocomplete coding and 20% agents in November to 80% agent coding and 20% edits+touchups in December" [6].

This represents the "biggest change to my basic coding workflow in ~2 decades of programming and it happened over the course of a few weeks" [6].

The experience is now fundamentally different: "I really am mostly programming in English now, a bit sheepishly telling the LLM what code to write... in words. It hurts the ego a bit but the power to operate over software in large 'code actions' is just too net useful" [6].

**The awareness gap**: While this shift is likely happening to "well into double digit percent of engineers," awareness in the general population remains "well into low single digit percent" [6].

---

## The New Abstraction Layer

Programming has gained a new conceptual layer that sits on top of traditional software engineering [3]. Beyond the usual layers of abstraction (hardware, OS, runtime, frameworks), developers must now master:

- **Agents and subagents** - Autonomous units that explore, plan, and execute
- **Prompts and contexts** - How to communicate intent and provide relevant information
- **Memory and modes** - Session state, plan mode vs. implementation mode
- **Permissions and tools** - Sandboxing, allowed actions, CLI integrations
- **Plugins, skills, hooks** - Extending agent capabilities and automating workflows
- **MCP and LSP** - Protocol-level integrations for tool and language server communication
- **Slash commands and workflows** - Higher-level automation patterns
- **IDE integrations** - Embedding agents into development environments

This represents a fundamental shift in what "programming" means. The programmer's contributions are becoming "increasingly sparse and between" as agents handle more of the implementation work [3].

### Abstraction Moats

One framing is that code is ultimately just abstractions—and as LLMs generate more implementation on demand, value shifts toward **choosing and offering the right abstractions** [7]. In this view, defensibility tends to come from either:

- **Complexity**: messy real-world systems with many integrations, edge cases, and constraints
- **Higher abstractions**: interfaces and products that are hard for an LLM to reliably generate end-to-end from a prompt

### The Mental Model Challenge

The core difficulty is building reliable mental models for entities that are:

- **Stochastic** - Outputs vary even for identical inputs
- **Fallible** - Make mistakes in non-deterministic ways
- **Unintelligible** - Internal reasoning is not fully transparent
- **Changing** - Capabilities shift with model updates

These properties contrast sharply with traditional software engineering, where determinism, predictability, and transparency are foundational assumptions. Integrating stochastic components into what was "good old fashioned engineering" requires rethinking fundamental assumptions about reliability and verification [3].

#### The Pellet Gun / Laser Beam Experience

A vivid metaphor captures the experiential reality: AI coding assistants behave like "a weapon that fires pellets or sometimes even misfires, and then once in a while when you hold it just right a powerful beam of laser erupts and melts your problem" [5]. Most attempts yield modest results or outright failures. Occasionally, when conditions align, the tool produces exceptional results that dissolve complex problems instantly.

This variability is central to the user experience. Success depends partly on skill—prompt construction, context management, task decomposition—but also on factors outside user control. Practitioners must internalize that inconsistency is the norm, not the exception, and structure their workflows to accommodate both the misfires and the breakthrough moments.

### The Skill Gap Reality

The feeling of being behind is pervasive among practitioners. The potential productivity boost from properly leveraging available tools could be 10X or more, but capturing that boost requires mastering a rapidly evolving ecosystem with no definitive manual [3].

Karpathy describes this as being handed a "powerful alien tool" that everyone must figure out how to operate on their own, while a "magnitude 9 earthquake" reshapes the profession. The response: "Roll up your sleeves to not fall behind" [3].

### Legacy Assumptions Paradox

Counterintuitively, experienced engineers may face more friction than newcomers when using AI tools effectively [4]. This stems from **legacy assumptions**—mental models formed when earlier models had significant limitations.

**The pattern repeats weekly**: An engineer starts approaching a problem manually (connecting a profiler, examining heap allocations by hand), only to see a colleague ask Claude to perform the same task and get a working PR in one shot [4].

Engineers who began working after models became highly capable lack these legacy memories. They ask the model to attempt tasks that experienced practitioners would have assumed were "out of scope" for AI—and often succeed.

**The recalibration cadence**: Model capabilities improve fast enough that expectations need adjustment "every month or two" [4]. What the model couldn't do last quarter may now be trivially within reach. The hardest part for early adopters is continuing to re-adjust upward.

**Concrete milestone**: One Anthropic engineer reported their first month without opening an IDE at all—Opus 4.5 wrote approximately 200 PRs, every single line [4]. This suggests the skill gap isn't just about using AI more, but about overcoming the instinct to use AI less than optimal.

## Mindset Over Tools

The most important factor in effective LLM-assisted development is not which tool or model you use - it's your mindset [1]. The specific LLM doesn't matter much. The agent doesn't matter much. What matters is understanding how to communicate effectively with the model and structure your work to leverage its capabilities.

This perspective contrasts with the common tendency to focus on tooling choices. While tools do have differences (e.g., some agents treat the entire code directory as context and figure out what to examine, reducing the need for explicit direction), the fundamental approach remains consistent across tools.

## Designer Notes

A key practice is maintaining **designer notes** within project source trees. These serve a dual purpose:

1. **LLM Context** - Provide the model with understanding of project architecture, design decisions, and conventions
2. **Human Onboarding** - Help human contributors understand and navigate the codebase

At the start of a session, direct the LLM to read these notes: "Go read this file." The more context you provide, the better the LLM understands your project and the more effectively the collaboration works [1].

This practice aligns with the broader pattern of maintaining AGENTS.md or CLAUDE.md files, but emphasizes that good documentation benefits both AI and human collaborators.

## Session Workflow

A straightforward session workflow [1]:

1. Open an agent instance in your project directory
2. Direct it to read relevant context files
3. Interact with it to accomplish tasks
4. When switching projects, close the session

The agent should treat the entire code directory as context, examining files as needed. Good agents will figure out what to look at without explicit direction.

For managing context across sessions, you can tell the LLM to "forget" things, though the effectiveness of this is uncertain.

### Tool Setup

The specific agent matters less than mindset, but practical considerations include:

- **Agent choice**: Prefer agents that treat the entire code directory as context (e.g., Codex CLI) over those requiring explicit file selection (e.g., Aider)
- **Editor integration**: Keep the agent in a separate terminal rather than forcing integration if your editor's terminal emulator has limitations
- **Simplicity**: Use tools "in the dumbest and most obvious way" - don't over-engineer the setup

## Inference-Speed Shipping

At the advanced end of solo development, output becomes limited primarily by **inference time and architectural thinking** rather than coding speed [2]. This represents a fundamental shift from implementation to oversight.

### Parallel Project Management

Advanced practitioners manage 3-8 simultaneous projects with careful mental model juggling [2]:

- Use queueing features rather than complex orchestration systems
- Commit directly to main rather than using branches
- Avoid reverting; instead redirect toward different solutions
- Keep tasks running remotely while traveling
- Run multiple machines simultaneously for parallel development

### Model Selection Strategy

Different models excel at different tasks [2]:

- **GPT 5.2 Codex**: Superior for large refactors - reads files extensively (10-15 minutes) before writing. Better context management and token efficiency
- **Claude Opus**: Faster for small edits but can miss context in larger features
- **General principle**: Match the model to the task scope

### Workflow Optimizations

- Cross-reference existing solutions across projects to save prompts
- Maintain docs folders with structured subsystem documentation
- Use image references for UI iterations ("fix padding" with screenshots)
- Let agents autonomously run in project folders for pattern implementation
- Start with CLI prototypes before building extensions or UIs
- Never use issue trackers; prioritize immediate implementation

### What Still Requires Human Thinking

Even at inference-speed, certain decisions remain manual [2]:

- Dependency and framework selection
- System architecture decisions (websockets vs. HTML, etc.)
- Data flow patterns between client/server
- Mental model maintenance across parallel projects

### The Oversight Shift

> "These days I don't read much code anymore. I watch the stream and sometimes look at key parts."

This represents a paradigm shift from **code review to architectural oversight**. The developer's role becomes system design and direction rather than implementation details.

## Design Principles

When working with LLMs on code design, leverage the principle that **core code is often implied by a core data structure and natural operations on it** [1]. If you define your data structures well, the LLM can often infer appropriate operations and implementations.

## Speedup vs. Expansion

A key insight about AI-assisted development: the productivity gain isn't primarily about doing the same things faster—it's about expanding what's worth attempting [6].

> "It's not clear how to measure the 'speedup' of LLM assistance. Certainly I feel net way faster at what I was going to do, but the main effect is that I do a lot more than I was going to do."

This expansion happens in two dimensions:

1. **Things that weren't worth coding before** become trivial to implement
2. **Code that was inaccessible due to knowledge/skill gaps** becomes approachable

The implication: measuring "speedup" misses the point. The real impact is on the *scope* of what a developer can take on, not just the velocity within existing scope.

---

## Leverage Through Declarative Framing

Agents excel when given success criteria rather than step-by-step instructions [6]:

> "Don't tell it what to do, give it success criteria and watch it go."

**High-leverage patterns**:
- Have the agent write tests first, then implement code that passes them
- Put the agent in a loop with browser automation (MCP)
- Write a naive algorithm first for correctness, then ask the agent to optimize while preserving behavior
- Shift from imperative ("do X, then Y") to declarative ("make this true") to get agents looping longer

The key insight: **agents are exceptionally good at looping until they meet specific goals**. This is where the "feel the AGI" magic emerges. Structure tasks to exploit this strength.

---

## The Compression Factor

The productivity gain from LLM assistance is substantial enough that the "compression factor" becomes difficult to estimate [1]. Tasks that wouldn't have been worth the effort become trivial.

### Example: API Integration

A concrete example: Adding integration with repology.org to fetch package information across different repositories. The task involved:

- Reading project identifiers from a control file
- Calling the repology.org API
- Parsing JSON responses into Python data structures
- Using this data to find email addresses for release notifications

This was "done in minutes" - a task that would have been "annoyingly long" by hand and might have been deprioritized given other pressing work.

### Implications

- Features and improvements that previously fell below the effort threshold become achievable
- Maintenance tasks that would accumulate as technical debt get addressed
- Solo developers can take on work that would previously require more resources
- The barrier shifts from "is this worth the effort" to "is this worth describing"

## Agent Tenacity

One of the most striking qualities of agent-based development is the relentless persistence agents display [6]:

> "It's so interesting to watch an agent relentlessly work at something. They never get tired, they never get demoralized, they just keep going and trying things where a person would have given up long ago to fight another day."

Watching an agent struggle with a problem for an extended period and then succeed is described as a "'feel the AGI' moment" [6]. The agent might work at something for 30 minutes where a human would have context-switched to something easier.

**The insight**: Stamina is a core bottleneck to work that humans rarely notice because it's so fundamental to the human experience. With LLMs, that bottleneck has been "dramatically increased" [6]. Problems that would have been abandoned due to frustration or fatigue can now be worked through systematically.

---

## The Fun Factor

Counterintuitively, programming can feel *more* enjoyable with agents, not less [6]:

- **Drudgery removed**: The "fill in the blanks" work is handled by the agent
- **Creative work remains**: What's left is the interesting design and decision-making
- **Less blocked/stuck**: There's "almost always a way to work hand in hand with it to make some positive progress"
- **More courage**: Willingness to attempt things that would have seemed too tedious before

However, this isn't universal. LLM coding "will split up engineers based on those who primarily liked coding and those who primarily liked building" [6]. For some, the act of writing code was the reward; for them, delegation to agents removes the enjoyable part.

---

## Skill Atrophy

A concerning observation: "I've already noticed that I am slowly starting to atrophy my ability to write code manually" [6].

The key insight is that **generation and discrimination are different cognitive capabilities**. You can review and understand code effectively even as your ability to produce it from scratch degrades.

> "Largely due to all the little mostly syntactic details involved in programming, you can review code just fine even if you struggle to write it."

This has implications for:
- How engineers maintain skills during extended agent-assisted periods
- Whether traditional coding practice remains necessary
- How to evaluate engineers who've primarily worked with agents
- The reversibility of the transition if agent capabilities regress or access changes

---

## Open Questions

The phase shift raises fundamental questions about the future of software development [6]:

**The 10X Engineer Ratio**: What happens to the productivity ratio between the mean and the max engineer? It's possible this grows *significantly*. If agents amplify capability, those who leverage them best may pull dramatically further ahead.

**Generalists vs. Specialists**: Do generalists increasingly outperform specialists? LLMs are better at "fill in the blanks" (the micro) than "grand strategy" (the macro). This suggests that broad vision and system-level thinking may become more valuable than deep expertise in narrow domains.

**The Feel of Future Development**: What does LLM coding feel like in the future? Is it like playing StarCraft (micromanaging many units)? Playing Factorio (designing systems and watching them execute)? Playing music (improvising with a partner)? The experience is still being defined.

**The Knowledge Work Bottleneck**: How much of society is bottlenecked by digital knowledge work? If AI removes that bottleneck, the second-order effects are difficult to predict.

---

## Sources

1. [Eric S. Raymond on X](https://x.com/esrtweet/status/2019391670609940746) - Discussion thread on LLM-assisted development workflow (2026)
2. [Shipping at Inference-Speed](https://steipete.me/posts/2025/shipping-at-inference-speed) - Peter Steinberger on AI-assisted development velocity (2025)
3. [Andrej Karpathy on X](https://x.com/i/status/2004607146781278521) - The new programmable layer of abstraction (2026)
4. [Anthropic engineer on X](https://x.com/i/status/2004626064187031831) - Legacy assumptions and the mental adjustment problem (2026)
5. [Developer on X](https://x.com/i/status/2004628491862696070) - The pellet gun / laser beam experience metaphor (2026)
6. [Andrej Karpathy on X](https://x.com/i/status/2015883857489522876) - Claude coding observations (2026)
7. [X discussion on abstraction moats](https://x.com/i/status/2010044820740563412) - Code as abstractions; moats in complexity or higher abstraction (2026)
