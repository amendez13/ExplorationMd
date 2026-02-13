[← Back to Topic](README.md) | [← Home](../../README.md)

# AI Agent Skills for Coding Assistants

Agent Skills are modular capabilities that extend AI coding assistants like Claude Code, Codex, Cursor, and others. Each skill packages instructions, metadata, and optional resources that the agent loads dynamically when relevant. Skills follow an open standard and can be shared across tools.

## Index

- [What Are Agent Skills](#what-are-agent-skills)
- [Global Skills](#global-skills)
  - [vercel-react-best-practices](#vercel-react-best-practices)
  - [brainstorming](#brainstorming)
  - [convex-best-practices](#convex-best-practices)
  - [frontend-design](#frontend-design)
- [Project-Dependent Skills](#project-dependent-skills)
  - [tauri-v2](#tauri-v2)
  - [swiftui-expert-skill](#swiftui-expert-skill)
  - [swift-concurrency](#swift-concurrency)
  - [swiftui-liquid-glass](#swiftui-liquid-glass)
  - [swiftui-skills](#swiftui-skills)
- [Installation](#installation)
- [Sources](#sources)

---

## What Are Agent Skills

Skills are folders containing a `SKILL.md` file as the entrypoint, plus optional templates, examples, and scripts. They teach AI assistants how to complete specific tasks in a repeatable way. Unlike `AGENTS.md` files that must be maintained per-project, skills can be installed once and shared across any project.

Skills solve the problem of keeping domain expertise in sync across projects. Common categories like SwiftUI, React, or concurrency patterns can be extracted into reusable skills that work everywhere.

---

## Global Skills

These skills apply across many projects and provide foundational capabilities.

### vercel-react-best-practices

**Source:** [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills)

Encapsulates 10+ years of React and Next.js performance optimization knowledge into 45 rules organized by impact. Focuses on eliminating request waterfalls (the #1 performance killer) and reducing bundle size.

**Key areas:**
- Critical rules for waterfalls and bundle optimization
- High-impact patterns for data fetching and caching
- Code examples comparing incorrect vs. correct implementations
- Impact metrics for each rule to guide automated refactoring

Install with: `npx add-skill vercel-labs/agent-skills`

### brainstorming

**Source:** [obra/superpowers](https://github.com/obra/superpowers)

Activates before writing code to force structured dialogue. Instead of jumping into implementation, it asks what you're really trying to do and teases out a spec through intelligent questions.

**Key behaviors:**
- Refuses vague requests and asks clarifying questions
- Shows specs in digestible chunks
- Forces requirement refinement before coding
- Part of the Superpowers framework for structured agent interactions

### convex-best-practices

**Sources:** [waynesutton/convexskills](https://github.com/waynesutton/convexskills), [Sstobo/convex-skills](https://github.com/Sstobo/convex-skills), [fluid-tools/claude-skills](https://github.com/fluid-tools/claude-skills)

Provides structured guidance for Convex development including queries, mutations, cron jobs, webhooks, and migrations.

**Key areas:**
- `convex-fundamentals` - Core mental model
- `convex-actions-scheduling` - Crons and background jobs
- `convex-anti-patterns` - Critical mistakes to avoid
- `convex-helpers-patterns` - RLS, triggers, rate limiting
- `convex-performance-patterns` - Indexes, denormalization, OCC

Install with: `npx add-skill fluid-tools/claude-skills`

### frontend-design

**Source:** [anthropics/claude-code](https://github.com/anthropics/claude-code/blob/main/plugins/frontend-design/skills/frontend-design/SKILL.md)

Creates distinctive, production-grade frontend interfaces that avoid generic "AI slop" aesthetics. Addresses distributional convergence where models default to safe, overused design choices.

**Core principles:**
- Never use generic fonts (Inter, Roboto, Arial, system fonts)
- Avoid cliched color schemes (purple gradients on white)
- Pick extreme aesthetic tones: brutalist, maximalist, retro-futuristic, editorial, etc.
- Match implementation complexity to aesthetic vision
- Every design should be different

---

## Project-Dependent Skills

These skills are useful for specific technology stacks.

### tauri-v2

**Sources:** [Smithery tauri skill](https://smithery.ai/skills/martinholovsky/tauri), [dchuk/claude-code-tauri-skills](https://playbooks.com/skills/dchuk/claude-code-tauri-skills/tauri-sidecar)

Cross-platform desktop application framework skill combining Rust backend with web frontend. Covers expertise in IPC security, capabilities system, CSP, plugin architecture, and window management.

**Key areas:**
- Sidecar embedding and management
- Plugin development for iOS/Android
- Cross-platform executable naming
- Rust and JavaScript APIs for process management

### swiftui-expert-skill

**Source:** [AvdLee/SwiftUI-Agent-Skill](https://github.com/AvdLee/SwiftUI-Agent-Skill)

Expert SwiftUI guidance covering modern APIs, state management, performance, and iOS 26+ Liquid Glass adoption. Created by Antoine van der Lee (SwiftLee blog) and Omar Elsayed.

**Key capabilities:**
- Choose correct state management (@State, @Binding, @Observable, @Bindable)
- Recommend modern replacements for deprecated APIs
- Guidance for sheets, navigation, scrolling, lists
- iOS 26+ Liquid Glass with availability fallbacks
- Stable view identity (avoiding ForEach pitfalls)
- View composition for readability and efficient diffing

### swift-concurrency

**Source:** [AvdLee/Swift-Concurrency-Agent-Skill](https://github.com/AvdLee/Swift-Concurrency-Agent-Skill)

Expert Swift Concurrency guidance for safe concurrency, performance optimization, and Swift 6+ migration. Created by Antoine van der Lee.

**Reference files:**
- `async-await-basics.md` - Fundamentals
- `tasks.md` - Lifecycle, cancellation, priorities
- `sendable.md` - Isolation domains and Sendable conformance
- `actors.md` - Actor isolation, global actors, reentrancy
- `async-sequences.md` - AsyncSequence and AsyncStream patterns
- `migration.md` - Step-by-step Swift 6 migration guide

**Target users:**
- Teams migrating to Swift 6 / strict concurrency
- Developers debugging data races and isolation errors
- Anyone wanting performance-minded concurrency patterns

### swiftui-liquid-glass

**Sources:** [dimillian/skills](https://skills.sh/dimillian/skills/swiftui-liquid-glass), [Smithery liquid-glass](https://smithery.ai/skills/patrickserrano/liquid-glass)

Specialized skill for iOS 26+ Liquid Glass API. Liquid Glass is a dynamic material combining optical glass properties with fluidity—blurring content, reflecting color and light, and reacting to touch.

**Key patterns:**
- Use `.buttonStyle(.glass)` / `.buttonStyle(.glassProminent)` for actions
- Add morphing transitions with `glassEffectID`
- Apply `.glassEffect(...)` after layout and visual modifiers
- Use `GlassEffectContainer` when multiple glass elements coexist
- Gate iOS 26+ features with `#available` and provide fallbacks

### swiftui-skills

**Source:** [swiftui-skills.ameyalambat.com](https://swiftui-skills.ameyalambat.com/)

Collection of skills built from Apple-authored documentation that ships inside Xcode. Conditions AI agents to follow the same patterns Apple expects in real SwiftUI apps.

**Key features:**
- Local install with no telemetry
- Derived from official Apple documentation
- Covers state management, navigation, and view patterns

---

## Installation

Skills follow an open standard and install similarly across tools:

**Claude Code:**
```bash
# Personal: ~/.claude/skills/
# Project: .claude/skills/
npx add-skill <org>/<repo>
```

**OpenAI Codex CLI:**
```bash
# ~/.codex/skills/
npx add-skill <org>/<repo>
```

For manual installation, copy the skill's `SKILL.md` and supporting files to the appropriate skills directory.

---

## Sources

1. [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills)
2. [obra/superpowers](https://github.com/obra/superpowers)
3. [fluid-tools/claude-skills](https://github.com/fluid-tools/claude-skills)
4. [anthropics/claude-code frontend-design](https://github.com/anthropics/claude-code/blob/main/plugins/frontend-design/skills/frontend-design/SKILL.md)
5. [AvdLee/SwiftUI-Agent-Skill](https://github.com/AvdLee/SwiftUI-Agent-Skill)
6. [AvdLee/Swift-Concurrency-Agent-Skill](https://github.com/AvdLee/Swift-Concurrency-Agent-Skill)
7. [smithery.ai/skills/martinholovsky/tauri](https://smithery.ai/skills/martinholovsky/tauri)
8. [swiftui-skills.ameyalambat.com](https://swiftui-skills.ameyalambat.com/)
9. [skills.sh/dimillian/skills/swiftui-liquid-glass](https://skills.sh/dimillian/skills/swiftui-liquid-glass)
