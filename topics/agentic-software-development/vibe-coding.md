[← Back to Topic](README.md) | [← Home](../../README.md)

# Vibe Coding

## Index

- [Overview](#overview)
- [Core Principles](#core-principles)
- [Techniques](#techniques)
- [When to Use Vibe Coding](#when-to-use-vibe-coding)
- [Contrast with Traditional Approaches](#contrast-with-traditional-approaches)
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

## Sources

1. [Andrej Karpathy on X](https://x.com/karpathy/status/1886192184808149383) - Original "vibe coding" definition (2025)
