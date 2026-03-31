[← Back to Topic](README.md) | [← Home](../../README.md)

# OpenClaw: A Practical Guide to Building and Operating a Personal Agent

This article synthesizes a complete “getting started” guide for OpenClaw: what it is, how to set it up safely, how onboarding and memory work, and how to grow from one agent to a role-based team. [1]

## Index

- [What OpenClaw Is](#what-openclaw-is)
- [Safe Setup: Where to Run It](#safe-setup-where-to-run-it)
- [Install and Onboarding Checklist](#install-and-onboarding-checklist)
- [First Chat: Defining the Job and Capturing Memory](#first-chat-defining-the-job-and-capturing-memory)
- [Starter Workflows (Copy/Paste Prompts)](#starter-workflows-copypaste-prompts)
- [Multi-Agent Teams](#multi-agent-teams)
- [Optimization: Integrations, Maintenance, Security, Cost](#optimization-integrations-maintenance-security-cost)
- [Sources](#sources)

---

## What OpenClaw Is

OpenClaw is presented as an always-on, locally-run personal AI assistant that you “talk to” over chat apps (e.g., Telegram) and that can operate your computer, use web search, and accumulate reusable “skills.” [1]

The important framing is “manager → employee”: treat the agent like a teammate with a defined role, access boundaries, and explicit operating rules. [1]

Related: [Always-On Agents](always-on-agents.md), [Agent Swarm Orchestration](agent-swarm-orchestration.md), and [Mobile Agent Control](mobile-agent-control.md).

## Safe Setup: Where to Run It

The guide strongly recommends *not* installing OpenClaw on a primary work/personal machine because it can have broad file/system access, and mistakes can be costly. [1]

It suggests isolating OpenClaw on one of these approaches (ordered by “easy to get started” rather than universal best): hosted providers, a VPS, or a dedicated machine (the Mac Mini is positioned as a popular, simple option but not required). [1]

## Install and Onboarding Checklist

### Install

The guide’s install command is:

`curl -fsSL https://openclaw.ai/install.sh | bash` [1]

### Onboarding (What to Decide)

Onboarding is described as a guided flow that begins with a security warning and then asks you to configure core capabilities. [1]

Key choices and steps called out in the guide:

- Terminal navigation basics (keyboard-driven selection in the onboarding TUI). [1]
- Model selection (examples given include Claude Opus 4.6 and Codex 5.4, with the intent to pick “best available” over time). [1]
- Provider authentication: either log in with an existing consumer subscription or use an API key; the guide recommends the API-key path and notes you should review provider terms if using consumer accounts. [1]
- A “talk to your agent” channel (Telegram suggested as the beginner-friendly choice). [1]
- Web search (can be enabled during onboarding or later). [1]
- Default skills (the guide explicitly recommends a `gog` skill for Gmail/Google Calendar/Docs, plus summarization). [1]
- Hooks (the guide recommends enabling hooks and calls out session memory as especially important). [1]

### Telegram

Telegram is suggested as the simplest chat interface, including using Telegram’s `@BotFather` as part of setup. [1]

## First Chat: Defining the Job and Capturing Memory

After “hatching,” the guide recommends giving the agent a clear job (personal assistant, social media manager, engineering intern, etc.) and answering questions that let it build an operating model. [1]

It also describes a workspace folder (example: `.openclaw/[agent_name]-workspace`) that stores the agent’s identity and operating rules as Markdown files, which are re-read at startup. [1]

Files explicitly called out:

- `AGENTS.md`: core instructions and memory. [1]
- `SOUL.md`: persona/tone/boundaries. [1]
- `IDENTITY.md`: name and “vibe.” [1]
- `TOOLS.md`: tool usage notes. [1]
- `USER.md`: details about the human user. [1]

Related: [AGENTS.md Library](agents-md-library.md).

## Starter Workflows (Copy/Paste Prompts)

The guide provides “just paste this” examples to make the agent useful quickly. Common patterns in these prompts:

- A trigger (daily/weekly cron-like cadence or “check in the next 30 minutes”). [1]
- A data source (calendar, email, CRM, signup list, support tickets). [1]
- An approval gate before an irreversible action (send/post). [1]
- A write-back action (create a ticket, update a calendar, draft a doc). [1]

Examples included in the guide: weekend coordination and calendar updates, meme ideation and posting, lead triage with enrichment, “just-in-time” meeting briefs, generating docs/FAQ candidates from support tickets, and lightweight project management with weekly retrospectives. [1]

## Multi-Agent Teams

The guide’s biggest “unlock” is running multiple specialized agents rather than one generalist. Each agent can have separate identity, tools, scheduled tasks, and workspace—treated as different employees. [1]

It describes a CLI flow for adding agents (e.g., `openclaw agents add agent_name`) and even transferring memories/tasks from one agent to another (“move marketing crons/memories to the marketing intern”). [1]

The author’s example “team” assigns distinct access scopes and responsibilities per agent (assistant/family manager/marketer/sales/helpdesk/operator/producer/developer/teacher), and uses scheduled routines (“crons”) as the backbone for recurring work. [1]

## Optimization: Integrations, Maintenance, Security, Cost

### Integrations (Where It Gets Leverage)

The guide frames integrations as the difference between “a more complicated Claude Code” and something that feels like a real assistant. Example integrations mentioned include:

- Google suite access via a `gog` CLI tool (email/calendar/docs) with explicit permission scoping. [1]
- Web search providers (Brave API preloaded; alternatives like Exa/Perplexity/Firecrawl mentioned). [1]
- GitHub access via narrowly scoped tokens for “on-demand developer” behavior. [1]
- Linear tokens for task assignment and workflow handoffs. [1]
- Obsidian as a shared Markdown-first collaboration space. [1]
- Home automation examples (Eight Sleep, Sonos, lighting). [1]

The safety recommendation is to start with read-only tokens and only expand permissions once you trust the setup and have explicit behavioral rules. [1]

### Maintenance (“Care and Feeding”)

The guide expects drift: forgetting, broken scheduled jobs, and the need to ask the agent to inspect/fix its own configs (especially `TOOLS.md` and `SOUL.md`). [1]

It also suggests keeping a way to reach the machine’s terminal (e.g., remote access to the device) and using a separate coding agent (e.g., Claude Code) to repair a broken OpenClaw workspace when needed. [1]

### Security

Security themes emphasized in the guide:

- Keep OpenClaw updated and run built-in audits (commands mentioned: `openclaw update` and `openclaw security audit`). [1]
- Avoid group chats/public channels for beginners because any participant can instruct the bot. [1]
- Treat external content as hostile: if the agent reads email/websites, it can be exposed to prompt injection; reinforce “don’t exfiltrate secrets” rules in `SOUL.md`. [1]
- Constrain external communications (email/SMS APIs) to prevent impersonation or unintended outreach. [1]
- Store secrets carefully (example mentioned: `.openclaw/.env`). [1]
- Prefer official/bundled skills or trusted developers; review `SKILL.md` before running community skills. [1]

### Cost

The guide warns cost can grow quickly with API-based usage across multiple agents, but notes many users may be fine on a fixed subscription for some workflows; it frames spending as a tradeoff against hiring human labor. [1]

## Sources

1. [lennysnewsletter.com](https://www.lennysnewsletter.com/p/openclaw-the-complete-guide-to-building)
