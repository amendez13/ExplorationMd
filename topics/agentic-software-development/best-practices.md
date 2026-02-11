[← Back to Topic](README.md) | [← Home](../../README.md)

# Claude Code Best Practices

Official guidance from Anthropic on getting the most out of Claude Code, from context management to parallel scaling.

## Index

- [Context Window: The Core Constraint](#context-window-the-core-constraint)
- [Verification: Give Claude Success Criteria](#verification-give-claude-success-criteria)
- [Workflow: Explore, Plan, Implement, Commit](#workflow-explore-plan-implement-commit)
- [Prompting: Be Specific and Rich](#prompting-be-specific-and-rich)
- [Environment Configuration](#environment-configuration)
  - [CLAUDE.md Essentials](#claudemd-essentials)
  - [Permissions and Sandboxing](#permissions-and-sandboxing)
  - [CLI Tools and MCP](#cli-tools-and-mcp)
  - [Hooks](#hooks)
  - [Skills](#skills)
  - [Custom Subagents](#custom-subagents)
- [Communication Patterns](#communication-patterns)
- [Session Management](#session-management)
  - [Course Correction](#course-correction)
  - [Context Management](#context-management)
  - [Subagents for Investigation](#subagents-for-investigation)
  - [Checkpoints and Rewinding](#checkpoints-and-rewinding)
- [Automation and Scaling](#automation-and-scaling)
  - [Headless Mode](#headless-mode)
  - [Parallel Sessions](#parallel-sessions)
  - [Fan-Out Patterns](#fan-out-patterns)
- [Common Failure Patterns](#common-failure-patterns)
- [Sources](#sources)

---

## Context Window: The Core Constraint

Most best practices derive from one fundamental constraint: **Claude's context window fills up fast, and performance degrades as it fills** [1].

The context window holds your entire conversation, including every message, every file Claude reads, and every command output. A single debugging session or codebase exploration might generate tens of thousands of tokens.

As context fills, Claude may start "forgetting" earlier instructions or making more mistakes. The context window is the most important resource to manage.

**Key action**: Track context usage continuously with a custom status line (`/statusline`).

---

## Verification: Give Claude Success Criteria

The single highest-leverage thing you can do is include tests, screenshots, or expected outputs so Claude can check itself [1].

Without clear success criteria, Claude might produce something that looks right but doesn't work. You become the only feedback loop, and every mistake requires your attention.

| Strategy | Before | After |
|----------|--------|-------|
| Provide verification criteria | "implement a function that validates email addresses" | "write a validateEmail function. example test cases: user@example.com is true, invalid is false, user@.com is false. run the tests after implementing" |
| Verify UI changes visually | "make the dashboard look better" | "[paste screenshot] implement this design. take a screenshot of the result and compare it to the original. list differences and fix them" |
| Address root causes | "the build is failing" | "the build fails with this error: [paste error]. fix it and verify the build succeeds. address the root cause, don't suppress the error" |

For UI changes, use the Claude in Chrome extension to have Claude test in a browser and iterate until it works.

Your verification can be a test suite, a linter, or a Bash command that checks output. Invest in making verification rock-solid.

---

## Workflow: Explore, Plan, Implement, Commit

Separate research and planning from implementation to avoid solving the wrong problem [1].

Use Plan Mode to separate exploration from execution:

### Four-Phase Workflow

1. **Explore** (Plan Mode): Read files and answer questions without making changes
   ```
   read /src/auth and understand how we handle sessions and login.
   also look at how we manage environment variables for secrets.
   ```

2. **Plan** (Plan Mode): Create a detailed implementation plan
   ```
   I want to add Google OAuth. What files need to change?
   What's the session flow? Create a plan.
   ```
   Press `Ctrl+G` to edit the plan directly before proceeding.

3. **Implement** (Normal Mode): Code and verify against the plan
   ```
   implement the OAuth flow from your plan. write tests for the
   callback handler, run the test suite and fix any failures.
   ```

4. **Commit**: Ask Claude to commit with a descriptive message and create a PR

### When to Skip Planning

Plan Mode adds overhead. For tasks where scope is clear and the fix is small (typos, log lines, renames), ask Claude directly.

Planning is most valuable when:
- You're uncertain about the approach
- The change modifies multiple files
- You're unfamiliar with the code being modified

If you could describe the diff in one sentence, skip the plan.

---

## Prompting: Be Specific and Rich

The more precise your instructions, the fewer corrections you'll need [1].

### Prompting Strategies

| Strategy | Before | After |
|----------|--------|-------|
| **Scope the task** | "add tests for foo.py" | "write a test for foo.py covering the edge case where the user is logged out. avoid mocks." |
| **Point to sources** | "why does ExecutionFactory have such a weird api?" | "look through ExecutionFactory's git history and summarize how its api came to be" |
| **Reference existing patterns** | "add a calendar widget" | "look at how existing widgets are implemented on the home page to understand the patterns. HotDogWidget.php is a good example. follow the pattern to implement a new calendar widget..." |
| **Describe symptoms** | "fix the login bug" | "users report that login fails after session timeout. check the auth flow in src/auth/, especially token refresh. write a failing test that reproduces the issue, then fix it" |

Vague prompts can be useful when exploring and you can afford to course-correct. A prompt like "what would you improve in this file?" can surface things you wouldn't have thought to ask.

### Rich Content Input

- **Reference files with `@`** instead of describing where code lives
- **Paste images directly** via copy/paste or drag and drop
- **Give URLs** for documentation; use `/permissions` to allowlist frequently-used domains
- **Pipe in data**: `cat error.log | claude`
- **Let Claude fetch what it needs** using Bash commands, MCP tools, or file reads

---

## Environment Configuration

### CLAUDE.md Essentials

Run `/init` to generate a starter CLAUDE.md file, then refine over time [1].

CLAUDE.md gives Claude persistent context it can't infer from code alone. Include Bash commands, code style, and workflow rules.

**Keep it concise**. For each line, ask: "Would removing this cause Claude to make mistakes?" If not, cut it. Bloated files cause Claude to ignore your actual instructions.

| Include | Exclude |
|---------|---------|
| Bash commands Claude can't guess | Anything Claude can figure out by reading code |
| Code style rules that differ from defaults | Standard language conventions Claude already knows |
| Testing instructions and preferred test runners | Detailed API documentation (link to docs instead) |
| Repository etiquette (branch naming, PR conventions) | Information that changes frequently |
| Architectural decisions specific to your project | Long explanations or tutorials |
| Developer environment quirks (required env vars) | File-by-file descriptions of the codebase |
| Common gotchas or non-obvious behaviors | Self-evident practices like "write clean code" |

**Tuning adherence**: Add emphasis ("IMPORTANT" or "YOU MUST") for critical rules. Check CLAUDE.md into git so your team can contribute.

**Import syntax**: Reference other files using `@path/to/import`:
```markdown
See @README.md for project overview and @package.json for available npm commands.

# Additional Instructions
- Git workflow: @docs/git-instructions.md
- Personal overrides: @~/.claude/my-project-instructions.md
```

**Placement options**:
- `~/.claude/CLAUDE.md`: Applies to all sessions
- `./CLAUDE.md`: Project root, check into git
- `./CLAUDE.local.md`: Project root, gitignored for personal overrides
- Parent/child directories: Automatic inheritance in monorepos

### Permissions and Sandboxing

Use `/permissions` to allowlist safe commands or `/sandbox` for OS-level isolation [1].

Two approaches to reduce interruptions:
- **Permission allowlists**: Permit specific tools you know are safe (`npm run lint`, `git commit`)
- **Sandboxing**: Enable OS-level isolation for filesystem and network access

Use `--dangerously-skip-permissions` only in sandboxed environments without internet access.

### CLI Tools and MCP

Tell Claude to use CLI tools like `gh`, `aws`, `gcloud`, and `sentry-cli` for external services [1].

CLI tools are the most context-efficient way to interact with external services. If you use GitHub, install `gh`. Without it, unauthenticated API requests hit rate limits quickly.

Claude is effective at learning new CLI tools: "Use 'foo-cli-tool --help' to learn about foo tool, then use it to solve A, B, C."

With MCP servers (`claude mcp add`), you can connect Notion, Figma, databases, monitoring data, and automate workflows.

### Hooks

Use hooks for actions that must happen every time with zero exceptions [1].

Unlike CLAUDE.md instructions (advisory), hooks are deterministic and guarantee the action happens.

Claude can write hooks: "Write a hook that runs eslint after every file edit" or "Write a hook that blocks writes to the migrations folder."

Run `/hooks` for interactive configuration.

### Skills

Create `SKILL.md` files in `.claude/skills/` for domain knowledge and reusable workflows [1].

Skills extend Claude's knowledge with project-specific information. Claude applies them automatically when relevant, or invoke directly with `/skill-name`.

**Domain knowledge skill**:
```markdown
---
name: api-conventions
description: REST API design conventions for our services
---
# API Conventions
- Use kebab-case for URL paths
- Use camelCase for JSON properties
- Always include pagination for list endpoints
- Version APIs in the URL path (/v1/, /v2/)
```

**Workflow skill** (invoked with `/fix-issue 1234`):
```markdown
---
name: fix-issue
description: Fix a GitHub issue
disable-model-invocation: true
---
Analyze and fix the GitHub issue: $ARGUMENTS.

1. Use `gh issue view` to get the issue details
2. Understand the problem described in the issue
3. Search the codebase for relevant files
4. Implement the necessary changes to fix the issue
5. Write and run tests to verify the fix
6. Ensure code passes linting and type checking
7. Create a descriptive commit message
8. Push and create a PR
```

Use `disable-model-invocation: true` for workflows with side effects that you want to trigger manually.

### Custom Subagents

Define specialized assistants in `.claude/agents/` that Claude can delegate to [1].

Subagents run in their own context with their own allowed tools. Useful for tasks that read many files or need specialized focus.

```markdown
---
name: security-reviewer
description: Reviews code for security vulnerabilities
tools: Read, Grep, Glob, Bash
model: opus
---
You are a senior security engineer. Review code for:
- Injection vulnerabilities (SQL, XSS, command injection)
- Authentication and authorization flaws
- Secrets or credentials in code
- Insecure data handling

Provide specific line references and suggested fixes.
```

Invoke explicitly: "Use a subagent to review this code for security issues."

---

## Communication Patterns

### Ask Codebase Questions

Ask Claude questions you'd ask a senior engineer [1]:

- How does logging work?
- How do I make a new API endpoint?
- What does `async move { ... }` do on line 134 of `foo.rs`?
- What edge cases does `CustomerOnboardingFlowImpl` handle?
- Why does this code call `foo()` instead of `bar()` on line 333?

No special prompting required. Effective for onboarding to unfamiliar codebases.

### Let Claude Interview You

For larger features, start with a minimal prompt and have Claude interview you using the `AskUserQuestion` tool [1]:

```
I want to build [brief description]. Interview me in detail using the AskUserQuestion tool.

Ask about technical implementation, UI/UX, edge cases, concerns, and tradeoffs. Don't ask obvious questions, dig into the hard parts I might not have considered.

Keep interviewing until we've covered everything, then write a complete spec to SPEC.md.
```

Once complete, start a fresh session to execute the spec. The new session has clean context focused entirely on implementation.

---

## Session Management

### Course Correction

Correct Claude as soon as you notice it going off track [1]. Best results come from tight feedback loops.

- **`Esc`**: Stop Claude mid-action; context preserved
- **`Esc + Esc` or `/rewind`**: Open rewind menu, restore conversation/code state, or summarize from selected message
- **`"Undo that"`**: Have Claude revert its changes
- **`/clear`**: Reset context between unrelated tasks

If you've corrected Claude more than twice on the same issue, context is cluttered with failed approaches. Run `/clear` and start fresh with a better prompt.

### Context Management

Manage context aggressively [1]:

- Use `/clear` frequently between tasks
- When auto-compaction triggers, Claude summarizes key code patterns, file states, and decisions
- For manual control: `/compact <instructions>`, e.g., `/compact Focus on the API changes`
- Partial compaction: `Esc + Esc` → select checkpoint → "Summarize from here"
- Add CLAUDE.md instructions like "When compacting, always preserve the full list of modified files and any test commands"

### Subagents for Investigation

Since context is your fundamental constraint, subagents are one of the most powerful tools [1].

When Claude researches a codebase, it reads many files that consume context. Subagents run in separate context windows and report back summaries:

```
Use subagents to investigate how our authentication system handles token
refresh, and whether we have any existing OAuth utilities I should reuse.
```

The subagent explores, reads files, and reports findings without cluttering your main conversation.

Also useful for verification: "use a subagent to review this code for edge cases"

### Checkpoints and Rewinding

Every action Claude makes creates a checkpoint. You can restore conversation, code, or both [1].

Double-tap `Escape` or run `/rewind` to open the rewind menu. Options include:
- Restore conversation only
- Restore code only
- Restore both
- Summarize from selected message

Checkpoints persist across sessions. You can close your terminal and rewind later.

**Note**: Checkpoints only track changes made by Claude, not external processes. This isn't a replacement for git.

---

## Automation and Scaling

Once effective with one Claude, multiply output with parallel sessions, headless mode, and fan-out patterns [1].

### Headless Mode

Use `claude -p "prompt"` in CI, pre-commit hooks, or scripts [1]:

```bash
# One-off queries
claude -p "Explain what this project does"

# Structured output for scripts
claude -p "List all API endpoints" --output-format json

# Streaming for real-time processing
claude -p "Analyze this log file" --output-format stream-json
```

### Parallel Sessions

Three ways to run parallel sessions [1]:

1. **Claude Desktop**: Manage multiple local sessions visually; each gets an isolated worktree
2. **Claude Code on the web**: Run on Anthropic's cloud infrastructure in isolated VMs
3. **Agent teams**: Automated coordination with shared tasks, messaging, and a team lead

**Writer/Reviewer pattern**:

| Session A (Writer) | Session B (Reviewer) |
|--------------------|----------------------|
| `Implement a rate limiter for our API endpoints` | |
| | `Review the rate limiter implementation in @src/middleware/rateLimiter.ts. Look for edge cases, race conditions, and consistency with our existing middleware patterns.` |
| `Here's the review feedback: [Session B output]. Address these issues.` | |

Fresh context improves code review since Claude won't be biased toward code it just wrote.

### Fan-Out Patterns

Loop through tasks calling `claude -p` for each. Use `--allowedTools` to scope permissions [1]:

1. **Generate a task list**: Have Claude list all files needing migration
2. **Script the loop**:
   ```bash
   for file in $(cat files.txt); do
     claude -p "Migrate $file from React to Vue. Return OK or FAIL." \
       --allowedTools "Edit,Bash(git commit *)"
   done
   ```
3. **Test on a few files, then run at scale**: Refine your prompt based on the first 2-3 files

Integrate into pipelines:
```bash
claude -p "<your prompt>" --output-format json | your_command
```

Use `--verbose` for debugging during development.

---

## Common Failure Patterns

Recognizing these patterns early saves time [1]:

| Pattern | Description | Fix |
|---------|-------------|-----|
| **Kitchen sink session** | Start with one task, ask something unrelated, return to first task. Context full of irrelevant information. | `/clear` between unrelated tasks |
| **Correcting over and over** | Claude does something wrong, you correct it repeatedly. Context polluted with failed approaches. | After two failed corrections, `/clear` and write a better initial prompt incorporating what you learned |
| **Over-specified CLAUDE.md** | CLAUDE.md is too long; important rules get lost in noise | Ruthlessly prune; if Claude does something correctly without the instruction, delete it or convert to a hook |
| **Trust-then-verify gap** | Claude produces plausible-looking implementation that doesn't handle edge cases | Always provide verification (tests, scripts, screenshots). If you can't verify it, don't ship it |
| **Infinite exploration** | Ask Claude to "investigate" without scoping. Claude reads hundreds of files, filling context | Scope investigations narrowly or use subagents so exploration doesn't consume main context |

---

## Sources

1. [Claude Code Best Practices](https://code.claude.com/docs/en/best-practices) - Official Anthropic documentation on tips and patterns for Claude Code
