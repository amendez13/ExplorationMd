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
