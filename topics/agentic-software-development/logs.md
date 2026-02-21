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

### [Multi-Model Pipeline: Reverse Engineering an App to Build an AI-Controlled CLI](https://x.com/robinebers/status/2010936430718304719)
*2026-02-11* | Tags: reverse-engineering, multi-model, cli-tools, skills, automation, whatsapp, real-world-integration

Developer demonstrates a multi-model pipeline to reverse engineer a coffee shop's Android app, build a CLI tool to interact with its APIs, and then expose that CLI as a skill to an AI agent (Clawdbot/OpenClaw) - enabling coffee ordering via WhatsApp.

**Key Points:**
- Pipeline used three different models for specialized tasks: Claude Code + GLM 4.7 for decompilation and API documentation, GPT 5.2-Codex-High for building the CLI tool, and OpenClaw for end-user interaction
- First stage: Decompile Android APK and write documentation for the reverse-engineered APIs
- Second stage: Use decompiled code + documentation to build a terminal tool that "pretends" to be the original app
- Third stage: Create a skill that teaches the AI agent how to use the CLI tool
- End result: AI can perform real-world actions (ordering coffee) through natural language via WhatsApp
- Demonstrates the pattern of building CLI wrappers around proprietary APIs to make them agent-accessible
- Shows how skills can bridge AI agents to arbitrary external systems with no official API support

---

### [Shipping at Inference-Speed](https://steipete.me/posts/2025/shipping-at-inference-speed)
*2026-02-11* | Tags: inference-speed, parallel-projects, architectural-oversight, codex, model-selection, workflow

Peter Steinberger describes how AI coding assistants have transformed development velocity to the point where output is limited only by inference time and architectural thinking. Shares detailed workflow for managing 3-8 simultaneous projects with agents writing essentially all code.

**Key Points:**
- Software creation is now mostly limited by inference time and hard thinking, not coding speed
- GPT 5.2 Codex excels at large refactors through extensive file reading (10-15 min) before writing; Opus is faster for small edits but misses context on larger features
- Manages 3-8 simultaneous projects with careful mental model juggling
- Uses queueing features rather than complex orchestration; commits directly to main rather than using branches
- Cross-references existing solutions across projects to save prompts; maintains docs folders with structured subsystem documentation
- Uses image references for UI iterations ("fix padding" with screenshots)
- Lets agents autonomously run in project folders for pattern implementation
- TypeScript for web, Go for CLI, Swift for macOS/iOS applications
- Context window set to 233,000 tokens for better file visibility
- Architecture decisions still require manual thinking: dependency selection, framework choices, data flow patterns
- Never uses issue trackers; prioritizes immediate implementation
- Starts with CLI prototypes before building extensions or UIs
- Runs two Macs simultaneously for parallel development; keeps tasks running remotely while traveling
- "Don't read much code anymore. Watch the stream and sometimes look at key parts" - focus shifted from code review to architectural oversight

---

### [Senior Engineers Coding as Strength, Not Weakness](https://x.com/i/status/2021767364304679098)
*2026-02-12* | Tags: engineering-culture, seniority, leadership, scaling, agentic-development

Tweet observation: senior engineers who continued coding were historically seen (in some org cultures) as unable to “let go” and scale, but that norm is changing rapidly.

**Key Points:**
- Status signals around “hands-on” work can shift as tooling and leverage change
- Senior engineers can stay closer to implementation without becoming the throughput limiter
- Culture and incentives matter: hands-on leadership can be a force multiplier when paired with strong review and delegation

---

### [How I use Claude Code for real engineering](https://youtu.be/kZ-zzHVUrO4?si=Ue6dee2iQmKy6tOu)
*2026-02-13* | Tags: claude-code, plan-mode, workflow, context-management, large-projects, prompting

Matt Pocock walks through his workflow for tackling larger coding projects with Claude Code using plan mode. The approach is plan-first exploration (including clarifying questions), multi-phase execution that can span context windows, and durable plan storage (e.g., GitHub issues) to survive resets.

**Key Points:**
- Start with a rough (often dictated) prompt, then use plan mode to refine the work and surface clarifying questions
- Break complex features into multi-phase plans sized to fit within context-window limits
- Use lightweight rules to keep plans concise and track unresolved questions explicitly
- Monitor context usage during implementation and plan around inevitable context resets
- Persist the plan outside the chat (e.g., as a GitHub issue) to rehydrate later without re-planning

---

### [Claude Code Best Practices - Official Documentation](https://code.claude.com/docs/en/best-practices)
*2026-02-11* | Tags: claude-code, best-practices, context-management, verification, planning, prompting, environment, scaling, official-docs

Official Anthropic documentation providing comprehensive guidance on getting the most out of Claude Code. Covers the agentic coding paradigm where Claude explores, plans, and implements autonomously, with emphasis on managing the fundamental constraint: context window performance degradation.

**Key Points:**
- Context window is the core constraint - performance degrades as it fills; track usage continuously with `/statusline`
- Verification is highest-leverage: include tests, screenshots, or expected outputs so Claude can self-check
- Four-phase workflow: Explore (Plan Mode) → Plan → Implement (Normal Mode) → Commit
- Specific prompts reduce corrections: scope tasks, point to sources, reference existing patterns, describe symptoms
- CLAUDE.md should be concise - only include what would cause mistakes if removed; bloated files are ignored
- Permission allowlists and sandboxing reduce interruptions while maintaining safety
- CLI tools are most context-efficient for external services; MCP servers extend to Notion, Figma, databases
- Hooks guarantee deterministic actions; skills extend domain knowledge on-demand
- Custom subagents run in separate contexts for specialized tasks (security review, research)
- Course-correct early: `Esc` to stop, `Esc+Esc` or `/rewind` to restore state, `/clear` between tasks
- Subagents keep main context clean - delegate research that would consume tokens
- Checkpoints persist across sessions; rewind any time to restore conversation and/or code state
- Headless mode (`claude -p`) enables CI integration, pre-commit hooks, and batch processing
- Parallel sessions via Claude Desktop, web, or agent teams; Writer/Reviewer pattern for quality
- Fan-out patterns for migrations: loop through file lists with scoped `--allowedTools`
- Common failures: kitchen-sink sessions, repeated corrections, over-specified CLAUDE.md, unverified output, infinite exploration

---

### [Claude Code Creator Reflects on One Year of Progress](https://x.com/i/status/2004887829252317325)
*2026-02-11* | Tags: claude-code, productivity-metrics, capability-trends, origin-story, opus-4.5

Boris Cherny (creator of Claude Code) reflects on the tool's evolution from a September 2024 side project to a core development tool. Shares concrete productivity metrics demonstrating the shift in software engineering.

**Key Points:**
- Claude Code started as a side project in September 2024; grew beyond expectations
- A year ago: Claude struggled to generate bash commands without escaping issues; worked for seconds or minutes at a time
- Today: Claude consistently runs for minutes, hours, and days at a time (using Stop hooks)
- 30-day productivity metrics: 259 PRs, 497 commits, 40k lines added, 38k lines removed
- Every single line written by Claude Code + Opus 4.5 - no human-written code
- "Code is no longer the bottleneck" - software engineering is fundamentally changing
- Technology described as "alien and magical" - enabling people to build and create beyond traditional coding limitations

---

### [The New Programmable Layer of Abstraction](https://x.com/i/status/2004607146781278521)
*2026-02-11* | Tags: abstraction-layer, mental-model, skill-gap, agents, karpathy

Andrej Karpathy (AI thought leader, former Tesla AI director, OpenAI researcher) describes the overwhelming new conceptual landscape facing programmers. Characterizes the moment as a "magnitude 9 earthquake" rocking the profession.

**Key Points:**
- Programmers must now master a new layer of abstraction on top of traditional engineering: agents, subagents, prompts, contexts, memory, modes, permissions, tools, plugins, skills, hooks, MCP, LSP, slash commands, workflows, IDE integrations
- The feeling of being "behind" is pervasive - sensing potential 10X productivity boost but struggling to claim it feels like "skill issue"
- Programming profession is being "dramatically refactored" with programmer contributions becoming "increasingly sparse and between"
- Must build mental models for "fundamentally stochastic, fallible, unintelligible and changing entities" intermingled with traditional deterministic engineering
- Compares to being handed a "powerful alien tool" with no manual - everyone must figure out how to operate it themselves
- Call to action: "Roll up your sleeves to not fall behind"

---

### [Legacy Assumptions and the Mental Adjustment Problem](https://x.com/i/status/2004626064187031831)
*2026-02-11* | Tags: mindset, legacy-assumptions, opus-4.5, productivity, debugging, anthropic

An Anthropic engineer on the Claude Code team reflects on the continuous mental recalibration required to use AI models effectively. Observes that newer engineers without legacy model assumptions often use AI more effectively than experienced practitioners.

**Key Points:**
- Experienced engineers regularly approach problems manually before remembering "Claude can probably do this"
- Concrete example: While debugging a memory leak, one engineer used traditional profiler-based approach; a coworker asked Claude to make a heap dump and analyze it—Claude 1-shotted the fix and submitted a PR
- Newer engineers and new grads without assumptions about model limitations use tools more effectively
- Legacy memories formed with older models create mental friction that must be continuously overcome
- Mental re-adjustment to model capabilities needed "every month or two" as models improve
- First month as an engineer without opening an IDE at all - Opus 4.5 wrote ~200 PRs, every line
- "This is *still* just the beginning" - expectation recalibration is ongoing even for early adopters

---

### [The Pellet Gun / Laser Beam Experience](https://x.com/i/status/2004628491862696070)
*2026-02-11* | Tags: user-experience, stochastic-behavior, mental-model, expectations

Developer describes the inconsistent experience of AI coding assistants using a vivid metaphor: a weapon that fires pellets, sometimes misfires, but occasionally produces a powerful laser beam when held just right.

**Key Points:**
- AI coding tools produce highly variable results - inconsistency is the norm, not the exception
- Most attempts yield modest results ("pellets") or outright failures ("misfires")
- Occasionally, when conditions align perfectly, the tool produces exceptional results ("laser beam" that "melts your problem")
- The metaphor captures the stochastic nature that makes these tools both frustrating and transformative
- Success depends partly on skill ("hold it just right") but also on factors outside user control
- This variability is a core experiential reality that practitioners must internalize

---

### [Quality Over Speed: The Anti-Slop Playbook](https://x.com/i/status/2017871044737192409)
*2026-02-11* | Tags: product-quality, ai-slop, user-engagement, validation, differentiation

Developer argues that in 2026, building actual quality products is the competitive advantage while others rush to ship "AI slop." Presents a five-point playbook for standing out through customer-centric practices.

**Key Points:**
- Build actual products while competitors ship low-quality "AI slop"
- Validate ideas before building - don't skip discovery because AI makes building fast
- Create public roadmaps where users can upvote features - transparency builds trust
- Let people report bugs and see them getting fixed - visible accountability
- Test software before release - basic discipline often skipped when velocity feels unlimited
- Make customers feel like they're part of something - community engagement as differentiator
- These practices "used to be the gold standard" but are now rare because AI makes building "fun and fast"
- Speed without quality is noise; sustainable success requires both
- Competitive advantage emerges from doing what others skip in their rush to ship

---

### [Beautiful Mermaid - Visual Diagrams for Agent Reasoning](https://x.com/i/status/2016564311393464497)
*2026-02-11* | Tags: diagrams, mermaid, visual-reasoning, tooling, craft-docs

Developer shares that diagrams are becoming the primary way of reasoning about code with AI agents. Introduces "Beautiful Mermaid" - a fast, themeable renderer for Mermaid diagrams built for daily use.

**Key Points:**
- Diagrams are becoming a primary tool for reasoning about code when working with agents
- Mermaid format is praised for its agent-friendly text-based diagram syntax
- Beautiful Mermaid is a new renderer: 100 diagrams under 100ms, svg-level theming, extensive customization
- ASCII renderer ported from Alexander Grooff's Go implementation to TypeScript
- Integration coming to chat.com and Craft Docs
- Open source: https://github.com/AlaricLighworker/beautiful-mermaid

---

### [Karpathy on Claude Coding: Phase Shift Observations](https://x.com/i/status/2015883857489522876)
*2026-02-11* | Tags: workflow-shift, model-limitations, tenacity, speedups, skill-atrophy, slopacolypse, karpathy

Andrej Karpathy shares extensive observations after weeks of intensive Claude Code usage. Documents the rapid transition from 80% manual to 80% agent coding within a month, detailed failure modes of current agents, and broader implications for the profession.

**Key Points:**
- Workflow shifted from 80% manual/20% agent to 80% agent/20% manual between November-December 2025; biggest coding workflow change in ~2 decades
- IDEs still necessary; "agent swarm" and "no IDE" hype premature; models make subtle conceptual errors like a "slightly sloppy, hasty junior dev"
- Common failure modes: wrong assumptions without checking, no clarification-seeking, no inconsistency surfacing, no tradeoff presentation, sycophancy, overcomplication, code bloat, dead code accumulation, orthogonal code changes
- Agent tenacity is remarkable - never tire, never demoralize, will struggle for 30 minutes to victory where humans would give up; "stamina is a core bottleneck to work"
- Speedup is hard to measure; main effect is expansion of what's worth building and approachable
- Declarative over imperative: "Don't tell it what to do, give it success criteria and watch it go"
- Programming feels more fun with drudgery removed; remaining work is creative; some engineers will split based on whether they liked coding vs building
- Skill atrophy already noticeable; generation and discrimination are different brain capabilities
- 2026 predicted as "slopacolypse" year across GitHub, Substack, arXiv, social media
- Open questions: 10X engineer ratio may grow substantially; generalists may outperform specialists; much of society is bottlenecked by digital knowledge work
- December 2025 marks a "threshold of coherence" causing phase shift in software engineering

---

### [How to Make Your Agent Learn and Ship While You Sleep](https://substack.com)
*2026-02-11T00:00:00Z* | Tags: automation, overnight-agents, compound-learning, scheduled-tasks, launchd, persistent-memory, autonomous-shipping

Developer describes a nightly automation loop where AI agents review daily work, extract learnings into persistent memory (AGENTS.md/CLAUDE.md), and then pick up the next priority feature from a backlog to implement—all running unattended overnight.

**Key Points:**
- Two-part nightly loop: compound review at 10:30 PM (extracts learnings from missed sessions), auto-compound at 11:00 PM (implements next priority item)
- Order matters: review updates AGENTS.md with patterns/gotchas; implementation job pulls fresh context before coding
- Uses three open-source projects: Compound Engineering Plugin (learning extraction), Compound Product (PRD-to-tasks pipeline), Ralph (autonomous agent loop)
- For Claude Code: replace `amp execute` with `claude -p "..." --dangerously-skip-permissions`
- Daily compound review script: agent reviews threads from last 24 hours, retroactively extracts learnings from sessions that didn't compound, updates AGENTS.md, commits and pushes to main
- Auto-compound script: fetches latest main (with fresh learnings), finds latest prioritized report, picks #1 priority, creates feature branch, generates PRD, converts to tasks, runs execution loop, creates draft PR
- Uses launchd on macOS (preferred over cron for edge case handling) with plist files for scheduling
- Requires caffeinate to prevent Mac sleep during automation window (5 PM to 2 AM)
- AGENTS.md/CLAUDE.md becomes "living knowledge base" that grows every night; agent reads its own updated instructions before each run
- "Compound effect": patterns discovered Monday inform Tuesday's work; gotchas hit Wednesday avoided Thursday
- Extension ideas: Slack notifications, multiple priority tracks, automatic PR merge if CI passes, weekly changelog generation

---

### [Entire - AI Code Transparency Platform](https://entire.io/)
*2026-02-12T00:00:00Z* | Tags: agent-tooling, infrastructure, code-review, transparency, git, thomas-dohmke, github

Former GitHub CEO Thomas Dohmke launches Entire, a developer platform for managing AI-generated code at scale. The company raised a record $60M seed round at $300M valuation from Felicis, Madrona, and others. Entire addresses the fundamental challenge that traditional software production pipelines were never designed for the era of AI agents.

**Key Points:**
- Three-component architecture: git-compatible database for AI code, universal semantic reasoning layer for multi-agent coordination, AI-native UI for agent-human collaboration
- Checkpoints CLI is the first product: open source tool that captures prompts, agent decision steps, implementation logic, and full transcripts alongside code
- Solves the "review bottleneck" - security and compliance require human sign-off but review cannot keep pace with agent velocity
- Git-compatible database stores reasoning context that agents emit, not just the code output - enables querying intent, not just implementation
- Currently supports Anthropic Claude Code and Google Gemini CLI
- Positions as a layer above GitHub/GitLab rather than a competitor - manages agent reasoning while existing platforms handle code storage
- Founded by Thomas Dohmke (GitHub CEO 2021-2025, previously sold HockeyApp to Microsoft)
- Investors include Madrona, M12, Basis Set, Harry Stebbings, Jerry Yang, Datadog founder Olivier Pomel

---

### [OSS Monetization and the “Attention Layer” Problem](https://x.com/i/status/2009688028931875156)
*2026-02-12T12:15:11Z* | Tags: oss, open-source, monetization, attention-economy, docs, open-core, consulting, closed-source, agent-marketplace

Developer argues that common OSS monetization models depend on *human attention* (docs, brand recognition, perceived expertise), and that coding agents break this funnel by consuming documentation and libraries without routing humans through the maintainer’s surfaces. Uses Tailwind as a cautionary example and predicts a shift toward paid access gating and agent-oriented marketplaces.

**Key Points:**
- OSS monetization is framed as “free access now, monetize human attention downstream”
- Coding agents reduce docs traffic and maintainer recognition by bypassing human browsing and discovery
- Open-core and “expertise moat” models are predicted to weaken when agents mediate usage and learning
- Tailwind is cited as evidence that downloads can rise while revenue and docs traffic fall
- “Optimizing docs for agents” is portrayed as accelerating the collapse of the human attention funnel
- Proposed response: move monetization to the gate via closed source, source-available licenses, or metered access
- Prediction: an agent-native marketplace where libraries become paid, authenticated capabilities (like APIs)

---

### [Moats Shift to Complexity and Higher Abstraction](https://x.com/i/status/2010044820740563412)
*2026-02-12* | Tags: abstraction, moats, complexity, competitive-advantage, product-strategy, llm-coding

A short X exchange argues that as LLMs increasingly generate implementation, the enduring value in "code" is in the abstractions you sell or offer. In this framing, defensibility tends to come either from real-world complexity or from building higher-level abstractions that LLMs cannot reliably produce end-to-end from a prompt.

**Key Points:**
- Code can be viewed primarily as a stack of abstractions, not as the core product itself
- As LLMs improve, the market shifts toward offering higher-level abstractions rather than raw implementation
- Moats come from either complexity (messy constraints and edge cases) or from higher abstractions that are hard for LLMs to generate reliably

---

### [Skills in OpenAI API - Cookbook](https://developers.openai.com/cookbook/examples/skills_in_api)
*2026-02-12* | Tags: openai, skills, api, skill-manifest, shell-tool, hosted-containers, local-execution

Official OpenAI cookbook documentation on implementing Skills via the API. Provides detailed code examples for creating, uploading, and using skills in both hosted container and local shell environments.

**Key Points:**
- Skills are reusable bundles with SKILL.md manifest, scripts, requirements.txt, and assets
- Skills fill the "middle layer" between prompts (always-on behavior) and tools (atomic capabilities)
- Two upload methods: multipart file upload or zip upload (recommended for reproducibility)
- Hosted shell (container_auto) and local shell both supported with identical skill_reference syntax
- Version pinning recommended for production; avoid floating to "latest"
- CLI design principles: run from command line, deterministic output, clear error messages, write to known paths
- Limits: 50 MB max zip, 500 max files, 25 MB max uncompressed file size, exactly one manifest required

---

### [Agentic ML Experimentation (Human-in-the-Loop)](https://x.com/i/status/2005421816110862601)
*2026-02-12T13:19:47Z* | Tags: ml-experiments, training-runs, testing, debugging, wandb, profiling, human-in-the-loop, code-quality, pr-triage

Developer reports using Claude as a "lab assistant" running end-to-end ML experimentation loops: implementing changes, debugging with toy examples, writing tests, launching and monitoring training runs, and maintaining a living record of runs/results. The human remains actively in the loop, correcting subtle mistakes and pushing back on bloated or over-coupled designs.

**Key Points:**
- Runs a full experiment workflow: implement → debug (toy examples) → test fail/pass → launch training → tail logs → pull wandb stats
- Keeps ongoing research artifacts: markdown highlights plus a structured record of runs/results (tables)
- Uses profiling to find optimizer inefficiencies, fixes them, and measures improvement
- Reviews open PRs, categorizes/prioritizes them, and makes commits against selected items
- Still requires strong human oversight for subtle errors, confusion episodes, missed ideas, and design bloat/coupling

---

### [Shell + Skills + Compaction: Tips for Long-Running Agents](https://developers.openai.com/blog/skills-shell-tips)
*2026-02-12T00:00:00Z* | Tags: openai, skills, shell-tool, compaction, context-management, long-running-agents, security, enterprise

OpenAI's Charlie Guo introduces three agentic primitives for building long-running agents: Skills (reusable instruction bundles aligned with Agent Skills open standard), Shell Tool (OpenAI-hosted containers for dependency installation and script execution), and Server-Side Compaction (automatic context management). Includes 10 practical tips and 3 implementation patterns.

**Key Points:**
- Skills bundle files with SKILL.md manifests as versioned playbooks; descriptions should explain when to use/avoid, not marketing copy
- Negative examples in skill descriptions reduce misfires (~20% accuracy drop recovered after adding edge cases in Glean testing)
- Shell tool supports both hosted containers and local runtimes; `/mnt/data` is the artifact boundary
- Compaction prevents context window failures; design for it as default, not fallback
- Two-layer allowlist system: org-level sets max destinations, request-level must be subset
- `domain_secrets` for auth: models see placeholders, sidecar injects real values for approved destinations only
- Enterprise pattern: Glean achieved 73% → 85% accuracy and 18.1% TTFT reduction with Salesforce-oriented skills
- Network + skills = exfiltration risk; maintain strict allowlists and treat tool output as untrusted

---

### [Designing Software for Agents as First-Class Users](https://x.com/rauchg/status/example)
*2026-02-12* | Tags: agent-native-design, paradigm-shift, product-thinking, cli-tools, skills, delegation

Discussion sparked by Guillermo Rauch's observation: most people who are AI-native in building software do not build AI-native software. Compares the current moment to early internet when merchant websites copied physical stores instead of creating native e-commerce experiences.

**Key Points:**
- Distinction between "AI-native builders" (using AI to code) and "AI-native products" (designed for agent consumption)
- Test for agentic thinking: "Would I delegate this to a person? If so, think agentically"
- Instead of building a local research app, create skills and CLIs that agents can use to do research
- Two design questions: "How would an agent use this?" vs "How can an agent do this?"
- Example tradeoff: build a browser automation script vs give Claude browser access directly
- Security/privacy considerations factor into the decision (some tasks need deterministic scripts, others benefit from agent flexibility)
- The real shift: designing for agents as first-class users of software, not just builders of it
- We're still in the "digital catalog phase" - equivalent to early web replicating physical catalogs
- Some builders are executing old ideas that just became feasible, rather than rethinking what to build
- Not everything should be agentic (e.g., auth should remain deterministic)

---

### [The SHIP Framework for Non-Coders](https://www.youtube.com/watch?v=s_54GhcpDvE)
*2026-02-13* | Tags: ship-framework, no-code, system-architecture, blueprints, tool-selection, test-builds, solo-workflow

20-year coder shares the SHIP framework for building apps with AI without writing code. The insight: experienced coders initially struggled with AI because their approach was wrong—the skill needed isn't coding, it's system architecture. Like giving a carpenter blueprints instead of just saying "build me a house."

**Key Points:**
- SHIP = Systems Planning, Handpick Tools, Initial Test Build, Production Build
- Systems Planning: spend 10 minutes identifying components (data storage, auth, external services) before touching AI
- Handpick Tools: don't let AI decide your stack—it often recommends fragmented services when one tool (e.g., Supabase) handles all
- Initial Test Build: build the ugliest possible version to prove the concept works; skip login, UI polish, edge cases
- Production Build: throw away the test and rebuild from scratch—trying to salvage messy test code creates spiraling bugs
- Magic phrase for non-technical users: "Answer without technical jargon. I'm not an engineer. Help me understand so I can make decisions."
- AI is "your construction crew"—the value is in architecture and systems thinking, not coding
- "Learning to code is definitely dying, but architecture and systems thinking—that's your next $100,000 skill"
- AI tools like Warp, Claude Code, Cursor mentioned as coding agents; Grok recommended for research/planning phase

---

### [The Eternal September of Open Source - GitHub's Response to AI-Generated Contributions](https://github.blog/open-source/maintainers/welcome-to-the-eternal-september-of-open-source-heres-what-we-plan-to-do-for-maintainers/)
*2026-02-13* | Tags: open-source, maintainer-burden, contribution-quality, github, ai-contributions, eternal-september, trust-systems

GitHub frames the current moment as open source's "Eternal September"—a reference to Usenet's 1993 influx of unprepared users. The cost to create contributions has dropped dramatically with AI tools, but review time hasn't decreased, overwhelming maintainers with volume.

**Key Points:**
- The asymmetry problem: AI slashes contribution creation cost, but review cost remains constant—maintainers can't keep up
- curl ended its bug bounty program after AI-generated security reports exploded, each taking hours to validate
- Ghostty moved to invitation-only contributions
- Multiple projects adopted explicit rules about AI-generated submissions
- Mitchell Hashimoto's Vouch implements explicit trust management requiring vouching by trusted maintainers
- GitHub already shipped: pinned comments, noise-reducing banners, PR performance improvements, temporary interaction limits
- Coming soon: repository-level PR controls, PR deletion from UI
- Under exploration: criteria-based gating (requiring linked issues), improved automated triage tools

---

### [Intelligent AI Delegation](https://arxiv.org/abs/2602.11865)
*2026-02-13* | Tags: delegation, multi-agent, task-decomposition, orchestration, limitations, research-paper

arXiv paper by Nenad Tomašev, Matija Franklin, and Simon Osindero addressing how AI agents decompose problems and delegate work. Argues that existing methods rely on simple heuristics and lack adaptive capabilities for complex delegation networks.

**Key Points:**
- Current task decomposition and delegation methods use simple heuristics, not adaptive frameworks
- Effective delegation requires authority transfer, responsibility tracking, accountability, and trust protocols
- Framework designed for both human and AI delegators/delegatees in mixed collaboration scenarios
- Addresses the gap for "the emerging agentic web" where agents increasingly interact with other agents
- Proposes dynamic adaptation to environmental changes and robust failure handling

---

### [The senior engineer's guide to AI coding: Context loading, custom hooks, and automation](note://76)
*2026-02-14T07:57:25Z* | Tags: context-loading, diagrams, mermaid, hooks, stop-hooks, automation, quality-gates, cli, aliases, senior-engineering, drift

Senior-engineer-focused workflow for making AI coding feel like a tuned development system: preload the right context (especially diagram files) and automate the feedback loops you already trust (typecheck/lint/format) so the agent fixes issues immediately rather than shipping broken changes downstream.

**Key Points:**
- Maintain a repo "memory" area of context files; Mermaid diagrams act as a compact, model-friendly compression of system flows
- Load all diagrams at startup via a single glob/concat shell command to skip repetitive context discovery
- Spend extra tokens up front to buy speed and reliability once execution starts; generate/backfill diagrams after PR boundaries
- Use aliases and tiny personal CLIs to encode repeatable prompt workflows and remove launch friction
- Use stop hooks as quality gates: if files changed, run checks (example: `bun typecheck`), feed errors back to the agent, then auto-commit when clean
- Keep hook stdout clean if it carries a JSON protocol payload; send debug output to stderr instead

---

### [Agents Have Zero Tolerance for Codebase Entropy](https://x.com/i/status/2022339274767520246)
*2026-02-15* | Tags: agent-native-design, codebase-quality, technical-debt, entropy, documentation, dead-code

Observation that everything done to make codebases "agent-ready" (better docs, less dead code, smaller surfaces) is what engineers always needed. Agents just can't work around the entropy humans learned to tolerate.

**Key Points:**
- Agent-ready practices are the same as good engineering practices—agents just have zero tolerance for their absence
- Agents can't "just know" a file is outdated or a code path is dead
- Agents take codebases at face value, forcing them to be worth taking at face value
- Human developers learned to work around accumulated entropy; agents surface it immediately
- Making codebases agent-ready is really about paying down hidden technical debt

---

### [Engineers Stay Essential in the Agent Era](https://x.com/i/status/2022762422302576970)
*2026-02-15* | Tags: prompting, customer-development, cross-team-coordination, product-judgment, engineering-roles

Reminder that even when “the AI writes the code,” teams still need humans to translate intent into prompts, gather customer signal, coordinate across teams, and decide what to build next. The bottleneck shifts from keystrokes to judgment and communication.

**Key Points:**
- Prompting/orchestration becomes a core engineering skill as agents handle more implementation
- Customer conversations and product prioritization remain human responsibilities
- Cross-team coordination becomes more important as build speed increases
- "Great engineers" matter more as leverage concentrates in judgment and decision-making

---

### [Telegram Plugin for Claude Code - Mobile Agent Control](https://x.com/i/status/2023960820091154569)
*2026-02-19* | Tags: mobile-control, telegram, remote-access, claude-code, oss, autonomous-agents, security

Levelsio (Pieter Levels) shares his use of a Telegram plugin for Claude Code that enables mobile-first agent control. The discussion covers the architecture, operational patterns, and security considerations for running agents remotely via messaging interfaces.

**Key Points:**
- Uses a Telegram plugin for Claude Code (OSS, no logins/payments) as alternative to SSH on mobile
- "More annoying to type inside SSH on iPhone than Telegram" - mobile typing optimized for messaging apps
- Simpler than OpenClaw: fewer status messages, no authentication overhead, minimal wrapper
- Can run as cron with heartbeat pattern; uses a script that checks every minute
- Suggests "SOUL .md" mission files to give agents persistent context and operating parameters
- Can grant repo access and let agent create PRs autonomously
- Reliability: "literally only made a mistake once in 12 months"
- Cost: ~$100/month for Claude API usage (separate from free interface)
- Security concern: "not convinced it's secure yet" - prompt injection risk increases with broader system access
- Use case: direct agents from anywhere (e.g., restaurant at 7pm)

---

### [Telegram Plugin for Claude Code - Operational Details](https://x.com/i/status/2024503974997483997)
*2026-02-19* | Tags: mobile-control, telegram, claude-code, server-setup, error-reporting, backups

Follow-up replies from Levelsio providing additional operational details for the Telegram-based agent control setup. Covers server setup requirements, error monitoring architecture, and backup practices.

**Key Points:**
- Server setup requires SSH access to set up Claude Code and the Telegram plugin on the server
- Error reporting uses a separate pre-AI cron job that checks error logs and sends notifications to Telegram (not part of the agent itself)
- Maintains daily and weekly backups for the agent environment
- Positions the Telegram plugin as simpler than OpenClaw for basic mobile control needs

---

### [Telegram Plugin for Claude Code - Per-Site VPS Architecture](https://x.com/i/status/2024507875356279026)
*2026-02-19* | Tags: mobile-control, telegram, claude-code, systemd, hetzner, vps, daemon, session-resume

Levelsio describes his per-site VPS architecture where each site runs on its own Hetzner VPS with Claude Code and the Telegram bot installed as a system daemon. Enables direct, persistent conversations with each site's agent.

**Key Points:**
- Each site runs on its own dedicated Hetzner VPS
- Claude Code and claw-telegram-bot installed on each VPS
- Telegram bot runs as a system daemon (systemd): starts on boot, auto-restarts if quit
- Message flow: Telegram message → bot resumes previous Claude Code session → writes input → captures response → sends back via Telegram
- Enables "talking directly to your sites" through Telegram
- Very simple architecture despite powerful capabilities

---

### [Rob — The SHIP Framework for Building Apps with AI (YouTube)](https://www.youtube.com/watch?v=s_54GhcpDvE)
*2026-02-20* | Tags: ship-framework, system-architecture, planning, tool-selection, prototyping, rebuild, ai-coding, solo-workflow

Rob describes a simple 4-step "SHIP" framework for getting unstuck from the build → break → re-prompt loop common with AI coding tools. The core message is that the leverage comes from architecture and decision-making ("blueprints"), not from writing code. This entry also links the specific video corresponding to the earlier placeholder YouTube link in the 2026-02-13 log entry.

**Key Points:**
- SHIP = Systems Planning, Handpick Your Tools, Initial Test Build, Production Build
- Systems Planning: write down the app's main components and constraints before prompting an AI coder (data, auth, integrations)
- Handpick Tools: don't accept the AI's default stack recommendations; explicitly compare cost, free tiers, and consolidation vs. lock-in tradeoffs
- Initial Test Build: build the smallest/ugliest version to validate the system works; skip polish, edge cases, and non-core features
- Production Build: throw away the test build and rebuild cleanly from a refined plan, rather than trying to "fix" AI-generated prototype spaghetti
- Prompting tip for non-engineers: ask for explanations "without technical jargon" so you can make tradeoffs deliberately
- (Sponsor segment, claimed): Warp is presented as a top-performing terminal-based AI coding agent on benchmarks

---

### [Andrej Karpathy on the Decade of Agents - Dwarkesh Podcast](https://www.youtube.com/watch?v=lXUZvyajciY)
*2026-02-20* | Tags: agent-timelines, rl-limitations, cognitive-core, model-collapse, self-driving-analogy, karpathy, interview

Andrej Karpathy (former Tesla AI director, OpenAI researcher) provides extensive views on why AI agents will require a decade to mature, fundamental limitations of RL, the cognitive deficits of current models, and lessons from self-driving deployment.

**Key Points:**
- "Decade of agents" not "year of agents"—agents lack continual learning, multimodality, confusion management; problems are tractable but will take ~10 years
- Current coding agents struggle with novel code—kept trying to use PyTorch DDP despite custom implementation, couldn't adapt to repository-specific styles
- RL is "sucking supervision through a straw"—upweights entire trajectory from single end reward, no selective credit assignment like humans do
- Process supervision via LLM judges is gameable—adversarial examples like "dhdhdhdh" got 100% reward; infinity of such exploits
- Pre-training stores ~0.07 bits/token; in-context learning stores ~320KB/token (35 million times more)—context is working memory, weights are hazy recollection
- Model collapse: outputs look reasonable individually but are "silently collapsed" to tiny manifold; ChatGPT has ~3 jokes; blocks synthetic data scaling
- Cognitive core hypothesis: a billion-parameter model stripped of memorized knowledge could be highly capable if trained on refined data
- Self-driving took 40+ years (demos from 1980s) and isn't done—"march of nines" where each 9 (90%→99%→99.9%) requires constant work
- Software has same safety property as self-driving—mistakes can leak millions of SSNs; vibe coding fine for throwaway, production needs care
- GDP won't show AI discontinuity—iPhone, computers don't show up either; expects gradual diffusion into same exponential
- Missing brain parts: transformer is cortical tissue, but hippocampus, amygdala equivalents absent; continual learning (sleep → distillation) missing

---

### [State of AI 2026 - Lex Fridman with Sebastian Raschka and Nathan Lambert](https://www.youtube.com/watch?v=EV7WhVT270Q)
*2026-02-20* | Tags: ai-landscape, scaling-laws, post-training, rlvr, open-models, claude-code, tool-use, agentic-development, interview

Wide-ranging podcast discussion covering the AI competitive landscape (US vs China), scaling laws across pre-training/RL/inference, RLVR and the "aha moment," agentic coding tools, and predictions for the next decade. Sebastian Raschka (author of *Build an LLM from Scratch*) and Nathan Lambert (AI2 post-training lead, RLHF book author) share both research and practitioner perspectives.

**Key Points:**
- No technology winner will emerge—ideas flow freely between labs; differentiation comes from budget, hardware, and organizational culture
- Claude Opus 4.5 hype is "almost a meme"—Anthropic bet hard on code and Claude Code is paying off; Google has structural TPU/data center advantages
- Chinese open-weight models: strategy to capture market share when enterprises won't pay for Chinese API subscriptions; licenses are "friendlier" than Llama/Gemma
- Architecture remarkably stable from GPT-2 to today—MoE, Group Query Attention, RMSNorm are incremental tweaks, not revolutions
- Three scaling axes all still work: pre-training, RL training, inference-time compute; no sign of plateaus
- RLVR: give question + answer, model develops step-by-step reasoning and self-correction organically; "aha moment" emerges without explicit teaching
- Sebastian verified: Qwen 3 base 15% → 50% accuracy on MATH-500 in just 50 RLVR steps; knowledge was already there, RLVR unlocks it
- RLHF does not scale (seminal paper is about "over-optimization"); RLVR does scale (OpenAI o1 showed the plot)
- Claude Code appears to utilize Opus 4.5 better than alternatives; same model in different interfaces produces notably different results
- Tool use is "huge unlock" for hallucination reduction—use calculator for math, search for facts—but trust/containerization infrastructure needed
- AI is "jagged"—superhuman at some things (frontend, data analysis), bad at others (distributed ML, novel research code)
- No discrete "superhuman coder" moment expected—continuous improvement with persistent gaps
- 2026 predictions: $2,000 subscription tier, more open-weight builders (many Chinese), software engineering shifts to system design and goals
- Learn by building from scratch (Sebastian), go narrow after fundamentals (Nathan), struggle matters for deep learning

---

### [My AI Adoption Journey](https://mitchellh.com/writing/my-ai-adoption-journey)
*2026-02-21* | Tags: adoption-journey, workflow-stages, agents-md, verification, parallel-work, skill-formation, hashimoto

Mitchell Hashimoto (software craftsman, HashiCorp founder) shares his structured journey from AI skepticism to continuous agent operation. Emphasizes deliberate phases of inefficiency before workflow transformation, with specific stages for building competence and trust.

**Key Points:**
- Three universal phases of tool adoption: inefficiency period, adequacy phase, workflow transformation—skipping stages leads to shallow adoption
- Stage 1: Abandon chatbots—pure chat requires too much human correction; move to agents that can read files, execute programs, and loop autonomously
- Stage 2: Reproduce work manually—complete tasks twice (manual then agent) to build intuition for what agents handle well
- Three principles from dual-execution: break sessions into clear tasks, separate planning from execution, provide agents verification mechanisms
- Stage 3: End-of-day agents—30-minute sessions before leaving for deep research, parallel exploration, issue triage; provides "warm start" next morning
- Stage 4: Parallel work—delegate high-confidence tasks while working on preferred projects; crucially turn off notifications to avoid context-switching costs
- Stage 5: Harness engineering—AGENTS.md files documenting agent behaviors, custom programmatic tools for verification, constraint specification
- Stage 6: Continuous operation—10-20% of workday covered by background agents; one agent at a time outperforms multiple simultaneous
- Use slower, higher-quality models for complex changes
- Skill formation tradeoff: delegating means not developing skills; junior developers lack fundamentals to judge when agents are wrong

---

### [How AI Assistance Impacts the Formation of Coding Skills](https://www.anthropic.com/research/AI-assistance-coding-skills)
*2026-02-21* | Tags: skill-formation, research, learning, debugging, junior-developers, anthropic

Anthropic research study investigating whether AI coding assistance helps developers learn new skills or creates a shortcut that undermines skill development. Randomized controlled trial with 52 junior software engineers learning Python's Trio library.

**Key Points:**
- AI assistance significantly impaired skill mastery: participants in the AI group scored 17% lower than those who coded by hand (equivalent of nearly two letter grades)
- Performance gap was particularly pronounced on debugging questions—this area may suffer most when developers rely on AI
- Productivity tradeoff: AI users completed tasks roughly two minutes faster, but improvement wasn't statistically significant
- Critical nuance: how developers use AI matters tremendously
- High-performing approaches: requesting explanations alongside code generation, using AI for conceptual inquiry while solving problems independently, follow-up questions for clarification
- Low-performing approaches: complete delegation of code writing, relying on AI for debugging rather than independent problem-solving
- Implications: organizations should design AI deployment strategies that preserve learning opportunities, particularly for junior developers who need to develop oversight capabilities

---

### [Ghostty Inspector AGENTS.md - Subsystem-Specific Agent Instructions](https://github.com/ghostty-org/ghostty/blob/ca07f8c3f775fe437d46722db80a755c2b6e6399/src/inspector/AGENTS.md)
*2026-02-21* | Tags: agents-md, subdirectory-placement, ghostty, imgui, zig, patterns

Real-world example of AGENTS.md placed in a subdirectory (`src/inspector/`) rather than at repository root. Ghostty's inspector subsystem (a browser-DevTools-like tool for terminal state inspection) demonstrates the pattern of scoped, subsystem-specific agent instructions.

**Key Points:**
- Subdirectory AGENTS.md files provide scoped context when agents work in specific parts of a codebase
- The file is concise (12 lines) with no redundancy—only subsystem-relevant information
- Points to external resources (dcimgui.h header, ImGui demo) rather than explaining dependencies inline
- Includes a find command (`find . -type f -name dcimgui.h`) for resource discovery
- Documents build flags specific to the subsystem (`-Demit-macos-app=false` for macOS API validation)
- Explicitly states what's missing ("This package does not include unit tests") to prevent wasted exploration
- Pattern is maintainable: subsystem teams can update their own AGENTS.md independently

---

### [Peter Steinberger on Agentic Engineering with Cloudbot - Pragmatic Engineer Podcast](https://www.youtube.com/watch?v=8lF7HmQ_RgY)
*2026-02-21* | Tags: cloudbot, parallel-agents, feedback-loops, prompt-requests, codex, workflow, cli-over-mcp, interview

Peter Steinberger (creator of PSPDFKit) discusses his radical AI-first development workflow after returning from a 3-year burnout break. He's building Cloudbot, a personal AI assistant with full computer access controllable via WhatsApp/Telegram. Covers running 5-10 agents in parallel, "weaving" code instead of merging it, and why he prefers GPT 5.2 Codex over Claude for complex tasks.

**Key Points:**
- Runs 5-10 agents in parallel; main project gets focus while satellite projects cook in background
- "Closing the loop" is the core principle—agents must be able to debug and test themselves; that's why AI is good at code but mediocre at creative writing
- Prefers GPT 5.2 Codex over Claude: takes 10x longer (10-40 min reading before writing) but much higher first-try success rate
- Claude reads 3 files then feels "confident enough" to write; Codex silently reads files for 10 minutes first
- PRs are now "prompt requests"—wants contributors to share prompts used, not just code; prompts are higher signal
- Basically rewrites every pull request; code quality of PRs has dropped due to vibe coding
- CLIs over MCPs: MCPs require pre-loading all tools, can't filter or chain; models are "really good at bash"
- Built `make-porter` to convert any MCP to CLI on demand
- Cloudbot has bootstrap phase creating user.md (info), soul.md (values), identity.md (name, emoji, inside jokes)
- Agent can edit its own configuration and update itself
- "I write better code now that I don't write code myself"
- Two developer types: builders (thrive with AI, care about outcomes) vs coders (struggle, love solving hard problems)
- Would hire based on GitHub activity and "loving the game"; could run a company with 30% of the people

---
