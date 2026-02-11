[← Back to Topic](README.md) | [← Home](../../README.md)

# Agentic Software Development - Source Log

*Chronological record of sources for this topic*

---

### [Claude Code Power User Tips from the Creator](https://x.com/bcherny/status/2017742759218794768)
*2026-02-10* | Tags: claude-code, workflow, parallelism, prompting, skills, environment-setup

Boris Cherny (creator of Claude Code) shares 10 categories of tips sourced from the Claude Code team. Emphasizes that there's no single right way to use Claude Code - experimentation is key.

**Key Points:**
- Parallel worktrees (3-5 at once) is the single biggest productivity unlock; native support built into Claude Desktop
- Start complex tasks in plan mode; pour energy into the plan so Claude can 1-shot implementation
- Invest in CLAUDE.md - after every correction, tell Claude to update it; ruthlessly iterate over time
- Create reusable skills for anything done more than once a day; commit to git for cross-project use
- Claude can autonomously fix bugs: paste Slack thread via MCP, say "fix", or "go fix failing CI tests"
- Advanced prompting: "grill me on these changes", "prove this works", "scrap this and implement the elegant solution"
- Environment: Ghostty terminal, voice dictation (3x faster than typing), /statusline for context usage
- Use subagents to throw more compute at problems and keep main context window clean
- Use Claude for data/analytics via bq CLI or database MCPs - no manual SQL needed
- Enable "Explanatory" output style for learning; generate HTML presentations or ASCII diagrams

---

### [ESR on LLM-Assisted Development Workflow](https://x.com/esrtweet/status/2019391670609940746)
*2026-02-10* | Tags: workflow, mindset, designer-notes, solo-developer, codex

Eric S. Raymond (ESR) shares insights from a discussion about his LLM-assisted development workflow. He emphasizes that effective use of coding agents is fundamentally about mindset, not tools. Key practices include maintaining "designer notes" in project source trees that serve as context for both LLMs and human contributors.

**Key Points:**
- Mindset matters more than tools - don't over-focus on which LLM or agent you use
- Include "designer notes" in project source trees to provide context for LLMs
- These notes also help human contributors onboard themselves
- Direct the LLM to read relevant files at the start of a session
- The LLM treats the entire code directory as context and figures out what to examine
- Prefers Codex CLI over Aider because it doesn't require explicit file selection
- Keep tools simple - use them "in the dumbest and most obvious way"
- Don't force editor integration if the terminal emulator has limitations
- Compression factor is high - features that wouldn't have been worth manual effort become trivial
- Example: Added integration with repology.org API in minutes, a task that would have been "annoyingly long" by hand
- Context: Actively maintaining 50+ projects, making automation particularly valuable
- Core code is often implied by data structures and natural operations on them

---

### [OpenAI's Internal Approach to Agentic Software Development](https://x.com/gdb/status/2019566641491963946)
*2026-02-10* | Tags: organizational-adoption, team-practices, codex, openai

Greg Brockman (OpenAI President) shares how OpenAI is retooling their teams towards agentic software development. He notes that since December there's been a step function improvement in what tools like Codex can do - engineers report their jobs have fundamentally changed, with agents now writing essentially all code and handling operations and debugging.

**Key Points:**
- Goal by March 31st: agents should be the tool of first resort for any technical task
- Designate an "agents captain" per team responsible for workflow integration
- Create and maintain AGENTS.md files; update when agent struggles with tasks
- Write reusable skills and commit to shared repositories
- Make internal tools agent-accessible via CLI or MCP servers
- Structure codebases to be agent-first with quick tests and clean interfaces
- "Say no to slop" - maintain code review standards for AI-generated code
- Build observability infrastructure around agent workflows

---

### [Autonomous Agent Prompts for Continuous Project Improvement](https://x.com/doodlestein/status/1999934160442687526)
*2026-02-10* | Tags: autonomous-agents, prompts, code-review, ux-polish, multi-project, codex

Developer shares prompts used to keep multiple projects advancing daily through autonomous agent work. With sufficient unit tests and e2e integration tests, agents can explore, find bugs, and fix issues without going rogue. Runs this workflow across 7+ projects on 3 machines, using a command palette device to trigger prompts instantly.

**Key Points:**
- Use prompts that have agents "randomly explore" code files, trace execution flows, and fix obvious bugs with "fresh eyes"
- Have agents cross-review code written by other agents, looking for bugs, inefficiencies, security issues
- When dissatisfied with a project, prompt agents to scrutinize UX and suggest "Stripe-level" polish improvements
- Queue follow-up prompts to elaborate suggestions into granular tasks with dependency structure
- Include a "plan space" review step before implementation to catch suboptimal task design
- End with a "fresh eyes" review of all new code for obvious bugs
- Have Claude commit changes in logically grouped commits with detailed messages
- Works best with Opus 4.5 or GPT 5.2 with high thinking effort
- Uses hardware command palette for one-button prompt execution

---

### [Agent Scripts - Shared Guardrails and Helper Tools](https://github.com/steipete/agent-scripts)
*2026-02-10* | Tags: agents-md, guardrails, git-workflow, portability, canonical-rules

Peter Steinberger's repository of shared guardrail helpers and scripts for AI agents, maintained as a canonical mirror across multiple development repositories. Demonstrates advanced AGENTS.md architecture with pointer-based rules inheritance.

**Key Points:**
- Use a "canonical mirror" model where one repository is the authoritative source for shared agent rules
- Pointer-based architecture: downstream repos reference shared rules via `READ ~/Projects/agent-scripts/AGENTS.MD BEFORE ANYTHING`
- Helper scripts must remain byte-identical across consuming repositories for consistency
- Key tools: `committer` (safe git commits), `docs-list.ts` (documentation validation with front-matter requirements)
- Portability principles: zero repo-specific imports, no tsconfig aliases, code must run in isolation
- Conventional Commits required; no destructive git ops without explicit consent
- Safe-by-default: review status/diff before pushing; deletions use `trash` command
- File management: keep files under ~500 lines; refactor/split as needed
- Shared vs local rules: shared blocks live in canonical AGENTS.MD, repo-specific additions go after the pointer line
- Sync workflow includes submodule handling and SHA bumping
- Avoid "AI slop" UI - intentional typography, committed color palettes, 1-2 high-impact motion moments

---

### [CodexSkillManager - macOS App for Agent Skills](https://github.com/Dimillian/CodexSkillManager)
*2026-02-10* | Tags: skill-management, macos, codex, claude-code, clawdhub

A macOS SwiftUI app for organizing and managing AI agent skills locally and remotely. Provides a unified interface for skills across Codex and Claude Code environments.

**Key Points:**
- Manages skills from three directories: `~/.codex/skills`, `~/.codex/skills/public`, and `~/.claude/skills`
- Renders skill documentation with Markdown formatting
- Import skills from folders or ZIP archives; delete skills from sidebar
- Browse remote skills from Clawdhub (a skill catalog) with search and recent additions
- Shows installation status indicators for Codex, Claude, or both
- Download remote skills for local use in either agent environment
- Build with Swift: `swift build && swift run CodexSkillManager`
- Requires macOS 26+ and Swift 6.2+

---

### [README Generator Prompt](https://gist.github.com/Shpigford/c7f9a32e8e6a0e0babb3f8dca4268e2f)
*2026-02-10* | Tags: prompts, documentation, readme, technical-writing

Josh Pigford shares a comprehensive prompt/skill for generating "absurdly thorough" README files. The prompt guides AI agents through deep codebase exploration before writing documentation that serves three purposes: local development setup, system understanding, and production deployment.

**Key Points:**
- Three-phase workflow: deep codebase exploration, deployment target detection, then strategic questioning only for critical ambiguities
- Deep exploration first: understand project structure, config files, database schema, dependencies, and scripts before writing anything
- Twelve-section README structure: overview, tech stack, prerequisites, getting started, architecture, environment variables, scripts, testing, deployment, troubleshooting, contributing, licensing
- Writing principles: "be absurdly thorough", make every command copy-pasteable, explain the "why" not just the "what"
- Assume readers are setting up on a fresh machine with no prior codebase familiarity
- Use tables liberally for reference material (env vars, commands, options)
- Include linked table of contents for documents over ~200 lines
- Detect deployment platform from config files (Dockerfile, fly.toml, render.yaml, etc.) and tailor instructions accordingly
- Architecture section should include directory structure, request lifecycle diagrams, data flow, and key component explanations

---

### [Vibe Coding - A New Kind of Development](https://x.com/karpathy/status/1886192184808149383)
*2026-02-10* | Tags: vibe-coding, workflow, mindset, throwaway-projects, voice-input, karpathy

Andrej Karpathy (AI thought leader, former Tesla AI director and OpenAI researcher) introduces the concept of "vibe coding" - a development style where you fully surrender to the AI's capabilities and stop treating the code as something you need to understand or control.

**Key Points:**
- "Vibe coding" means fully giving in to the vibes, embracing exponentials, and forgetting that the code even exists
- Always "Accept All" - don't read the diffs anymore
- Use voice input (SuperWhisper) to barely touch the keyboard
- Make trivial requests without hesitation ("decrease the padding on the sidebar by half")
- When error messages appear, just copy-paste them with no comment - usually that fixes it
- Code grows beyond usual comprehension; would require significant effort to actually read through
- When LLMs can't fix a bug, work around it or ask for random changes until it goes away
- Acknowledges this is best suited for "throwaway weekend projects" rather than production code
- Represents a paradigm shift: "building a project, but it's not really coding - I just see stuff, say stuff, run stuff, and copy paste stuff"

---

### [Multi-Agent Orchestration as Core Complexity](https://x.com/i/status/2021386270904287295)
*2026-02-11* | Tags: multi-agent, orchestration, context-management, agent-coordination

Developer shares experience building multi-agent setups, noting that agent orchestration is where the real complexity lives. References Steve Yegge's early identification of this challenge.

**Key Points:**
- Getting individual agents to work is not the hard part
- The core challenge is getting multiple agents to coordinate effectively
- Context loss mid-task is a major problem in multi-agent systems
- Agent orchestration is an emerging area of complexity that industry leaders identified early

---
