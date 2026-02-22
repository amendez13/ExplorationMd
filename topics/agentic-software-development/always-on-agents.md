[← Back to Topic](README.md) | [← Home](../../README.md)

# Always-On Agents: OpenClaw After 50 Days

A comprehensive first-hand account of running an always-on AI agent system for over 50 days, covering practical use cases, architectural evolution, honest failure modes, and lessons learned.

## Index

- [What is OpenClaw](#what-is-openclaw)
- [The Evolution Over Time](#the-evolution-over-time)
  - [Week One: Novelty Phase](#week-one-novelty-phase)
  - [Week Three: Building Automations](#week-three-building-automations)
  - [Week Five: Context Pollution](#week-five-context-pollution)
  - [Week Seven: Model Matching](#week-seven-model-matching)
  - [Week Eight and Beyond: System Thinking](#week-eight-and-beyond-system-thinking)
- [Three Foundational Principles](#three-foundational-principles)
- [Daily Automations](#daily-automations)
  - [Morning Briefings](#morning-briefings)
  - [Historical Learning Display](#historical-learning-display)
  - [Self-Updating and Backups](#self-updating-and-backups)
  - [Background Health Checks](#background-health-checks)
- [Research and Content Creation](#research-and-content-creation)
  - [Parallel Sub-Agent Research](#parallel-sub-agent-research)
  - [YouTube Analytics](#youtube-analytics)
  - [Accumulated Research Over Time](#accumulated-research-over-time)
- [Daily Life Assistant](#daily-life-assistant)
  - [Email Triage](#email-triage)
  - [Calendar and Family Management](#calendar-and-family-management)
  - [Voice Note Transcription](#voice-note-transcription)
  - [Location-Based Queries](#location-based-queries)
- [Infrastructure and DevOps](#infrastructure-and-devops)
- [Knowledge Base Architecture](#knowledge-base-architecture)
  - [Markdown-First Philosophy](#markdown-first-philosophy)
  - [Obsidian Integration](#obsidian-integration)
  - [Nightly Semantic Indexing](#nightly-semantic-indexing)
  - [Bookmark Replacement](#bookmark-replacement)
- [Channel Architecture](#channel-architecture)
  - [Why Discord](#why-discord)
  - [Per-Channel Model Routing](#per-channel-model-routing)
  - [Context Separation](#context-separation)
- [What Breaks and How](#what-breaks-and-how)
  - [Memory Loss and Context Compaction](#memory-loss-and-context-compaction)
  - [Cost Reality](#cost-reality)
  - [The "What Do I Use It For" Problem](#the-what-do-i-use-it-for-problem)
  - [Tasks That Need Babysitting](#tasks-that-need-babysitting)
  - [Security Concerns](#security-concerns)
- [Mitigation Strategies](#mitigation-strategies)
- [Assessment](#assessment)
- [Getting Started](#getting-started)
- [Sources](#sources)

---

## What is OpenClaw

OpenClaw is an always-on AI agent that runs on your own infrastructure—a server, VPS, Mac Mini, or even a Raspberry Pi—24/7. It connects to messaging apps you already have on your phone (Telegram, WhatsApp, Discord, iMessage), enabling continuous agent access without being at a workstation.

The tool has gone through several iterations: ClawdBot → MoltBot → OpenClaw (with a recent OpenAI Foundation shift). The core value proposition is persistent, accessible AI assistance that integrates with your existing communication patterns.

## The Evolution Over Time

Running an always-on agent reveals distinct phases of usage that differ dramatically from week one to week seven.

### Week One: Novelty Phase

Initial usage resembles a ChatGPT replacement—random questions, testing capabilities, exploring what's possible. The critical early decision: **markdown-first**. Many users build workflows around SQLite, databases, vector stores, and custom schemas. Starting with plain text Obsidian files means:

- Any person can read them
- Any program can work with them
- When the next tool iteration arrives, data moves in seconds
- No lock-in, just files

### Week Three: Building Automations

This is when practical value emerges: morning briefings, background checks, research workflows. The agent starts feeling genuinely useful.

### Week Five: Context Pollution

A wall appears. Everything is in one conversation—research, bookmarks, analytics, daily tasks—all mixed together. This creates context pollution that degrades performance and makes retrieval difficult.

The solution: **separate contexts**. One Discord channel per workflow. Research doesn't bleed into analytics. Bookmarks don't pollute daily assistant tasks.

### Week Seven: Model Matching

Not every channel needs the same model. Opus is for deep thinking; cheap models work fine for routine tasks. Matching model to task is when costs stop being overwhelming.

### Week Eight and Beyond: System Thinking

The agent stops being a chatbot and becomes infrastructure. You reorganize your workflow around it rather than fitting it into existing patterns.

## Three Foundational Principles

After 50 days, three principles emerged as essential:

1. **Markdown-first from the beginning** - Plain text files that move with you
2. **Separate contexts** - Different channels/conversations for different workflows
3. **Match model to task** - Use expensive models only where they add value

## Daily Automations

### Morning Briefings

A cron job at 7:00 AM:
- Scans tweets from followed accounts
- Picks the top 10 most relevant
- Writes them to Obsidian notes
- Appends video ideas to a backlog
- Sends a summary

The value compounds over time—it doesn't just summarize, it **connects dots**. Example: "This tweet about model pricing connects to your video idea about cost optimization."

### Historical Learning Display

Every morning, the agent:
1. Fetches Wikipedia's "On This Day" events
2. Picks the most impactful historical event
3. Generates a woodcut-style image showing "10 seconds before" that event happened (iceberg approaching Titanic, apple about to fall on Newton's head)
4. Pushes to an e-ink display in mystery mode (date and location only)

This creates a daily ritual: walk past the display, guess the event, learn something new.

### Self-Updating and Backups

Two automated cron jobs:

**4:00 AM - Self-update:**
- Updates skills from ClawHub
- Updates the OpenClaw package itself
- Restarts the gateway
- Reports results

**4:30 AM - Full backup:**
- Configuration files
- Workflow directory
- Cron schedules
- SOUL file (agent identity)
- MEMORY files
- All skills

If the server dies, recovery takes 30 minutes—not rebuilding from scratch.

### Background Health Checks

Every 30 minutes:
- Scans emails
- Checks calendar
- Monitors services
- Catches things that would otherwise be missed

Example catches:
- Netflix payment failure found during routine email scan
- Domain renewal coming up
- Meeting about to be missed
- Relevant newsletter article connecting to current work

**Critical security note:** The agent runs in **draft-only mode** for email. It can read the inbox, flag importance, and draft responses—but cannot send. No robust general solution exists yet for prompt injection via email, so inbox content is treated as potentially hostile.

## Research and Content Creation

### Parallel Sub-Agent Research

For research-intensive tasks, the agent spawns multiple parallel sub-agents:

Example for video research:
- One searches Twitter
- One crawls Reddit
- One hits Hacker News
- One analyzes YouTube competition
- One scrapes forums

They run simultaneously and produce structured research files—50+ pages for a single video, completed in minutes rather than hours. Each includes competitive analysis, ranked ideas, full outlines, and source links.

### YouTube Analytics

A dedicated Discord channel with YouTube API access enables natural language queries:
- "How did my last five videos compare on retention?"
- "Which topics get the most engagement?"
- "Compare my OpenClaw videos to my Claude Code videos"

The agent synthesizes numbers and provides advice based on data—more flexible than YouTube Studio's built-in dashboards.

### Accumulated Research Over Time

Throughout the week, links, articles, tweets, and half-formed thoughts get dropped into a research channel. The agent:
- Enriches snippets
- Connects dots across sources
- Builds context over time

By the time work begins, weeks of accumulated, organized research is waiting.

## Daily Life Assistant

### Email Triage

Beyond proactive catches, the day-to-day workflow:
1. Agent reads inbox
2. Flags what's important
3. Drafts responses
4. Human reviews and sends

This enables same-day email responses rather than weekend catch-up sessions.

### Calendar and Family Management

Works for the whole family. Example: a WhatsApp group where family members can:
- Drop messages like "Schedule Dentist Thursday at 3:00 PM"
- Add events
- Check schedule
- Get reminders

WhatsApp is used here because family members who don't use Telegram or Discord can participate.

### Voice Note Transcription

Send a voice message on WhatsApp, Telegram, or Discord; it transcribes with Whisper and responds in text. Use cases:
- Quick thoughts while driving
- Shopping lists while walking
- Meeting notes on the go

The agent can also take action on transcribed content—putting important items in Obsidian files, drafting emails, or sending messages without requiring manual follow-up.

### Location-Based Queries

Using Google Places API:
- Find coffee shops within walking distance
- Check ratings, reviews, prices
- Filter by specific criteria (example: finding snowboard boots in a specific size across multiple shops, checking online availability)

### Weather and Reminders

Beyond basic forecasts, useful proactive alerts:
- Cold snap warnings (e.g., -19°C incoming)
- Rehab exercise reminders with snooze capability
- Meeting reminders before weekly calls

Small things individually, but they compound.

## Infrastructure and DevOps

The agent migrated from the old ClawdBot package to OpenClaw:
- Found both packages running simultaneously
- Killed a zombie process running at 160% CPU
- Deleted old system services
- Fixed seven days of silently broken cron jobs

All from one message: "go fix everything."

With VPS API access:
- Review running applications
- Flag unhealthy services
- Fix and restart apps
- Full server remote control via Discord

**Security:** Allowlist of permitted commands, denylist of commands requiring explicit permission.

Phone-based coding becomes possible: fix bugs, build features, create PRs—all from mobile while away from the desk. Recommended primarily for quick fixes and simple ideas rather than production work.

## Knowledge Base Architecture

### Markdown-First Philosophy

Everything ends up in Obsidian as plain text files:
- Agent organizes them automatically
- Nightly index rebuilds
- Semantic search available

This is characterized as a "second brain" that's always on and handles all organization.

### Obsidian Integration

With ~3000 notes, the agent indexes everything nightly using QMD for semantic search. Semantic search enables questions like:
- "What did I decide about thumbnail design last month?"
- "What's the overarching theme in my last five videos?"

This is meaning-based retrieval, not keyword matching.

### Nightly Semantic Indexing

At 3:00 AM, the entire embedding index rebuilds. Initial setup took a few minutes for ~2500 notes; now updates take about 10 seconds.

The knowledge base grows every night as daily interactions are captured and organized.

### Bookmark Replacement

Previously used Raindrop (paid subscription, separate app, manual tagging, folders). Built a system pulling bookmarks via API to Obsidian, plus a skill for automatic enrichment.

Realized the intermediary was unnecessary. Now:
1. Drop any link into Discord inbox channel
2. Agent summarizes content
3. Extracts key information
4. Creates tags for every bookmark
5. Builds a knowledge graph connecting related links
6. All in markdown, all searchable, all building context over time
7. Runs on a cheaper model (link processing doesn't need Opus)

Raindrop subscription canceled.

## Channel Architecture

### Why Discord

Migration to Discord was the biggest structural change. Started on WhatsApp, moved to Telegram for features, hit a wall around week five when everything was in one conversation.

Discord provides:
- Channels as dedicated workspaces with own context
- Different models per channel
- Better formatting
- Lower costs through model matching

The switch wasn't about the app itself—it was about the architecture.

### Per-Channel Model Routing

Example channel configuration:
- **YouTube stats channel**: Cheap model (mostly data retrieval)
- **Research channel**: Opus (deep thinking)
- **Inbox channel**: Fast cheap model (processing links, summarizing, categorizing)

This is how costs become manageable—matching model to task, not overpaying for capabilities the task doesn't need.

### Context Separation

Each channel stays focused:
- Channel for YouTube analytics
- Channel for video idea research
- Inbox channel for bookmarks
- General channel for daily assistant

Context pollution eliminated. Always know where to go for what.

Be careful about adding more channels—only add when genuinely needed.

## What Breaks and How

### Memory Loss and Context Compaction

The number one technical frustration: the agent forgets things mid-conversation with no warning. It drifts, loses track, then replies to something from three sentences back.

**Silent compaction** is the culprit. The context window fills up, the agent compresses the conversation, and important details disappear. ChatGPT at least warns when context is long; OpenClaw just silently compresses.

Mitigation:
- Write everything to files
- Use QMD for semantic search
- Run `/compact` manually before the system does it automatically
- Use the status command to check remaining context
- If over 50% full, start a new session

### Cost Reality

Opus is amazing but expensive. The answer is multi-model routing:
- Use Opus for real thinking
- Use cheaper models for heartbeats and sub-agents
- Discord channel setup built around matching model to task

Cost must be planned for—it's real money.

### The "What Do I Use It For" Problem

The most common question on Reddit and Discord. The harsh truth: **if you don't have workflows to automate, OpenClaw won't invent them for you.**

- If you don't manage your calendar, an AI calendar manager won't help
- If you don't check email, AI email triage is pointless

People getting the most value already had systems—OpenClaw made their systems easier, faster, and automatic. The systems came first.

### Tasks That Need Babysitting

Complex multi-step tasks still fail or need pushing:
- Browser automation is flaky
- Sessions and extensions drop
- Agents sometimes go silent mid-task (need to ask "Hey, how's it going?")

Works better as an assistant than an autonomous agent, at least for now. Simpler tasks are more reliable; complexity requires check-ins and detailed instructions.

**Sub-agents help with compaction**: Explicitly launch sub-agents for research. Each sub-agent gets its own context window, so research doesn't fill the main context. The main agent only does coordination while sub-agents do the work.

### Security Concerns

No foolproof general solution for prompt injection exists. Inbox content is treated as hostile or potentially hostile.

If your agent reads untrusted emails, someone could craft a message that makes your agent do something unintended.

Mitigations:
- Don't expose anything to the outside world
- Keep machines on Tailscale
- Draft-only email mode
- Approval needed for destructive actions
- Run security audits regularly (use ClawHub's security check or point the agent at the OpenClaw security documentation)

## Mitigation Strategies

For sub-agent context management, explicitly launch sub-agents so:
- The orchestrator launches sub-agents
- Each sub-agent gets its own context window
- Sub-agents don't fill the main context window
- Main agent only does coordination

For security, implement everything in the official security documentation and verify with the agent.

## Assessment

**Setup difficulty:** 7/10 (intentionally not easier—it can be dangerous if misused)

**Daily value once running:** 9/10

**Reliability for simple workflows:** 8/10

**Reliability for complex browser tasks:** 5/10

**Best feature:** Discord channel architecture with per-channel models

**Biggest unlock:** File-based memory with markdown-first approach and nightly semantic indexing

**Most quietly useful:** Background heartbeat checks

**Biggest pain:** Memory and context compaction

**Surprise:** It gets better over time. The more context in SOUL and MEMORY files, the better it understands you. After 50 days, it anticipates needs and internalizes tiny style preferences.

### What It Replaced

Expected to replace ChatGPT, but also replaced:
- Parts of Zapier
- Raindrop (bookmarks)
- Parts of YouTube Studio analytics
- Parts of web analytics
- Half of Apple Shortcuts

For personal use: no longer paying for Zapier or Raindrop.

### Who Should Use It

**Yes, if:**
- You have workflows to automate
- You're comfortable with a terminal
- You understand the cost implications

**Not yet, if:**
- You want something that just works out of the box
- You're not technical
- You expect fully autonomous AI that never needs babysitting

The capability is already there. The onboarding isn't, by design (for now). Once security hardens and setup gets easier, everyone will run some version of this.

## Getting Started

Start with these three things for one week:

1. **Draft-only email triage with urgent alerts** - catches things you miss
2. **Daily briefing that writes to a markdown file** - morning context, organized automatically
3. **One Discord inbox channel for bookmarks** - drop links, agent enriches them, replaces paid apps immediately, builds knowledge base over time

Everything else grows from there.

## Sources

1. [Rad - 20 Real Use Cases After 50 Days with OpenClaw](https://youtu.be/NZ1mKAWJPr4?si=p4xemg5ogNpKWfc4)
