[← Back to Topic](README.md) | [← Home](../../README.md)

# Claude Code Power User Tips

## Index

- [Parallel Worktrees](#parallel-worktrees)
- [Plan Mode Strategy](#plan-mode-strategy)
- [CLAUDE.md as Living Documentation](#claudemd-as-living-documentation)
- [Custom Skills](#custom-skills)
- [Autonomous Bug Fixing](#autonomous-bug-fixing)
- [Advanced Prompting Techniques](#advanced-prompting-techniques)
- [Environment Setup](#environment-setup)
- [Subagents](#subagents)
- [Data and Analytics](#data-and-analytics)
- [Learning Mode](#learning-mode)
- [Sources](#sources)

---

## Parallel Worktrees

The single biggest productivity unlock according to the Claude Code team: **spin up 3-5 git worktrees at once**, each running its own Claude session in parallel [1].

### Implementation Approaches

- **Git worktrees** (team preference): Native support built into Claude Desktop app
- **Multiple git checkouts**: Alternative approach that works similarly
- **Shell aliases** (za, zb, zc): One keystroke to hop between worktrees
- **Named worktrees**: Organize by purpose (e.g., a dedicated "analysis" worktree only for reading logs and running BigQuery)

The key insight is treating each worktree as an independent context where Claude can work without interference from other tasks.

## Plan Mode Strategy

Start every complex task in plan mode. The goal: **pour energy into the plan so Claude can 1-shot the implementation** [1].

### Advanced Patterns

- **Two-Claude review**: Have one Claude write the plan, then spin up a second Claude to review it "as a staff engineer"
- **Re-plan on friction**: The moment something goes sideways, switch back to plan mode and re-plan rather than pushing through
- **Verification planning**: Explicitly tell Claude to enter plan mode for verification steps, not just for the build phase

## CLAUDE.md as Living Documentation

CLAUDE.md files should evolve continuously based on Claude's behavior [1].

### The Feedback Loop

After every correction, end with: "Update your CLAUDE.md so you don't make that mistake again." Claude is effective at writing rules for itself.

### Maintenance Practices

- **Ruthless editing**: Keep iterating until Claude's mistake rate measurably drops
- **Notes directories**: One engineer has Claude maintain a notes directory for every task/project, updated after every PR, then points CLAUDE.md at it
- **Measure effectiveness**: Track whether mistake rates actually decrease over time

## Custom Skills

If you do something more than once a day, turn it into a skill or command [1].

### Skill Examples from the Team

- **/techdebt**: Run at the end of every session to find and kill duplicated code
- **Context aggregator**: Syncs 7 days of Slack, GDrive, Asana, and GitHub into one context dump
- **Analytics engineer agents**: Write dbt models, review code, and test changes in dev

### Best Practice

Commit skills to git and reuse across every project. Build a personal library of capabilities.

### Skill Management Tools

**CodexSkillManager** [2] is a macOS app that provides a unified interface for managing skills:

- Manages skills across three directories: `~/.codex/skills`, `~/.codex/skills/public`, and `~/.claude/skills`
- Browse and download remote skills from Clawdhub (a community skill catalog)
- Visual installation status indicators showing whether skills are installed for Codex, Claude, or both
- Import skills from folders or ZIP archives
- Render skill documentation with Markdown formatting

This centralizes skill discovery and management rather than manually copying files between directories.

## Autonomous Bug Fixing

Claude can fix most bugs with minimal direction [1].

### Patterns

- **Slack MCP integration**: Paste a Slack bug thread into Claude and say "fix" - zero context switching
- **CI delegation**: "Go fix the failing CI tests" - don't micromanage how
- **Docker log analysis**: Point Claude at docker logs to troubleshoot distributed systems - it's effective at this

The theme is reducing context switching and trusting Claude with more autonomy on well-defined problems.

## Advanced Prompting Techniques

Level up prompting with these patterns from the team [1]:

### Challenge Claude

- "Grill me on these changes and don't make a PR until I pass your test" - make Claude your reviewer
- "Prove to me this works" - have Claude diff behavior between main and feature branch

### Refinement Prompts

- After a mediocre fix: "Knowing everything you know now, scrap this and implement the elegant solution"

### Specification Quality

Write detailed specs and reduce ambiguity before handing work off. The more specific you are, the better the output.

## Environment Setup

Terminal and environment configuration matters for Claude Code productivity [1].

### Terminal Choice

The team loves **Ghostty** for:
- Synchronized rendering
- 24-bit color
- Proper unicode support

### Status and Context

- Use `/statusline` to customize status bar showing context usage and current git branch
- Color-code and name terminal tabs (one per task/worktree)
- Consider tmux for session management

### Voice Dictation

Use voice dictation - you speak 3x faster than you type, and prompts become more detailed as a result. On macOS, hit fn twice to activate.

## Subagents

Subagents allow throwing more compute at problems while keeping context clean [1].

### Usage Patterns

- Append "use subagents" to requests where you want more compute applied
- Offload individual tasks to subagents to keep main agent's context window focused
- Route permission requests to Opus 4.5 via a hook - let it scan for attacks and auto-approve safe ones

## Data and Analytics

Claude Code is effective for data and analytics work [1].

### BigQuery Integration

Ask Claude Code to use the "bq" CLI to pull and analyze metrics. The Claude Code team has a BigQuery skill checked into their codebase that everyone uses.

Result: No manual SQL writing for 6+ months.

### Generalization

This pattern works for any database with a CLI, MCP, or API.

## Learning Mode

Use Claude Code as a learning tool [1]:

### Output Styles

Enable the "Explanatory" or "Learning" output style in `/config` to have Claude explain the *why* behind its changes.

### Visual Learning

- Have Claude generate visual HTML presentations explaining unfamiliar code
- Ask Claude to draw ASCII diagrams of new protocols and codebases

### Spaced Repetition

Build a spaced-repetition learning skill: you explain your understanding, Claude asks follow-ups to fill gaps, stores the result.

## Sources

1. [Boris Cherny on X](https://x.com/bcherny/status/2017742759218794768) - Claude Code creator sharing tips from the Claude Code team (2026)
2. [CodexSkillManager](https://github.com/Dimillian/CodexSkillManager) - macOS app for managing agent skills across Codex and Claude Code
