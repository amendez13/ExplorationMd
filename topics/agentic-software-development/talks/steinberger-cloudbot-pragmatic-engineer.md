[← Back to Topic](../README.md) | [← Home](../../../README.md)

# Peter Steinberger on Agentic Engineering with Cloudbot - Pragmatic Engineer Podcast

Peter Steinberger (creator of PSPDFKit, used on 1B+ devices) discusses his return to tech after a 3-year burnout break and his radical AI-first development workflow. He's building Cloudbot, a personal AI assistant that runs agents with full computer access, controllable via WhatsApp/Telegram. The conversation covers why he no longer reads most code he ships, prompt requests vs pull requests, and closing the feedback loop as the key to effective agent usage.

## Index

- [Background: PSPDFKit to Burnout to AI](#background-pspdfkit-to-burnout-to-ai)
- [The April 2025 Moment: AI Gets Real](#the-april-2025-moment-ai-gets-real)
- [Current Workflow: Parallel Agents and Weaving Code](#current-workflow-parallel-agents-and-weaving-code)
- [Model Selection: GPT 5.2 Codex vs Claude](#model-selection-gpt-52-codex-vs-claude)
- [Closing the Loop: The Core Principle](#closing-the-loop-the-core-principle)
- [CLIs Over MCPs](#clis-over-mcps)
- [Cloudbot: The Hyper-Personal Assistant](#cloudbot-the-hyper-personal-assistant)
- [PRs as Prompt Requests](#prs-as-prompt-requests)
- [Code Reviews Are Dead](#code-reviews-are-dead)
- [What Changed, What Stayed the Same](#what-changed-what-stayed-the-same)
- [The Builder vs Coder Distinction](#the-builder-vs-coder-distinction)
- [Hiring and Skills for This Era](#hiring-and-skills-for-this-era)
- [Why Experienced Devs Struggle with AI](#why-experienced-devs-struggle-with-ai)
- [Advice for New Developers](#advice-for-new-developers)
- [Sources](#sources)

---

## Background: PSPDFKit to Burnout to AI

Peter's journey to AI-first development came after building PSPDFKit for 13 years (a PDF framework used on 1B+ devices), experiencing severe burnout, selling his shares, and disappearing from tech for 3 years without touching a computer for months at a time.

Key observations from the PSPDFKit era that inform his current approach:
- **Polish matters**: PSPDFKit succeeded because it "felt" better than competitors with more features—quality of experience over feature count
- **Developer-focused marketing**: Never did cold outreach; let insightful technical blog posts and conference talks attract customers who would lobby internally
- **Support as differentiator**: CEO doing support personally creates impact; responding within 5 minutes is "magical"
- **PDF complexity**: What looks simple (text selection in PDFs) can take 3 months; assumptions about scale (100 links vs 500,000 links) can break entire architectures

The burnout came from working too hard for too long, but more fundamentally from "not believing in it anymore" and too many management conflicts. Running a company "democratically" as CEO was identified as a mistake.

---

## The April 2025 Moment: AI Gets Real

Peter came back to coding in April 2025, having missed the entire GPT-3 through GPT-4 evolution that made other developers skeptical. His first real experience was Claude Code in beta:

> "I took this big messy side project and dragged it into Google AI Studio with Gemini 2.5... typed 'write me a spec' and it generated 400 lines of spec. I dragged this back into Claude Code and was like 'build' and then 'continue, continue'... and eventually it told me 'it's 100% production ready' and I started it and it crashed."

Despite the crash, this was his "holy mind-blowing moment"—seeing the potential despite the imperfection.

The breakthrough insight: **Models are "ghosts of our collective human knowledge"** and work similarly to humans. You don't get it right the first time. That's why you need feedback loops.

---

## Current Workflow: Parallel Agents and Weaving Code

Peter runs **5-10 agents in parallel** across multiple projects:

> "I have one main project that has my focus and some satellite projects that also need attention but where I can maybe spend 5 minutes, it does something for half an hour, and I try it and it doesn't need much capacity up there."

The mental model is like Starcraft—main base plus side bases for resources. Or chess grandmasters playing multiple boards simultaneously, making quick decisions at each.

**Workflow characteristics:**
- Design features through conversation: "Let's look at this. What options do we have? Did you consider this feature?"
- Don't tell agents what to do—use trigger words like "let's discuss" or "give me options" to stay in planning mode
- Say "build" when ready to execute
- Context-switch constantly between cooking tasks
- "Weave" code into existing structures rather than "merge" it
- Uses "gate" terminology (from agents themselves): "full gate" = lint + build + tests

**Key realization**: "I feel the same friction when I prompt as when I wrote code." You develop intuition for how long things should take, what good output looks like, when the agent is struggling.

---

## Model Selection: GPT 5.2 Codex vs Claude

Peter strongly prefers GPT 5.2 Codex over Claude for complex coding:

> "Whatever OpenAI cooked there is insanely good. Pretty much every prompt I type gives me the result I want, which is insane."

**Claude Code limitations:**
- Reads 3 files then feels "confident enough" to create code
- Requires constant steering to read more context
- Faster output but lower first-try success rate
- Many workarounds needed (plan mode, specific prompting)

**Codex advantages:**
- Takes 10x longer (10-40 minutes of reading before writing)
- Silently reads files extensively before acting
- Much higher first-try success rate on complex features
- No "Opus silliness" or "Ralph Wigum" workarounds needed

> "Claude would read three files and then be confident enough to just create code... Codex will just be silent and read files for 10 minutes."

The tradeoff: You need parallel work to stay productive while Codex thinks. If you only use one terminal, Codex feels "unbearable."

---

## Closing the Loop: The Core Principle

The most important insight for effective AI coding:

> "The big secret is closing the loop. The agent needs to be able to debug and test itself."

**Why AI is good at coding but mediocre at creative writing:**
- Code can be compiled, linted, executed, output verified
- Creative writing has no easy validation mechanism
- Design your systems to enable automated verification

**Practical example**: For a Mac app bug that was hard to debug through the UI:
> "I told it: build a CLI just for debugging that invokes all the same code paths that you can call yourself, then you just iterate and fix it yourself. It cooked for an hour and was done."

The agent built its own debugging tooling, found race conditions and misconfigurations, and fixed them—all because it could close the loop.

**For websites**: Build the core so it can run via CLI, not just browser. Browser loops are slow; CLI loops are fast.

---

## CLIs Over MCPs

Peter considers MCPs a "crutch" and prefers CLIs:

**MCP problems:**
- Pre-export all functions/tools into context at session start
- Can't filter returned data (500 cities when you need one)
- Can't chain operations or write scripts
- Restarts required to add new MCPs

**CLI advantages:**
- Models are "really good at using bash"
- Can filter with jq, chain commands, script workflows
- Dynamic loading—agents can discover and use CLIs on demand
- Built `make-porter` to convert any MCP to a CLI

> "Even right now if you use MCP you have to restart Claude Code which is very user unfriendly."

Cloudbot has no MCP support, but can use any MCP by converting it with make-porter on demand.

---

## Cloudbot: The Hyper-Personal Assistant

Cloudbot evolved from a WhatsApp relay to a full personal assistant with:
- Access to computer, files, email, calendar
- WhatsApp, Telegram, Discord, Slack, Signal, iMessage, Teams
- Home automation (cameras, lights, music, bed control)
- Proactive capabilities via heartbeat pattern
- Voice calling to businesses for reservations

**The bootstrapping experience:**
> "I programmed the model to be curious and go through this bootstrapping phase... it creates a user.md with information about you, a soul.md with core values, and an identity with the name, core emoji, inside jokes..."

This creates the feeling of talking to a friend, not a model. The technology "disappears."

**Key architecture decisions:**
- Everything as CLI for agent accessibility
- Agent can edit its own configuration
- Agent can update itself ("update yourself" → fetches, updates, returns with new features)
- Plugin architecture for extensibility

The Discord experiment: Putting an agent with full computer access in a public Discord. "Absolutely insane" from a security perspective, but it let people experience the product and "everybody who experienced it for a few minutes got hooked."

---

## PRs as Prompt Requests

Peter's radical shift on pull requests:

> "I see PRs more as prompt requests now. I don't read the code. Somebody opens a pull request, I think about the feature, and then with my agent we start off with the PR and I design the feature as I see fit."

**What he wants from contributors:**
- Add the prompts used to generate the code
- Prompts are higher signal than code for understanding solution quality
- Well-written feature requests ("prompt requests") let him just say "build" to his agent

> "If someone sends me a PR that's just a few fixes, I told people please don't do that. It takes me 10 times more time to review than to just type 'fix' in Codex and wait a few minutes."

---

## Code Reviews Are Dead

> "I basically rewrite every pull request."

The role of contributors has shifted:
- PRs give agents understanding of goals
- Sometimes the bug reports are useful
- Code itself is often rewritten to fit project architecture
- Overall PR code quality has dropped due to vibe coding

**What matters now:**
- Architecture discussions over code discussions
- Design decisions and tradeoffs
- System-level thinking

---

## What Changed, What Stayed the Same

**Changed:**
- PRs → Prompt requests
- Code reviews → Architecture discussions
- CI → "Local CI" (agent runs tests before pushing)
- Writing code → "Weaving code" into structures
- Reading code → Watching the stream, occasionally checking key parts

**Stayed the same:**
- Tests are critical (now even more important as feedback loops)
- Documentation matters (agent-generated docs are now "really good")
- System architecture thinking
- Closing feedback loops
- Caring about user experience

> "I write better code now that I don't write code myself."

---

## The Builder vs Coder Distinction

Peter identifies two types of developers with very different AI outcomes:

**Builders** (thrive with AI):
- Care about the outcome, the product
- Care about how it feels
- Willing to let go of implementation details
- Get energy from shipping

**Coders** (struggle with AI):
- Love solving hard problems
- Enjoy the craft of code itself
- Get satisfaction from algorithmic elegance
- "Those are the people who really struggle and often reject AI"

> "I wouldn't say architect. I like the word builder."

---

## Hiring and Skills for This Era

On what he'd look for in hires:

> "Someone who's active on GitHub and does open source and someone where I have the feeling that they love the game."

**Key skills:**
- Open source activity (demonstrates both technical skill and sharing mindset)
- Curiosity and willingness to experiment
- Comfort with uncertainty
- System-level thinking

**Company implications:**
> "I could easily run a company with 30% of the people. You want really senior engineers that really understand what they build but that are also comfortable delegating."

The bottleneck shifts from coding to architectural judgment and verification design.

---

## Why Experienced Devs Struggle with AI

Peter analyzed a blog post from a respected developer dismissing AI coding:

**Mistakes identified:**
- Tested models in chat interface, not agent environment
- Single prompt, no iteration
- Didn't specify environment constraints (macOS version)
- Expected first-try success
- Spent maybe one day before concluding "not good"

> "Do you think I can write bug-free code on the first attempt? Of course it will not work. That's why you have to close the feedback loop."

The skill is in:
- Understanding how models think
- Learning their "language"
- Spending time to build intuition
- Working with the tool, not testing it once

---

## Advice for New Developers

> "I would recommend them to be infinitely curious."

**Practical approach:**
- Check out complex open source projects
- Use AI to explain everything—it's "infinitely patient"
- Ask "why was it built this way" to gain system understanding
- Build things to gain experience (even if not writing much code manually)

**Advantages of being new:**
- Not "tainted" by assumptions about model limitations
- No legacy memories of what models couldn't do
- Use agents in ways experienced devs don't think of

**Challenge:**
- Universities aren't set up to teach this
- "You discover this through pain"

---

## Sources

1. [Pragmatic Engineer Podcast - Peter Steinberger on Cloudbot](https://www.youtube.com/watch?v=8lF7HmQ_RgY)
