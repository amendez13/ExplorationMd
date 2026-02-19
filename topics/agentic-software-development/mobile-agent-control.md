[← Back to Topic](README.md) | [← Home](../../README.md)

# Mobile Agent Control

Running AI coding agents remotely from mobile devices using messaging interfaces like Telegram.

## Index

- [The Problem](#the-problem)
- [Telegram as Agent Interface](#telegram-as-agent-interface)
  - [Why Telegram Over SSH](#why-telegram-over-ssh)
  - [Project Architecture](#project-architecture)
- [Comparison with OpenClaw](#comparison-with-openclaw)
- [Operational Considerations](#operational-considerations)
  - [Monitoring and Heartbeats](#monitoring-and-heartbeats)
  - [Mission Files](#mission-files)
  - [Autonomy Patterns](#autonomy-patterns)
- [Security Considerations](#security-considerations)
- [Cost Structure](#cost-structure)
- [Sources](#sources)

---

## The Problem

AI coding agents are typically operated through desktop terminals or IDEs. But developers aren't always at their workstations. The ability to monitor, direct, and control agents from anywhere—a restaurant, commute, or away from the desk—creates new workflow possibilities.

SSH is the traditional remote access method, but typing on a mobile phone inside an SSH session is cumbersome. Messaging applications provide a more natural mobile-first interface for agent interaction.

## Telegram as Agent Interface

### Why Telegram Over SSH

The core insight: typing on mobile is optimized for messaging apps, not terminal emulators.

> "More annoying to type inside SSH on iPhone than Telegram"

Telegram provides:
- Native mobile keyboard experience
- Message history and scrollback
- Notification handling
- No session management overhead
- Available anywhere with internet access

Use case example: directing an agent while at dinner at 7pm, away from any workstation.

### Project Architecture

The open-source project referenced (claw-telegram-bot) functions as a Telegram plugin for Claude Code:

- **OSS and free** - no logins or payments required for the bot itself
- **Lightweight** - explicitly simpler than full orchestration tools like OpenClaw
- **Direct Claude Code integration** - wraps Claude Code rather than building a separate agent

The simplicity is intentional—a minimal bridge between Telegram messages and Claude Code commands.

## Comparison with OpenClaw

OpenClaw and the Telegram plugin serve different purposes:

| Aspect | Telegram Plugin | OpenClaw |
|--------|-----------------|----------|
| Complexity | Minimal wrapper | Full orchestration |
| Verbosity | Less messaging | More status updates during execution |
| Authentication | None (bot-level only) | User authentication |
| Payment | None | Paid service |
| Use case | Remote triggering | Automated multi-step workflows |

The Telegram approach is positioned as "simpler" for users who want mobile access without full orchestration capabilities.

## Operational Considerations

### Monitoring and Heartbeats

Running agents autonomously requires monitoring. Referenced patterns:

- **Cron-style heartbeat**: Run agent checks on a schedule (similar to OpenClaw's heartbeat pattern)
- **Minute-level monitoring**: A script that "checks stuff every minute" to verify agent health
- **Error tracking**: Reliability reported as "literally only made a mistake once in 12 months"

### Mission Files

A "SOUL .md" file pattern is suggested for giving agents persistent context:

- Contains the agent's mission and operating parameters
- Provides consistent context across sessions
- Similar to CLAUDE.md but focused on autonomous operation goals
- Read by the agent at startup to establish behavioral boundaries

### Autonomy Patterns

The Telegram interface enables several autonomy patterns:

1. **Repo access + PR creation**: Give the agent repository access and let it submit PRs
2. **Scheduled execution**: Run on cron-like schedules for routine tasks
3. **Remote triggering**: Kick off work sessions from mobile when needed

## Security Considerations

Running agents with remote access raises security concerns:

> "I'm not convinced it's secure yet"

Key risks identified:

- **Prompt injection**: When agents have broader system access, prompt injection becomes more dangerous. The surface area increases when agents can access repositories, create PRs, and interact with external systems.
- **Authentication scope**: Bot-level security vs user-level security tradeoffs
- **Action scope**: What can the agent do if compromised?

Mitigation approaches:
- Limit agent permissions to specific repositories
- Use separate credentials with minimal scope
- Monitor agent actions via logs and notifications
- Review PRs before merging (human in the loop)

The author notes heightened awareness of prompt injection risks when running agents against broader systems, suggesting security remains an active concern.

## Cost Structure

The operational cost is separate from the interface:

- **Telegram plugin**: Free/OSS
- **Claude API usage**: ~$100/month reported for active usage

The cost is in the underlying model API consumption, not the access interface.

## Sources

1. [Levelsio - Telegram Claude Code Bot Discussion](https://x.com/i/status/2023960820091154569)
