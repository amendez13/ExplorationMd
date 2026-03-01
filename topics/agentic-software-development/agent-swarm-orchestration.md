[← Back to Topic](README.md) | [← Home](../../README.md)

# Agent Swarm Orchestration

A one-person dev team powered by an orchestration layer that spawns, monitors, and manages fleets of coding agents. Rather than using Codex or Claude Code directly, an always-on orchestrator translates business context into precise prompts and delegates to specialized agents.

## Index

- [The Architecture](#the-architecture)
  - [Why Two Tiers](#why-two-tiers)
  - [The Context Window Constraint](#the-context-window-constraint)
- [Hardening the Orchestration Layer](#hardening-the-orchestration-layer)
  - [Observability](#observability)
  - [Control Flow Ownership](#control-flow-ownership)
  - [Interruption Mechanisms](#interruption-mechanisms)
- [The Eight-Step Workflow](#the-eight-step-workflow)
  - [Step 1: Customer Request to Scoping](#step-1-customer-request-to-scoping)
  - [Step 2: Spawn the Agent](#step-2-spawn-the-agent)
  - [Step 3: Monitoring Loop](#step-3-monitoring-loop)
  - [Step 4: Agent Creates PR](#step-4-agent-creates-pr)
  - [Step 5: Automated Code Review](#step-5-automated-code-review)
  - [Step 6: Automated Testing](#step-6-automated-testing)
  - [Step 7: Human Review](#step-7-human-review)
  - [Step 8: Merge and Cleanup](#step-8-merge-and-cleanup)
- [The Ralph Loop V2](#the-ralph-loop-v2)
  - [Adaptive Respawning](#adaptive-respawning)
  - [Proactive Task Discovery](#proactive-task-discovery)
  - [Learning from Success](#learning-from-success)
- [Agent Selection](#agent-selection)
- [Implementation Details](#implementation-details)
  - [Worktree Isolation](#worktree-isolation)
  - [tmux Session Management](#tmux-session-management)
  - [Task Registry](#task-registry)
  - [Mid-Task Redirection](#mid-task-redirection)
- [Definition of Done](#definition-of-done)
- [Code Review Stack](#code-review-stack)
- [Hardware Bottlenecks](#hardware-bottlenecks)
- [Productivity Claims](#productivity-claims)
- [Getting Started](#getting-started)
- [Sources](#sources)

---

## The Architecture

The system uses OpenClaw as an orchestration layer between the human operator and fleets of coding agents. The orchestrator (named "Zoe" in this implementation) holds all business context and translates it into precise prompts for each coding agent.

```
Human → OpenClaw Orchestrator → Multiple Agents (Codex, Claude Code, Gemini)
              ↓
        Business Context:
        - Customer data
        - Meeting notes
        - Past decisions
        - What worked/failed
```

### Why Two Tiers

The single most important insight: **specialized agents through context, not through different models.**

The orchestrator and coding agents have drastically different context loads:

| Orchestrator (Zoe) | Coding Agents |
|-------------------|---------------|
| Customer history | Codebase |
| Meeting notes | Type definitions |
| Business decisions | Test patterns |
| Feature priorities | Build configs |
| Past failures | Style guides |

### The Context Window Constraint

Context windows are zero-sum. Fill it with code and there's no room for business context. Fill it with customer history and there's no room for the codebase. The two-tier system solves this: each AI is loaded with exactly what it needs for its role.

## Hardening the Orchestration Layer

An orchestration layer without proper controls is like an air traffic controller with no radar—the planes are flying, you just don't know where. Getting agents to work is not the hard part. The real complexity is getting multiple agents to coordinate effectively while maintaining safety and reliability. [2]

### Observability

Your job isn't to wire agents together. It's to know at every moment:
- What each agent decided
- Why it decided it
- Whether to trust that decision

Without observability, multi-agent pipelines fail silently. Consider a classic failure: Planner → Researcher → Executor pipeline that works in demos. In production, the Planner emits a subtly malformed subtask, the Researcher returns low-confidence results without erroring, and the Executor fires an irreversible API call. No exception raised—every agent technically "succeeded."

**Mitigation**: Instrument each handoff with structured trace logs, confidence thresholds, and human-in-the-loop gates before irreversible actions. The failure gets caught at the Researcher's output before the Executor ever runs.

### Control Flow Ownership

In naive pipelines, agents decide what runs next. An agent returns `next_step: "execute"` and the orchestrator blindly complies. This is the agent version of SQL injection—untrusted output determining trusted behavior.

A hardened orchestrator **owns the control flow**. Agents request next actions. The orchestrator approves them.

**Before (dangerous)**:
```
Agent returns send_email action → Orchestrator executes it
```

**After (hardened)**:
```
Agent returns send_email action →
Orchestrator checks:
  - Is this action in the allowed registry?
  - Is the recipient approved?
  - Has this fired in the last N seconds?
→ Then executes
```

Same capability. Completely different blast radius.

### Interruption Mechanisms

Most orchestrators handle the happy path. What they don't handle: an agent 45 seconds deep, $2 in API costs burned, stuck in a reasoning loop it will never exit.

A well-designed orchestrator tracks:
- Token consumption
- Wall-clock time
- Action repetition

When any threshold is crossed, it interrupts, captures partial state, and surfaces a decision to a human or supervisor agent.

**The design question**: If an agent started making confident but wrong decisions right now, how many actions would execute before you could stop it? The answer tells you exactly how much work remains to harden your orchestrator.

## The Eight-Step Workflow

### Step 1: Customer Request to Scoping

After a customer call, the operator discusses the request with the orchestrator. Because meeting notes sync automatically to an Obsidian vault, zero explanation is needed—the orchestrator already has context.

The orchestrator then:
1. **Tops up credits** immediately (admin API access to unblock the customer)
2. **Pulls customer config from prod database** (read-only access the coding agents will never have)
3. **Spawns a Codex agent** with a detailed prompt containing all context

### Step 2: Spawn the Agent

Each agent gets its own worktree (isolated branch) and tmux session:

```bash
# Create worktree + spawn agent
git worktree add ../feat-custom-templates -b feat/custom-templates origin/main
cd ../feat-custom-templates && pnpm install

tmux new-session -d -s "codex-templates" \
  -c "/path/to/worktrees/feat-custom-templates" \
  "$HOME/.codex-agent/run-agent.sh templates gpt-5.3-codex high"
```

Agent launch commands:

```bash
# Codex
codex --model gpt-5.3-codex \
  -c "model_reasoning_effort=high" \
  --dangerously-bypass-approvals-and-sandbox \
  "Your prompt here"

# Claude Code
claude --model claude-opus-4.5 \
  --dangerously-skip-permissions \
  -p "Your prompt here"
```

### Step 3: Monitoring Loop

A cron job runs every 10 minutes to babysit all agents. It doesn't poll agents directly (expensive). Instead, it runs a deterministic script that reads the JSON registry and checks:

- tmux sessions alive
- Open PRs on tracked branches
- CI status via gh CLI
- Auto-respawns failed agents (max 3 attempts) if CI fails or critical review feedback
- Only alerts if something needs human attention

The human isn't watching terminals. The system tells them when to look.

### Step 4: Agent Creates PR

The agent commits, pushes, and opens a PR via `gh pr create --fill`. At this point there's no notification—a PR alone isn't "done."

### Step 5: Automated Code Review

Every PR gets reviewed by three AI models:

| Reviewer | Strengths | Value |
|----------|-----------|-------|
| Codex | Edge cases, logic errors, race conditions | Most thorough, low false positive rate |
| Gemini Code Assist | Security issues, scalability problems | Free, catches what others miss, suggests fixes |
| Claude Code | Validation of other reviewers | Mostly overly cautious, skip unless marked critical |

All three post comments directly on the PR.

### Step 6: Automated Testing

The CI pipeline runs:
- Lint and TypeScript checks
- Unit tests
- E2E tests
- Playwright tests against a preview environment

A new rule: if the PR changes any UI, it must include a screenshot in the PR description. Otherwise CI fails. This dramatically shortens review time.

### Step 7: Human Review

Now the notification arrives: "PR #341 ready for review."

By this point:
- CI passed
- Three AI reviewers approved
- Screenshots show UI changes
- Edge cases documented in review comments

Human review takes 5-10 minutes. Many PRs merge without reading the code—the screenshot shows everything needed.

### Step 8: Merge and Cleanup

PR merges. A daily cron job cleans up orphaned worktrees and task registry JSON.

## The Ralph Loop V2

This is essentially the [Ralph Loop](overnight-agents.md) but with adaptive capabilities.

### Adaptive Respawning

When an agent fails, the orchestrator doesn't just respawn with the same prompt. It looks at the failure with full business context and figures out how to unblock:

- **Agent ran out of context?** → "Focus only on these three files."
- **Agent went wrong direction?** → "Stop. The customer wanted X, not Y. Here's what they said in the meeting."
- **Agent needs clarification?** → "Here's customer's email and what their company does."

The orchestrator babysits agents through to completion with context the agents don't have—customer history, meeting notes, what was tried before, why it failed.

### Proactive Task Discovery

The orchestrator doesn't wait for task assignment. It finds work proactively:

- **Morning:** Scans Sentry → finds 4 new errors → spawns 4 agents to investigate and fix
- **After meetings:** Scans meeting notes → flags 3 feature requests → spawns 3 Codex agents
- **Evening:** Scans git log → spawns Claude Code to update changelog and customer docs

### Learning from Success

When agents succeed, the pattern gets logged:
- "This prompt structure works for billing features"
- "Codex needs the type definitions upfront"
- "Always include the test file paths"

Reward signals: CI passing, all three code reviews passing, human merge. Any failure triggers the loop. Over time, the orchestrator writes better prompts because it remembers what shipped.

## Agent Selection

Not all coding agents are equal:

| Agent | Use Case | Notes |
|-------|----------|-------|
| Codex | Backend logic, complex bugs, multi-file refactors, reasoning across codebase | Slower but thorough, ~90% of tasks |
| Claude Code | Frontend work, git operations, quick tasks | Faster, fewer permission issues |
| Gemini | Design sensibility, beautiful UIs | Generate HTML/CSS spec first, hand to Claude to implement |

The orchestrator picks the right agent for each task and routes outputs between them.

## Implementation Details

### Worktree Isolation

Each agent runs in an isolated git worktree:

```bash
git worktree add ../feat-name -b feat/branch-name origin/main
```

Benefits:
- Clean separation between parallel tasks
- No merge conflicts during development
- Easy cleanup when done

### tmux Session Management

tmux is far better than `codex exec` or `claude -p` because mid-task redirection is powerful:

```bash
# Wrong approach? Don't kill it:
tmux send-keys -t codex-templates "Stop. Focus on the API layer first, not the UI." Enter

# Needs more context?
tmux send-keys -t codex-templates "The schema is in src/types/template.ts. Use that." Enter
```

### Task Registry

Tasks are tracked in `.clawdbot/active-tasks.json`:

```json
{
  "id": "feat-custom-templates",
  "tmuxSession": "codex-templates",
  "agent": "codex",
  "description": "Custom email templates for agency customer",
  "repo": "medialyst",
  "worktree": "feat-custom-templates",
  "branch": "feat/custom-templates",
  "startedAt": 1740268800000,
  "status": "running",
  "notifyOnComplete": true
}
```

When complete, updated with PR number and checks:

```json
{
  "status": "done",
  "pr": 341,
  "completedAt": 1740275400000,
  "checks": {
    "prCreated": true,
    "ciPassed": true,
    "claudeReviewPassed": true,
    "geminiReviewPassed": true
  },
  "note": "All checks passed. Ready to merge."
}
```

### Mid-Task Redirection

When an agent goes off-track, send messages to the tmux session rather than killing and restarting:

```bash
tmux send-keys -t session-name "Your redirection message" Enter
```

## Definition of Done

Very important that agents know this:

- [ ] PR created
- [ ] Branch synced to main (no merge conflicts)
- [ ] CI passing (lint, types, unit tests, E2E)
- [ ] Codex review passed
- [ ] Claude Code review passed
- [ ] Gemini review passed
- [ ] Screenshots included (if UI changes)

## Code Review Stack

Three-model review catches different issues:

1. **Codex Reviewer** - Exceptional at edge cases. Does the most thorough review. Catches logic errors, missing error handling, race conditions. False positive rate is very low.

2. **Gemini Code Assist Reviewer** - Free and incredibly useful. Catches security issues, scalability problems other agents miss. Suggests specific fixes. Install it.

3. **Claude Code Reviewer** - Mostly useless for finding issues. Tends to be overly cautious with "consider adding..." suggestions that are usually overengineering. Skip everything unless marked critical. It validates what the other reviewers flag but rarely finds critical issues on its own.

## Hardware Bottlenecks

The unexpected ceiling: **RAM**.

Each agent needs its own worktree. Each worktree needs its own `node_modules`. Each agent runs builds, type checks, tests. Five agents running simultaneously means five parallel TypeScript compilers, five test runners, five sets of dependencies loaded into memory.

A Mac Mini with 16GB tops out at 4-5 agents before swapping—and that requires luck that they don't try to build at the same time.

Recommendation: Mac Studio M4 Max with 128GB RAM (~$3,500) to power a serious agent swarm.

## Productivity Claims

Reported metrics over 4 weeks:
- 94 commits in one day (most productive day, 3 client calls, no editor opened)
- Average ~50 commits/day
- 7 PRs in 30 minutes (idea to production)
- Success rate: system one-shots almost all small to medium tasks without intervention
- Cost: ~$100/month Claude + $90/month Codex (can start with $20)

The git history looks like a dev team was hired. In reality it's one person managing an OpenClaw agent that manages a fleet of other Claude Code and Codex agents.

## Getting Started

Copy the workflow description into OpenClaw and tell it: "Implement this agent swarm setup for my codebase."

It will:
- Read the architecture
- Create the scripts
- Set up the directory structure
- Configure cron monitoring

Setup takes about 10 minutes.

## Sources

1. [Elvis Sun - OpenClaw + Codex/ClaudeCode Agent Swarm: The One-Person Dev Team](https://x.com/i/status/2025920521871716562)
2. [@asmah2107 - Agent Orchestration: Observability, Control Flow, and Interruption](https://x.com/i/status/2027721280242532455)
