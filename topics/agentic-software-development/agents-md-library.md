[← Back to Topic](README.md) | [← Home](../../README.md)

# AGENTS.md Library

A curated collection of real-world AGENTS.md files demonstrating effective patterns for giving AI agents subsystem-specific context. These files serve as reference implementations and inspiration for structuring your own agent instructions.

## Index

- [What is AGENTS.md](#what-is-agentsmd)
- [Why Subdirectory Placement Matters](#why-subdirectory-placement-matters)
- [Library](#library)
  - [Ghostty Inspector Subsystem](#ghostty-inspector-subsystem)
- [Pattern Analysis](#pattern-analysis)
- [Sources](#sources)

---

## What is AGENTS.md

AGENTS.md (or CLAUDE.md for Claude Code specifically) is a convention for providing AI coding agents with project-specific context. The file contains instructions, constraints, and resources that help agents understand how to work effectively within a codebase.

Unlike generic documentation, AGENTS.md is written specifically for AI consumption—optimized for the way agents parse and apply instructions during coding sessions.

## Why Subdirectory Placement Matters

Placing AGENTS.md files in subdirectories rather than only at the repository root is a powerful pattern for several reasons:

1. **Scoped context**: When an agent works in a specific subdirectory, it automatically receives relevant context without loading unrelated instructions for other parts of the codebase.

2. **Reduced token usage**: Subsystem-specific instructions are loaded only when needed, preserving context window for actual work.

3. **Maintainability**: Teams can maintain instructions for their domains independently. The frontend team updates `src/frontend/AGENTS.md`; the API team maintains `src/api/AGENTS.md`.

4. **Hierarchical inheritance**: Most agent tools merge parent and child AGENTS.md files, so subsystem files can override or extend root-level instructions.

5. **Discovery-friendly**: Agents exploring the codebase naturally encounter relevant instructions as they navigate to specific directories.

The pattern reflects how human developers think about codebases—not as monolithic wholes, but as composed subsystems with their own conventions, dependencies, and gotchas.

---

## Library

### Ghostty Inspector Subsystem

**Source**: [ghostty-org/ghostty](https://github.com/ghostty-org/ghostty/blob/ca07f8c3f775fe437d46722db80a755c2b6e6399/src/inspector/AGENTS.md)

**Location**: `src/inspector/AGENTS.md`

**Context**: Ghostty is a GPU-accelerated terminal emulator written in Zig. The inspector subsystem is a developer tool similar to browser DevTools, built with Dear ImGui.

```markdown
## Inspector Subsystem

The inspector functions as a developer tool similar to browser inspection utilities.
It enables users to examine and modify terminal state.

**Key resources:**

- Access the complete C API via `dcimgui.h`, located in the `.zig-cache` folder
  at the repository root. Use this command: `find . -type f -name dcimgui.h`.
  Employ the most recent version available.

- Reference comprehensive widget usage examples by consulting the ImGui demo file at:
  `https://raw.githubusercontent.com/ocornut/imgui/refs/heads/master/imgui_demo.cpp`

- On macOS, execute builds with the `-Demit-macos-app=false` flag to validate API usage.

- This package does not include unit tests.
```

**Why this works**:

1. **Concise purpose statement**: A single sentence explains what the inspector does—no fluff.

2. **Concrete resource pointers**: Instead of explaining Dear ImGui, it points to the exact header file and demo code. Agents can fetch these resources when needed.

3. **Build flag specificity**: The macOS flag prevents agents from burning context on build failures that stem from environment, not code.

4. **Explicit test expectations**: Stating "no unit tests" prevents agents from wasting time searching for test files or wondering why tests don't exist.

5. **Appropriate scope**: At 12 lines, this file contains only what an agent needs to work in the inspector directory. Everything else lives in parent AGENTS.md files or is discovered through code exploration.

---

## Pattern Analysis

Common patterns observed across effective AGENTS.md files in subdirectories:

| Pattern | Description | Example |
|---------|-------------|---------|
| Purpose statement | One sentence explaining the subsystem's role | "The inspector functions as a developer tool..." |
| Resource pointers | URLs or paths to external dependencies and docs | Link to ImGui demo, find command for header |
| Build/test commands | Subsystem-specific invocations | `-Demit-macos-app=false` flag |
| Negative constraints | What NOT to do or expect | "This package does not include unit tests" |
| Discovery commands | Shell commands to find relevant files | `find . -type f -name dcimgui.h` |

**What to avoid**:

- Duplicating information already in parent AGENTS.md
- Including general coding style rules (those belong at root level)
- Over-explaining obvious patterns (agents can infer from code)
- Outdated instructions that no longer apply

---

## Sources

1. [ghostty-org/ghostty - src/inspector/AGENTS.md](https://github.com/ghostty-org/ghostty/blob/ca07f8c3f775fe437d46722db80a755c2b6e6399/src/inspector/AGENTS.md)
