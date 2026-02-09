[← Back to Topic](README.md) | [← Home](../../README.md)

# Interactive Planning Techniques with AI Coding Agents

## Index
- [Overview](#overview)
- [Interrogative Plan Mode](#interrogative-plan-mode)
- [Product Architecture Planning Framework](#product-architecture-planning-framework)
- [Comparison: Implementation vs. Product Planning](#comparison:-implementation-vs.-product-planning)


---

## Overview

AI coding agents like Claude Code and GitHub Copilot offer "Plan Mode" features that help developers map out implementation strategies before writing code. While these modes are often used passively—simply asking the AI to generate a step-by-step plan—more advanced techniques can transform them into interactive design review sessions that surface hidden assumptions and edge cases.

## Interrogative Plan Mode

Rather than immediately jumping to implementation, some engineers use a conversational planning phase where the AI agent asks clarifying questions before generating code. This approach establishes shared understanding and surfaces assumptions early [1].

**Investment in Complex Tasks**

For non-trivial features or refactoring, dedicating time to plan mode yields significant returns [2]:
- Agent explores codebase to understand existing patterns and architecture
- Agent designs implementation approach and presents it for approval
- Engineer reviews and refines plan before any code is written
- Reduces rework by catching misalignments before implementation begins

**Learning from Mistakes: CLAUDE.md Evolution**

A powerful pattern is treating your CLAUDE.md file as a living document that captures lessons learned [2]:
- After every correction or mistake, update CLAUDE.md with specific guidance
- Document project conventions, common pitfalls, and preferred approaches
- The agent reads this file at session start, learning from past errors
- Over time, the agent makes fewer mistakes as CLAUDE.md encodes institutional knowledge

Example CLAUDE.md entries:
- "Always use dataclasses instead of plain dicts for configuration objects"
- "Run tests with `pytest -v` to see detailed output, not just pass/fail"
- "API responses should always include error codes, not just messages"

This creates a feedback loop where the agent becomes progressively more aligned with your project's standards and your personal preferences [2].

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

1. [Using Codex Plan Mode to Clarify Fuzzy Development Ideas](https://x.com/i/status/2019079129518281053)
2. [Plan Prompt - A Relentless Product Architecture Framework](https://x.com/i/status/2019079131284066656)
