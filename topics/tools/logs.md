[← Back to Topic](README.md) | [← Home](../../README.md)

# Tools - Source Log

*Chronological record of sources for this topic*

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
