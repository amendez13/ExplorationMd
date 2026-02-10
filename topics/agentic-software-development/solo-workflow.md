[← Back to Topic](README.md) | [← Home](../../README.md)

# Solo Developer Workflow

## Index

- [Mindset Over Tools](#mindset-over-tools)
- [Designer Notes](#designer-notes)
- [Session Workflow](#session-workflow)
- [Design Principles](#design-principles)
- [The Compression Factor](#the-compression-factor)
- [Sources](#sources)

---

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

## Design Principles

When working with LLMs on code design, leverage the principle that **core code is often implied by a core data structure and natural operations on it** [1]. If you define your data structures well, the LLM can often infer appropriate operations and implementations.

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

## Sources

1. [Eric S. Raymond on X](https://x.com/esrtweet/status/2019391670609940746) - Discussion thread on LLM-assisted development workflow (2026)
