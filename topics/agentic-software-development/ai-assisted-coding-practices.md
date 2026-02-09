[← Back to Topic](README.md) | [← Home](../../README.md)

# AI-Assisted Coding Practices

## Index
- [Overview](#overview)
- [Overview](#overview)
- [Usage Philosophy and Personalization](#usage-philosophy-and-personalization)
- [Context Files for Design Clarity](#context-files-for-design-clarity)
- [Middle-Out Design Pattern](#middle-out-design-pattern)
- [Managing LLM Context Limits](#managing-llm-context-limits)


---

## Overview

## Overview

Effective use of AI coding agents requires both tactical techniques and a broader philosophy of experimentation and personalization. This article covers practical patterns for working with AI assistants, from managing context and design documentation to discovering your own optimal workflows.

## Usage Philosophy and Personalization

Individuals develop personal philosophies for when and how to engage AI coding agents, often based on task complexity, familiarity, or the desire to maintain certain skills [1].

**Autonomous vs. Supervised Modes**

Different tasks call for different levels of agent autonomy [2]:
- **"Just fix"**: Point the agent at a bug with minimal instruction and let it determine the approach
- **Guided implementation**: Provide architectural direction but let the agent handle details
- **Pair programming**: Collaborate interactively, reviewing each step before proceeding

**Challenging the Agent**

Treat the agent as a capable peer who can critique your work [2]:
- Ask: "Can you review this code and tell me what could be improved?"
- Request: "Show me a more elegant solution to this problem"
- The agent often suggests better abstractions, more idiomatic patterns, or overlooked edge cases
- This accelerates skill development by exposing you to alternative approaches

**Reusable Skills and Automation**

A core principle: if you do something more than once a day, turn it into a custom skill [2]:
- Create slash commands for repetitive workflows (e.g., `/deploy`, `/test-all`, `/review-pr`)
- Commit skills to git so they're available across projects and team members
- Skills encapsulate domain knowledge and best practices
- Over time, build a library of project-specific and personal productivity commands

Example skill use cases:
- Database migrations with validation steps
- Generating API documentation from code
- Running security checks before commits
- Deploying to specific environments with pre-flight checks [2]

## Context Files for Design Clarity

### The Technique

A context file is an informal document maintained from the start of a project where developers capture their thoughts, design notes, to-do lists, file formats, and module organization ideas [1]. This file serves dual purposes: as a design clarification exercise for the developer and as a comprehensive prompt for the LLM.

### Benefits

Context files provide several advantages in AI-assisted development:

- **Design Clarification**: Writing down even half-formed, speculative thoughts forces developers to think through their design decisions more carefully [1]
- **Comprehensive Prompting**: The accumulated context gives the LLM a richer understanding of the project's goals, constraints, and architecture [1]
- **Reduced Hallucinations**: By providing explicit design decisions and constraints, developers receive more accurate code generation aligned with their actual requirements [1]
- **Living Documentation**: The file evolves with the project, capturing design evolution and rationale

### What to Include

Context files should capture [1]:
- Design thoughts and architectural decisions
- To-do lists and feature plans
- File format specifications
- Module organization and boundaries
- Interface contracts and invariants
- Even informal or speculative ideas that may evolve

## Middle-Out Design Pattern

### The Approach

Middle-out design focuses on the program's core engine as a reusable component with clearly specified inputs, outputs, and invariants, rather than starting from high-level requirements or low-level implementation details [1]. This contrasts with classical top-down design that risks over-constraining interfaces prematurely.

### Core Principles

The middle-out approach emphasizes:

1. **Core Engine First**: Treat the central logic as a reusable component with well-defined boundaries [1]
2. **Clear Specifications**: Define inputs, outputs, and invariants explicitly before implementation [1]
3. **Modular Decomposition**: Break problems into well-specified, individually testable pieces like parsers or data transformers [1]
4. **Defer Optimization**: Don't worry about low-level performance details initially; focus on getting a working, unit-testable engine first [1]
5. **Integration Later**: Design pieces that can be clearly specified to the LLM individually, then integrated [1]

### Advantages Over Top-Down Design

Middle-out design provides several benefits when working with LLMs:

- **Proper Modularity**: Achieves separation of concerns that LLMs typically don't provide when given only high-level program requirements [1]
- **Testability**: Each component can be unit-tested independently before integration [1]
- **Avoids Over-Constraint**: Prevents locking in interface decisions before understanding the core logic [1]
- **Clearer LLM Prompts**: Well-specified components translate to more precise prompts that reduce hallucinations [1]

## Managing LLM Context Limits

Partitioning work into smaller, well-specified pieces serves an additional practical purpose: preventing context window overflow [1]. By breaking down the problem:

- Each piece fits comfortably within the LLM's context limit
- The LLM can focus on one well-defined problem at a time
- Integration logic can be handled separately with minimal context
- Hallucinations decrease when the LLM isn't trying to juggle too much information simultaneously

## Sources

1. [Two Practices for Effective AI-Assisted Programming](https://x.com/i/status/2019391670609940746)
2. [Claude Code Usage Tips from the Creator](https://x.com/i/status/2017742741636321619)
