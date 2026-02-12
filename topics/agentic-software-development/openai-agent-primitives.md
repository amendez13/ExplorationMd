[← Back to Topic](README.md) | [← Home](../../README.md)

# OpenAI Agent Primitives: Skills, Shell, and Compaction

OpenAI introduced three agentic primitives for building long-running agents: Skills (reusable instruction bundles), Shell Tool (hosted execution environments), and Server-Side Compaction (automatic context management). These patterns apply broadly to any agent framework.

## Index

- [Skills as Procedures](#skills-as-procedures)
- [Shell Execution Environment](#shell-execution-environment)
- [Context Management via Compaction](#context-management-via-compaction)
- [Ten Practical Tips](#ten-practical-tips)
- [Implementation Patterns](#implementation-patterns)
- [Recommended Workflow](#recommended-workflow)
- [Sources](#sources)

---

## Skills as Procedures

Skills bundle files with a `SKILL.md` manifest, functioning as versioned playbooks the model consults when needed [1]. The platform exposes skill name, description, and path to inform model routing decisions.

This aligns with the Agent Skills open standard—a common format across providers.

---

## Shell Execution Environment

The shell tool operates either through OpenAI-managed containers or locally-controlled runtimes [1]. Key characteristics:

- Enables dependency installation, script execution, and output generation
- Integrated with the Responses API for stateful work and multi-turn continuation
- Container reuse supported for continuity across turns
- `/mnt/data` serves as the artifact boundary for outputs to retrieve or pass downstream

---

## Context Management via Compaction

Compaction preserves continuity during extended workflows through automatic in-stream compression or explicit `/responses/compact` endpoint usage [1].

This addresses the fundamental constraint of context windows filling during long-running tasks—similar to Claude Code's auto-compaction and `/compact` command.

---

## Ten Practical Tips

### 1. Skill Descriptions as Routing Logic

Write descriptions answering: when to use, when to avoid, outputs, and success criteria—not marketing language [1].

### 2. Negative Examples Reduce Misfires

Include explicit "don't call when..." scenarios. Early Glean testing showed ~20% initial accuracy drops that recovered after adding edge case coverage [1].

### 3. Embed Templates Inside Skills

Templates and examples loaded only upon invocation don't inflate unrelated queries, improving latency and quality for structured outputs [1].

### 4. Design for Continuity Early

Use container reuse, pass `previous_response_id`, and treat compaction as a default rather than emergency fallback [1].

### 5. Explicit Determinism

When production workflows require predictability, instruct models: "Use the [skill name] skill" [1].

### 6. Secure Skill + Network Combinations

Network access combined with powerful procedures creates exfiltration risks. Maintain strict allowlists and assume tool output is untrusted [1].

### 7. Use `/mnt/data` as Artifact Boundary

Designate this directory for outputs retrieved, reviewed, or passed downstream [1].

### 8. Two-Layer Allowlist System

Organization-level allowlists set maximum destinations; request-level policies must remain subsets. Requests exceeding org allowlists error [1].

### 9. `domain_secrets` for Authentication

Models see credential placeholders; a sidecar injects real values only for approved destinations, preventing credential exposure [1].

### 10. Consistent API Across Environments

Skills work with both hosted and local shell modes, enabling development iteration locally before production deployment [1].

---

## Implementation Patterns

### Pattern A: Data Pipeline

Install dependencies → fetch external data → write artifact to `/mnt/data` [1]

### Pattern B: Deterministic Workflows

Encode workflows in skills, mount into shell environment, execute deterministically for spreadsheet analysis, dataset cleaning, or standardized reports [1].

### Pattern C: Enterprise SOPs (Glean Example)

Skills as living SOPs encoding recurring tasks. Glean achieved:
- 73% → 85% accuracy improvements
- 18.1% time-to-first-token reductions

Using Salesforce-oriented skills [1].

---

## Recommended Workflow

1. Iterate locally during development
2. Transition to hosted containers for repeatability
3. Maintain skills consistently across deployment contexts
4. Keep networking locked with minimal allowlists
5. Use domain secrets for authenticated API calls

---

## Sources

1. [Shell + Skills + Compaction: Tips for Long-Running Agents](https://developers.openai.com/blog/skills-shell-tips) - OpenAI Developer Blog, February 2026
