# Interactive Planning Techniques with AI Coding Agents

## Index

- [Overview](#overview)
- [Interrogative Plan Mode](#interrogative-plan-mode)
  - [The Technique](#the-technique)
  - [Benefits](#benefits)
  - [When to Apply](#when-to-apply)
  - [Implementation Approach](#implementation-approach)
- [Sources](#sources)

---

## Overview

AI coding agents like Claude Code and GitHub Copilot offer "Plan Mode" features that help developers map out implementation strategies before writing code. While these modes are often used passively—simply asking the AI to generate a step-by-step plan—more advanced techniques can transform them into interactive design review sessions that surface hidden assumptions and edge cases.

## Interrogative Plan Mode

### The Technique

Rather than instructing an AI agent to simply "plan out this feature," developers can explicitly request that the agent challenge their assumptions and ask probing questions about half-formed ideas [1]. This transforms Plan Mode from a planning tool into an interrogative design review session, similar to presenting a proposal to a senior engineer who will scrutinize every assumption.

The key is to frame the request to emphasize questioning over planning:
- "Challenge my assumptions about this design"
- "Ask me difficult questions about edge cases before we plan"
- "Grill me on potential problems with this approach"

### Benefits

This interrogative approach offers several advantages over passive planning [1]:

1. **Early Problem Detection**: Identifies design flaws, edge cases, and ambiguities before any code is written
2. **Assumption Surfacing**: Forces developers to confront unstated assumptions that might cause issues later
3. **Concrete Requirements**: Transforms vague ideas into well-defined specifications through iterative questioning
4. **Time Savings**: Prevents implementation work on flawed designs that would require significant rework
5. **Design Rigor**: Encourages thorough thinking about system behavior, error handling, and boundary conditions

### When to Apply

This technique is particularly valuable in specific scenarios [1]:

- **Fuzzy Requirements**: When you have a general concept but unclear implementation details
- **New Feature Exploration**: Starting a project where the full scope isn't yet defined
- **Complex Integrations**: Working with unfamiliar systems or APIs where edge cases aren't obvious
- **Architecture Decisions**: Choosing between multiple approaches without clear trade-off understanding

### Implementation Approach

To effectively use interrogative Plan Mode:

1. **Set the Frame**: Explicitly instruct the AI to ask questions rather than provide solutions
2. **Embrace Discomfort**: The most valuable questions will challenge areas you haven't fully thought through
3. **Iterate Before Planning**: Continue the questioning dialogue until you have concrete answers
4. **Document Decisions**: Capture the reasoning behind choices made during the interrogation
5. **Then Plan**: Only after thorough questioning should you proceed to actual implementation planning

This approach recognizes that AI agents can serve not just as code generators or planners, but as thinking partners that help developers refine their own understanding before committing to implementation.

## Sources

[1] Source URL (to be added)
1. [x.com](https://x.com/i/status/2019079129518281053)
2. [Source]((will be provided separately))
