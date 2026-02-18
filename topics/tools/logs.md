[← Back to Topic](README.md) | [← Home](../../README.md)

# Tools - Source Log

*Chronological record of sources for this topic*

---

### [Favorite MCP servers: Exa and Ref](note://78)
*2026-02-14* | Tags: tools, mcp, model-context-protocol, exa, ref-tools, search, documentation

Short note that two MCP servers (Exa and Ref) have remained the author’s favorites “for months,” framed as notable stability in a fast-moving agent tooling ecosystem.

**Key Points:**
- Calls out Exa and Ref as durable defaults for MCP-based workflows
- Implies long-lived utility versus churn in the MCP server landscape

---

### [Exa MCP Server (GitHub repository)](https://github.com/exa-labs/exa-mcp-server)
*2026-02-14* | Tags: tools, mcp, exa, web-search, code-search, research, http

Repository documenting Exa’s hosted MCP endpoint and available tools for web search, code context search, and research workflows.

**Key Points:**
- Documents hosted MCP endpoint `https://mcp.exa.ai/mcp` and `npx`-based setup options
- Lists default and opt-in tools (web search, code context, company/person research, deep researcher)
- Includes client-specific configuration examples (Cursor, VS Code, Claude Code, etc.)

---

### [Model Context Protocol servers list (community servers)](https://github.com/modelcontextprotocol/servers)
*2026-02-14* | Tags: tools, mcp, registry, server-list, community

Repository README that links to community-built MCP servers and related resources, including Exa in its third-party server list.

**Key Points:**
- Positions the repo as a hub for reference implementations and community servers
- Links to the MCP Registry for browsing published servers
- Includes an entry linking to Exa’s MCP server repository

---

### [Ref MCP (GitHub repository)](https://github.com/ref-tools/ref-tools-mcp)
*2026-02-14* | Tags: tools, mcp, ref-tools, documentation, retrieval, token-efficiency

Repository for Ref’s MCP server focused on documentation search and URL reading, with emphasis on session-aware retrieval and minimizing unnecessary context tokens.

**Key Points:**
- Documents hosted HTTP setup (`https://api.ref.tools/mcp?apiKey=...`) and local stdio setup via `npx`
- Defines tools like `ref_search_documentation` and `ref_read_url`
- Describes session-based filtering and partial-page reading to reduce context size

---

### [ref.tools](https://ref.tools/)
*2026-02-14* | Tags: tools, mcp, ref-tools, documentation, agentic-search

Ref’s product site describing the tool as “context for your coding agent” and the broader approach to documentation retrieval for agents.

**Key Points:**
- Positions Ref as an agent-oriented documentation context layer
- Serves as the primary product entry point (pricing/signup/docs links)

---

### [summarize.sh](https://summarize.sh/)
*2026-02-14* | Tags: tools, summarize, summarization, cli, chrome-extension, firefox, extraction, media

Project page for Summarize, describing it as a fast extraction + summarization tool available both as a CLI and as a browser side panel extension.

**Key Points:**
- Positions Summarize as “summaries that live where you work” (CLI + Chrome Side Panel)
- Emphasizes extraction pipeline quality (link/file/media → clean text → summary)
- Links to docs and the open-source repository for installation and configuration

---

### [Summarize Docs: Cache (design)](https://summarize.sh/docs/cache.html)
*2026-02-14* | Tags: tools, summarize, caching, sqlite, offline-first, media-cache

Documentation of Summarize’s cache design: a CLI-only SQLite database for transcripts/extractions/summaries plus a separate file cache for downloaded media.

**Key Points:**
- Default SQLite cache path is `~/.summarize/cache.sqlite`, with TTL and size caps
- Separate media download cache under `~/.summarize/cache/media` with its own TTL/size limits
- Cache keys incorporate inputs and settings (e.g., model/length/language) to avoid stale reuse

---

### [Summarize Docs: Browser Side Panel (Chrome + Firefox Extension + Daemon)](https://summarize.sh/docs/chrome-extension.html)
*2026-02-14* | Tags: tools, summarize, chrome-extension, firefox, daemon, localhost, sse

Architecture and setup notes for the browser side panel: the extension streams summaries but relies on a local daemon for extraction and media tooling.

**Key Points:**
- Uses a local, token-authenticated daemon on `127.0.0.1` (autostart via launchd/systemd/Scheduled Task)
- Streams results (SSE) for in-panel rendering and “live” summaries
- Explains page-vs-media routing: the daemon chooses the best pipeline for the current tab

---

### [Summarize Docs: YouTube mode](https://summarize.sh/docs/youtube.html)
*2026-02-14* | Tags: tools, summarize, youtube, transcripts, whisper, yt-dlp, slides, ocr

YouTube-specific extraction behavior and fallbacks, including transcript-first modes and slide screenshot extraction.

**Key Points:**
- Transcript-first flow with selectable strategies (`--youtube auto|web|no-auto|apify|yt-dlp`)
- `yt-dlp` mode can download audio and transcribe via local `whisper.cpp` (preferred) with cloud fallbacks
- `--slides` extracts slide screenshots; optional OCR via `--slides-ocr`

---

### [Summarize Docs: CLI models](https://summarize.sh/docs/cli.html)
*2026-02-14* | Tags: tools, summarize, cli, model-routing, cursor, codex, claude, gemini

Documentation for using installed CLIs as model backends (Codex, Claude, Gemini, Cursor Agent) and configuring auto-fallback behavior.

**Key Points:**
- Defines `cli/<provider>/<model>` ids and the `--cli` shortcut selection
- Auto-mode supports configurable CLI fallback and persists the last-success provider
- Cursor Agent runs via an `agent` binary and can rely on Cursor auth

---

### [Summarize GitHub Release v0.11.1 (includes v0.11.0 highlights)](https://github.com/steipete/summarize/releases/tag/v0.11.1)
*2026-02-14* | Tags: tools, summarize, release-notes, groq, whisper, cursor-agent, caching

Release notes covering v0.11.0/v0.11.1, including CLI-provider additions and media/transcription reliability improvements.

**Key Points:**
- Makes Groq Whisper the preferred cloud transcriber and improves fallback flow
- Adds a Cursor Agent CLI provider (`--cli agent`) and more explicit auto CLI fallback controls
- Includes cache behavior tweaks for auto presets to avoid stale per-candidate cache hits

---

### [Summarize GitHub Repository](https://github.com/steipete/summarize)
*2026-02-14* | Tags: tools, summarize, open-source, npm, homebrew, daemon, chrome-extension

Repository with installation instructions, feature overview (URLs/files/media), and the CLI/extension architecture details.

**Key Points:**
- Installs via npm (`npm i -g @steipete/summarize`) or Homebrew (`brew install steipete/tap/summarize`, macOS arm64)
- Supports URLs, local files, and media sources like YouTube and podcasts
- Documents a local-daemon design for the browser side panel and automation-friendly CLI modes

---

### [OpenUsage - AI Limits Tracker for Cursor, Claude Code, Codex and more](https://www.openusage.ai/)
*2026-02-12* | Tags: tools, openusage, usage-tracking, limits, quota, menu-bar, open-source, plugin, providers, macos

OpenUsage is positioned as a lightweight menu bar app for monitoring AI tool usage limits without leaving your coding workflow. The landing page emphasizes background refresh, minimal setup, and a provider-as-plugin approach.

**Key Points:**
- Focuses on avoiding surprise lockouts by showing where you stand against limits
- Presents itself as "one download" from GitHub with minimal setup effort
- Claims automatic discovery of certain accounts (e.g., Cursor, Claude Code)
- Highlights a plugin-based, open-source model to keep provider support up to date
- Notes a Tauri/React/TypeScript implementation stack

---

### [OpenUsage GitHub Repository](https://github.com/robinebers/openusage)
*2026-02-12* | Tags: tools, openusage, usage-tracking, menu-bar, plugins, providers, releases, macos

The project repository documents OpenUsage’s feature set (menu bar UI, scheduled refresh, plugin-based providers) and provides downloads via GitHub releases. It also enumerates supported providers and contribution paths for adding new provider plugins.

**Key Points:**
- Distributes via GitHub releases for macOS; states the app auto-updates after install
- Describes a "one glance" menu bar UX with progress bars and labels
- Lists a broad set of supported providers (Amp, Claude, Codex, Copilot, Cursor, Windsurf, etc.)
- Encourages contributions via a provider plugin model and a Plugin API
- Calls out Windows/Linux as a high-priority TODO needing testers

---

### Most-Used AI Agent Skills (Global and Project-Dependent)
*2026-02-13T00:00:00Z* | Tags: tools, ai-agents, skills, claude-code, codex, react, convex, tauri, swiftui, swift-concurrency

Summary of frequently-used AI agent skills for coding assistants, organized by global (cross-project) and project-dependent categories.

**Key Points:**
- Global skills: vercel-react-best-practices (React/Next.js optimization), brainstorming (structured requirement gathering), convex-best-practices (Convex patterns), frontend-design (avoiding generic AI aesthetics)
- Project-dependent: tauri-v2 (cross-platform desktop), swiftui-expert-skill (modern SwiftUI APIs), swift-concurrency (Swift 6 migration), swiftui-liquid-glass (iOS 26+ glass effects), swiftui-skills (Apple documentation patterns)
- Skills are modular packages that extend AI assistants with domain expertise
- Install via `npx add-skill <org>/<repo>` for most skills

---

### [Obsidian "file over app" notes workflow (Apple Notes migration, iCloud sync, CLI search)](https://x.com/i/status/2021534318506512672)
*2026-02-12* | Tags: tools, obsidian, markdown, pkm, note-taking, file-over-app, apple-notes, migration, icloud, cli, publish

Spanish thread describing a switch from Apple Notes to Obsidian for a file-first, Markdown-based notes workflow. Highlights reduced vendor lock-in, practical migration notes, and new CLI-based search possibilities.

**Key Points:**
- Prefers "file over app": plain-text Markdown files on disk, reducing lock-in and making reuse easy
- Syncs across devices via iCloud (no Obsidian Sync needed for this setup); notes are not stored in an Obsidian cloud
- Apple Notes import: unlock encrypted notes first; review import logs; some manual cleanup may be required
- Corrects support plan: considering Obsidian Publish (not Sync); mentions Publish starting at $25 USD
- Notes new Obsidian CLI (initially Catalyst) enabling more efficient search, potentially useful for AI agent workflows

---

### [Local caching, transcript gaps, and privacy tradeoffs in media summarization](https://x.com/i/status/2022513027870810384)
*2026-02-14* | Tags: tools, summarization, offline-first, caching, privacy, transcripts, tts, performance, residential-ip

Short thread of implementation notes about a media summarization workflow: some inputs lack transcripts so TTS is used as a fallback, outputs are cached locally, and the summarizer is described as having “no phone home” behavior. The author also flags server-side caching as potentially problematic and notes the system can be slow.

**Key Points:**
- Not all tracks have transcripts; TTS is used as fallback
- Caches locally; “summarize has no phone home feature”
- Server-side caching may introduce problems (implied privacy or correctness concerns)
- Network assumptions may matter (mentioned as being meant for residential IP)
- Performance can be a limiting factor (“slooooooow”)

---

### [AI Prompt Coach (announcement thread)](https://x.com/i/status/2021570677162340560)
*2026-02-14T09:51:10Z* | Tags: tools, prompt-engineering, ai-coding, prompts, coaching

Short promo thread referencing “AI Prompt Coach”, described as a free tool for improving AI coding prompts using 15 principles derived from Anthropic and OpenAI research.

**Key Points:**
- Positions the tool as a “prompt coach” for AI coding prompts
- Claims the tool is free and based on 15 principles
- Links to the tool landing page (via a URL shortener)

---

### [AI Prompt Coach (tool site)](https://coach.robinebers.com/)
*2026-02-14T09:51:10Z* | Tags: tools, prompt-engineering, ai-coding, prompts, rubric, scoring

Landing page for AI Prompt Coach, a web app that analyzes prompts and returns a score, per-principle feedback, and example prompts with scores.

**Key Points:**
- Includes an “Analyze My Prompt” flow and notes results can take ~20–30 seconds to generate
- Shows example prompts with numeric scores (cards and a prompt/score table)
- Includes opt-in prompt sharing and a disabled “web search with Perplexity” toggle in the page HTML

---

### [AI Prompt Coach example evaluation (principle breakdown)](https://coach.robinebers.com/p/j579efb0x2bpbx9m1jmzac4yeh7x2b1c)
*2026-02-14T09:51:10Z* | Tags: tools, prompt-engineering, rubric, scoring, best-practices

Public example evaluation page showing a 15-principle rubric and per-principle scoring, reasoning, and suggestions (including non-applicable items).

**Key Points:**
- Exposes the 15 principle names used by the coach (e.g., "Name Your Tools and Versions", "Take It One Step at a Time")
- Provides per-principle pass/fail, reasoning, suggestions, and individual scores
- Marks some principles as non-applicable depending on the prompt type (e.g., debugging vs. greenfield)

---

### [Claude Code Telegram Bot (GitHub Repository)](https://github.com/RichardAtCT/claude-code-telegram)
*2026-02-18* | Tags: tools, telegram, claude-code, remote-access, mobile, bot, ai-workflow

Telegram bot providing remote access to Claude Code through natural language chat. Enables mobile-friendly codebase interaction, GitHub webhook notifications, and scheduled tasks.

**Key Points:**
- Two operating modes: Agentic (conversational) and Classic (terminal-like commands)
- Full Claude Code integration via Python SDK with CLI fallback
- Multi-layer security: user whitelist, directory sandboxing, rate limiting, audit logging
- FastAPI webhook server with GitHub HMAC-SHA256 verification
- Cron-based scheduler for automated codebase health checks and notifications
- Session export in Markdown, HTML, and JSON formats
- Usage and cost tracking per user

---
