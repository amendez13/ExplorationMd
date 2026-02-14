[← Back to Topic](README.md) | [← Home](../../README.md)

# Senior Engineer AI Coding: Context Loading, Hooks, and Automation

Turning “AI coding” from a chat window into a tuned engineering system is mostly about two things: engineering your inputs (context) and engineering your feedback loops (automation and quality gates) [1].

## Index

- [Preloading Context with Diagram Files](#preloading-context-with-diagram-files)
- [Workflow Automation: Aliases and Personal CLIs](#workflow-automation-aliases-and-personal-clis)
- [Hooks as Quality Gates (Stop Hooks)](#hooks-as-quality-gates-stop-hooks)
- [Handling Drift and Winning Over Skeptics](#handling-drift-and-winning-over-skeptics)
- [Practical Checklist](#practical-checklist)
- [Sources](#sources)

---

## Preloading Context with Diagram Files

A high-leverage pattern is to maintain a small “memory” area in the repo containing purpose-built context files, especially Markdown with Mermaid diagrams that compress system flows, decision points, and dependencies into a form LLMs can ingest quickly [1].

The payoff is shifting work from repetitive “orientation” (searching, reading, re-explaining architecture) into a one-shot context injection that lets you ask higher-level questions immediately (for example, “explain the authentication flow”) and reduces wrong edits by making downstream dependencies visible up front [1].

Practical loading approach: keep diagrams under a predictable directory (for example `AI/diagrams/`) and use your tool’s “append system prompt” / startup context feature to run a shell command that globs and concatenates those files into the initial context (for example `cat AI/diagrams/**/*.md`) [1]. This spends more tokens at startup but buys speed and reliability once execution begins [1].

When to generate diagrams:
- After something works (often at the pull request boundary), diagram it so future AI-assisted work starts from an accurate map [1].
- For older codebases, backfilling diagrams can be one of the best “first” investments because it accelerates future changes and reviews [1].

Adjacent use cases include generating data-flow or security diagrams for customer/compliance requests, and treating structured file types (Markdown, JSON, Mermaid, and media with metadata) as a primary mechanism for feeding models “compressed” context [1].

## Workflow Automation: Aliases and Personal CLIs

Once context loading is stable, remove launch friction:
- Use shell aliases to start specific modes quickly (model choice, permission mode, and “load all diagrams”) so entering a known workflow is a one-command action [1].
- Embrace the terminal’s constrained UI as a feature: it favors fast, keyboard-driven iteration over building (and maintaining) a custom UI for every workflow [1].

Another pattern is writing tiny personal CLIs as wrappers around AI tools: bake in prompt templates, ask a few targeted questions, and generate consistent outputs without re-remembering prompt recipes [1]. A demo example described a “sketch” workflow that prompts for a site concept and style constraints, produces multiple image variations, and then uses a chosen image as input to another agent that scaffolds the site sections [1].

## Hooks as Quality Gates (Stop Hooks)

Hooks move the checks you already trust (typecheck, lint, format, etc.) earlier in the loop—at a time when the agent can fix failures immediately [1].

A concrete example is a “stop hook” that triggers when the agent finishes and waits:
1. Detect whether files changed [1].
2. If they did, run a typecheck gate (example command: `bun typecheck`) [1].
3. If errors exist, return a structured message instructing the agent to fix them and include the error output; otherwise proceed to an automated commit step [1].

Implementation gotcha: if the hook communicates back to the agent by printing a JSON object to stdout (for example via `console.log`), any extra stdout noise can break the protocol. Use stderr for debug logs (for example `console.error`) and keep invoked commands quiet [1].

This setup can be purely local, or standardized as team-shared configuration in important repos to scale baseline quality and reduce “post-generation” mental overhead across an organization [1]. Other checks called out as good fits: formatting, linting, file-length constraints, circular dependency detection, complexity analysis, and duplication detection [1].

## Handling Drift and Winning Over Skeptics

Tooling ergonomics: CLI and IDE are complementary. CLIs optimize for fast, configurable launches with preloaded prompts/context, while IDEs excel at targeted edits, selecting lines, and leveraging diagnostics that can feed warnings back into agent workflows [1].

For skeptical senior engineers, the pitch is not “codegen”, but eliminating the slowest part of the job: issue orientation. Examples of high-value prompts include: summarize history/blame, identify risk and impact, surface security concerns, and list plausible debug paths—turning a day of orientation into a structured starting point for change [1]. A framing used in the discussion: design prompts and workflows around what you’d do with “infinite junior to mid-career talent” (trace history, write specs, enumerate risks, prepare artifacts), then automate a large fraction of it [1].

When an agent drifts:
- Export the conversation and ask a second model to critique as an unbiased “second set of eyes” [1].
- If needed, revert to a previous commit and restart from a revised original prompt rather than steering a deeply drifted session [1].
- Prefer planning modes for anything beyond small edits; they materially reduce drift [1].

## Practical Checklist

- Create a repo-local AI “memory” area (diagrams + context docs) and keep it up to date at PR boundaries [1].
- Add a one-shot loader that concatenates diagram files into initial context (trade tokens for reliability) [1].
- Add aliases for common agent launch modes (model + permissions + context preload) [1].
- Wrap repeated prompt recipes into tiny CLIs that ask a few questions and output consistent artifacts [1].
- Add stop hooks that run real quality gates and feed failures back to the agent as structured output [1].
- Keep hook stdout clean if it’s protocol-bearing JSON; use stderr for debugging [1].
- Use CLI for fast starts and IDE for precise edits/diagnostics; reset drift by restarting from a known-good commit if needed [1].

## Sources

1. [The senior engineer's guide to AI coding: Context loading, custom hooks, and automation](note://76)

