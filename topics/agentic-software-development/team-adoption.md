[← Back to Topic](README.md) | [← Home](../../README.md)

# Team Adoption of Agentic Development

## Index

- [The Shift](#the-shift)
- [Adoption Framework](#adoption-framework)
- [Infrastructure Requirements](#infrastructure-requirements)
- [Guardrail Helpers and Safety Patterns](#guardrail-helpers-and-safety-patterns)
- [Code Quality at Scale](#code-quality-at-scale)
- [Sources](#sources)

---

## The Shift

Software development is undergoing a fundamental transformation. Engineers who have adopted agentic tools report that their jobs have changed substantially - agents now write essentially all code and handle significant portions of operations and debugging work [1].

The goal for organizations adopting this approach: **agents should be the tool of first resort** for any technical task, rather than directly using editors or terminals.

This represents not just a technical change but a deep cultural shift with significant downstream implications.

## Adoption Framework

### Team Structure

**Designate an "agents captain"** for each team - a person responsible for:
- Thinking about how agents can be brought into the team's workflow
- Sharing experiences and answering questions
- Driving adoption initiatives

### Documentation Practices

**Create and maintain AGENTS.md files** for projects:
- Document project-specific context for agents
- Update whenever the agent does something wrong or struggles with a task
- Treat as living documentation that improves agent effectiveness over time

#### Advanced AGENTS.md Architecture [2]

For organizations managing multiple repositories, consider a **canonical mirror model**:

- Maintain a single authoritative repository for shared agent rules (e.g., `agent-scripts`)
- Downstream repos reference shared rules via a pointer line: `READ ~/Projects/agent-scripts/AGENTS.MD BEFORE ANYTHING`
- Shared blocks (`[shared]` and `<tools>`) live only in the canonical source
- Repo-specific additions go **after** the pointer line in consuming repos
- Helper scripts remain byte-identical across repositories for consistency

**Sync workflow for multi-repo updates:**
1. Pull latest from canonical source
2. Verify each target repo contains the pointer line at top
3. Append repo-local instructions separately (prevents overwrites)
4. Update all helper scripts simultaneously
5. For submodules: repeat pointer check, push changes, bump parent repo SHAs

**Build a skills library:**
- Write reusable skills for anything you get agents to do
- Commit skills to a shared repository
- Enable knowledge sharing across teams

### Tooling Accessibility

**Inventory internal tools** and ensure they are agent-accessible:
- Maintain a list of tools your team relies on
- Assign ownership for making each tool accessible
- Prefer CLI interfaces or MCP servers for agent integration

## Infrastructure Requirements

### Codebase Structure

Structure codebases to be **agent-first**:
- Write tests that are quick to run
- Create high-quality interfaces between components
- This area requires exploration as models evolve rapidly

### Supporting Infrastructure

Build infrastructure around agent workflows:
- **Observability** - understand what agents are doing
- **Trajectory tracking** - record not just committed code but the agent paths that produced it
- **Central tool management** - control which tools agents can access

## Guardrail Helpers and Safety Patterns

Reusable helper tools can enforce safety constraints across agent workflows [2].

### Git Safety

- Use a **committer helper** script that stages exactly specified files and enforces non-empty commit messages
- Require **Conventional Commits** format (`feat|fix|refactor|build|ci|chore|docs|style|perf|test`)
- Safe-by-default: review status/diff before pushing
- No destructive operations (`reset --hard`, `rm`) without explicit consent
- Prefer HTTPS for remotes; avoid repo-wide find-and-replace scripts
- Use `trash` command for deletions as a safety guardrail

### File Management

- Keep files under ~500 lines; refactor/split as needed
- Stage upstream files in `/tmp/` before cherry-picking—never overwrite tracked files directly
- Zero repo-specific imports for maximum portability of helper scripts

### Documentation Validation

- Use tools like **docs-list** to validate documentation structure
- Enforce front-matter requirements: `summary` and `read_when` fields
- Update docs when APIs/behavior change—no shipping without docs updates
- Add `read_when` hints on cross-cutting documentation

## Code Quality at Scale

Managing AI-generated code at scale is an emerging challenge requiring new processes and conventions.

### The "No Slop" Principle

Prevent "functionally-correct but poorly-maintainable code" from accumulating:

1. **Human accountability** - ensure someone is accountable for any code that gets merged
2. **Maintain review standards** - apply at least the same bar as for human-written code
3. **Author understanding** - code submitters must understand what they're submitting, not just that it works

### Review Considerations

As a code reviewer for AI-generated code:
- Don't lower standards because the code "technically works"
- Evaluate maintainability, not just functionality
- Ensure the human author can explain and defend design decisions

## Sources

1. [Greg Brockman on X](https://x.com/gdb/status/2019566641491963946) - OpenAI's internal approach to agentic software development (2026)
2. [agent-scripts](https://github.com/steipete/agent-scripts) - Shared guardrails and helper tools for AI agents with canonical mirror architecture
