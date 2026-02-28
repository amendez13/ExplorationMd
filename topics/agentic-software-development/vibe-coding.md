[← Back to Topic](README.md) | [← Home](../../README.md)

# Vibe Coding

## Index

- [Overview](#overview)
- [Core Principles](#core-principles)
- [Techniques](#techniques)
- [When to Use Vibe Coding](#when-to-use-vibe-coding)
- [Contrast with Traditional Approaches](#contrast-with-traditional-approaches)
- [One-Year Retrospective: Agentic Engineering](#one-year-retrospective-agentic-engineering)
- [GLM-5 and the Agentic Engineering Threshold](#glm-5-and-the-agentic-engineering-threshold)
- [Sources](#sources)

---

## Overview

"Vibe coding" is a term coined by Andrej Karpathy to describe a development style where you fully surrender control to AI coding assistants. Rather than carefully reviewing diffs, understanding code structure, or maintaining mental models of the codebase, vibe coding embraces a hands-off approach where you simply describe what you want and accept what the AI produces [1].

The approach reflects the rapidly improving capabilities of LLMs - when models like Claude's Sonnet become "too good," the traditional developer role of carefully crafting and reviewing code shifts toward directing and accepting AI output.

## Core Principles

### Embrace Exponentials

Accept that AI capabilities are growing exponentially. What seemed impossible months ago is now routine. Vibe coding means leaning into this trajectory rather than clinging to traditional coding practices [1].

### Forget the Code Exists

Stop treating the codebase as something you need to fully understand or control. The code becomes an implementation detail that the AI manages while you focus on the desired outcomes [1].

### Accept All, Always

Don't read the diffs. Trust the AI's changes and accept them without review. This requires a fundamental mindset shift from traditional code review practices [1].

## Techniques

### Voice-First Interaction

Use voice input (e.g., SuperWhisper) to communicate with AI coding assistants. This makes development feel more like conversation than programming, and removes the keyboard as a bottleneck [1].

### Trivial Requests Without Hesitation

Ask for the "dumbest things" without shame - small UI tweaks, minor adjustments, things you're "too lazy to find." The AI's ability to locate and modify code means you don't need to know where things are [1].

Example requests:
- "Decrease the padding on the sidebar by half"
- "Make the button blue"
- "Move this element to the left"

### Error Handling via Copy-Paste

When error messages appear, copy-paste them directly into the AI with no additional context or explanation. Often, the AI can diagnose and fix the issue from the error message alone [1].

### Workarounds Over Debugging

When the AI can't fix a bug through direct approaches, either:
- Work around the issue entirely
- Ask for random changes until the problem disappears

This trades debugging time for iteration speed [1].

## When to Use Vibe Coding

Vibe coding is explicitly positioned as appropriate for certain contexts [1]:

**Good fit:**
- Throwaway weekend projects
- Rapid prototyping
- Learning experiments
- Personal tools you don't need to maintain

**Poor fit:**
- Production systems
- Code that needs to be maintained long-term
- Collaborative projects where others need to understand the code
- Systems where bugs have significant consequences

## Contrast with Traditional Approaches

Vibe coding represents the opposite end of the spectrum from careful, methodical LLM-assisted development:

| Aspect | Traditional LLM-Assisted | Vibe Coding |
|--------|-------------------------|-------------|
| Code review | Review all diffs carefully | Accept all without reading |
| Understanding | Maintain mental model of codebase | Let code grow beyond comprehension |
| Debugging | Systematic diagnosis | Random changes until fixed |
| Input method | Keyboard-centric | Voice-first |
| Error handling | Analyze and understand | Copy-paste with no comment |

The key insight is that different contexts call for different levels of rigor. Vibe coding acknowledges that for low-stakes projects, the overhead of careful review may not be worth the time investment.

## One-Year Retrospective: Agentic Engineering

In February 2026, Karpathy published a one-year retrospective on vibe coding [2]. The core observation: what started as a throwaway approach has evolved into something more serious.

### From Fun to Professional Default

One year ago, LLM capability was low enough that vibe coding was mainly for "fun throwaway projects, demos and explorations." Today, programming via LLM agents is "increasingly becoming a default workflow for professionals, except with more oversight and scrutiny."

### Introducing "Agentic Engineering"

Karpathy proposes "agentic engineering" as the professional evolution:

- **"Agentic"** because the new default is that you are not writing the code directly 99% of the time—you are orchestrating agents who do, acting as oversight
- **"Engineering"** to emphasize that there is an art, science, and expertise to it—something you can learn and become better at, with its own depth of a different kind

### It's Not Magic, It's Delegation

The key insight from the retrospective: "It's not magic, it's delegation." Going faster requires:
- Being explicit about what you want
- Understanding what the AI is doing on your behalf
- Knowing what tools are at its disposal
- Understanding what is hard and what is easy

### Deep Expertise as Multiplier

Karpathy pushes back on the "prompters" framing: at top tiers, deep technical expertise may be *even more* of a multiplier than before because of the added leverage. The ability to understand what the agent is doing and guide it effectively compounds with technical knowledge.

### Workflow Evolution

The retrospective describes a workflow shift toward:
- Making knowledge and context accessible to tools (not just in your head, accessed through legacy web UIs)
- Making things you care about testable, observable, and legible
- Arranging systems so agents can run in longer loops with you removed as the bottleneck

Echoing Tesla's safety framing: "Every action is error"—the same principle now applies to software development.

## GLM-5 and the Agentic Engineering Threshold

In February 2026, Zhipu AI and Tsinghua University released GLM-5, a 744B parameter open-weights model that many in the AI community framed as marking "the moment vibe coding died and agentic engineering was born" [3].

The model's capabilities represent what mature agentic systems look like:
- **Autonomous project execution**: Plans, builds, tests, debugs, and iterates on software projects for hours without human intervention
- **Full-stack agent benchmarks**: 77.8% on SWE-bench Verified (real GitHub bug fixes), #1 on BrowseComp (web browsing tasks)
- **Long-horizon planning**: Ran a simulated vending machine business for 365 days, managing inventory, cash flow, and purchasing decisions

The release was notable for its anonymous debut as "Pony Alpha" on OpenRouter—users couldn't tell it apart from Claude or GPT models until Zhipu revealed its origin. This demonstrated that open-source models have reached parity with proprietary frontier systems for agentic coding tasks.

The gap between "vibe coding" (hands-off, throwaway projects) and "agentic engineering" (professional, oversight-heavy) maps to model capability. As models like GLM-5 become capable of multi-hour autonomous execution with high success rates, the distinction shifts from "can I trust this?" to "how do I architect effective oversight?"

## Sources

1. [Andrej Karpathy on X](https://x.com/karpathy/status/1886192184808149383) - Original "vibe coding" definition (2025)
2. [Andrej Karpathy on X](https://x.com/i/status/2026731645169185220) - One-year retrospective and "agentic engineering" (2026)
3. [x.com](https://x.com/i/status/2027682677302956055) - GLM-5 release and agentic engineering threshold (2026)
