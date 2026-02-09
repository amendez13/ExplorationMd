# Interactive Planning Techniques with AI Coding Agents

## Index

- [Overview](#overview)
- [Interrogative Plan Mode](#interrogative-plan-mode)
- [Product Architecture Planning Framework](#product-architecture-planning-framework)
- [Comparison: Implementation vs. Product Planning](#comparison-implementation-vs-product-planning)
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

## Product Architecture Planning Framework

### The Framework

The Product Architecture Planning Framework uses AI as an adversarial interrogator to extract comprehensive requirements before any technical planning begins [2]. Rather than immediately jumping into implementation details, this approach establishes a rigorous discovery phase where the AI systematically challenges assumptions, surfaces constraints, and questions fundamental problem definitions [2].

The core principle is **exhaustive discovery over premature planning** [2]. The AI is instructed to refuse forward progress until all unknowns are fully explored, including organizational constraints (timeline, budget, team capacity) and technical limitations [2].

### Methodology

The framework operates through several key practices:

**Relentless Questioning**
- Continuously use tools like `request_user_input` to extract details [2]
- Challenge any vague language or undefined terms [2]
- Explore edge cases and failure modes systematically [2]
- Investigate second-order consequences of decisions [2]

**Constraint Surfacing**
- Explicitly identify timeline and budget limitations [2]
- Determine team size and capacity constraints [2]
- Uncover technical limitations and dependencies [2]
- Surface unstated assumptions about resources [2]

**Problem Validation**
- Question whether the stated problem is the real problem [2]
- Challenge the fundamental need being addressed [2]
- Explore alternative problem framings [2]
- Ensure alignment between symptoms and root causes [2]

**Clarity Gate**
- Forbid moving to structured planning until complete mutual understanding exists [2]
- Require explicit confirmation that all unknowns have been addressed [2]
- Establish shared mental models between human and AI [2]

### Key Differentiators

This framework differs from standard planning approaches in several ways:

1. **Adversarial Posture**: The AI explicitly adopts a challenging stance, acting as a rigorous interrogator rather than a cooperative assistant [2]

2. **Problem-First**: Questions the fundamental problem definition before accepting implementation requirements [2]

3. **Constraint-Explicit**: Makes organizational and resource constraints a mandatory part of the discovery process [2]

4. **Gatekeeping**: Creates a hard boundary between discovery and planning phases, refusing to proceed prematurely [2]

### When to Apply

This framework is most valuable for:

- **Strategic Product Decisions**: When defining new features or products with significant resource investment [2]
- **Ambiguous Requirements**: When stakeholders have broad goals but unclear specifics [2]
- **Cross-Functional Impact**: When decisions affect multiple teams or systems [2]
- **High-Risk Initiatives**: When failure costs are significant or irreversible [2]
- **Organizational Alignment**: When assumptions about constraints, budget, or timeline need validation [2]

It may be excessive for:
- Well-defined bug fixes
- Routine maintenance tasks
- Implementations with clear, documented requirements
- Time-sensitive tactical decisions

## Comparison: Implementation vs. Product Planning

While both Interrogative Plan Mode and the Product Architecture Planning Framework use questioning techniques, they operate at different scopes:

| Aspect | Interrogative Plan Mode | Product Architecture Framework |
|--------|------------------------|--------------------------------|
| **Scope** | Implementation details | Product strategy and problem definition |
| **Assumes** | Problem is well-defined | Problem may need validation |
| **Questions** | How, where, what patterns | Why, whether, what alternatives |
| **Constraints** | Technical constraints | Organizational + technical constraints |
| **Output** | Implementation plan | Requirements document + problem validation |
| **Timing** | Before coding | Before any planning |

Both techniques can be used sequentially: the Product Architecture Framework first to validate the problem and surface constraints, then Interrogative Plan Mode to design the implementation approach [1][2].

## Sources

1. [x.com](https://x.com/mrmps/status/1879977943851299036)
2. [x.com](https://x.com/i/status/2019079129518281053)
