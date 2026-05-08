[← Back to Topic](README.md) | [← Home](../../README.md)

# Strategic Illegibility: Protecting Moats in an AI-Legible World

## Index

- [Overview](#overview)
- [What “Legible to AI” Means](#what-legible-to-ai-means)
- [The Commoditization Risk](#the-commoditization-risk)
- [Strategic Illegibility: What to Keep Partially Unwritten](#strategic-illegibility-what-to-keep-partially-unwritten)
- [Decision Tests](#decision-tests)
- [Practical Patterns (Inference)](#practical-patterns-inference)
- [Sources](#sources)

---

## Overview

Brian Halligan argues that the push to make companies “legible to AI” creates a strategic tension: the same structure that lets agents operate your business faster can also make your operating logic easier for vendors (and, indirectly, competitors) to generalize into standardized “best practices.” The result is a new discipline he calls **strategic illegibility**: being deliberate about what should *not* become machine-executable, even as you modernize your data and workflows. [1]

## What “Legible to AI” Means

In Halligan’s framing, “legible” organizations move knowledge and process from tacit to explicit and from messy to structured, for example: [1]

- Knowledge written down rather than held only in people’s heads
- Data in structured formats rather than screenshots and PDFs
- Context attached to decisions rather than assumed
- Consistent labels, clear ownership, and understandable operating logic

This can be viewed as building a second “digital twin” version of the company that agents can navigate and operate. [1]

## The Commoditization Risk

Halligan’s core risk claim is not about vendors training foundation models on your data (often restricted by contract), but about **pattern matching**: once your playbook is legible enough for an agent to execute, it is legible enough for a vendor to abstract across many customers, productize it, and redistribute it as a reference architecture. “Your edge becomes their feature.” [1]

This is closely related to broader AI-era moat erosion themes: when reading, writing, and reasoning over operational artifacts gets cheaper, competitive advantage shifts away from “hidden implementation” toward harder-to-transfer assets. (Compare with [AI-Assisted Decompilation and the End of Code Secrecy](ai-assisted-decompilation.md).)

## Strategic Illegibility: What to Keep Partially Unwritten

Halligan suggests the highest-value things to keep partially illegible are the aspects that make a company “weird, specific, and hard to copy,” including: [1]

- **Taste**: what “good” looks like in the domain
- **Founder judgment / mental models**
- **Customer intimacy and trust**
- **Timing and contrarian beliefs** that are not yet formalized into strategy
- **Negotiation intuition**: price floors, concession patterns, deal instincts
- **Informal power maps**: who actually decides, unwritten escalation paths

He also argues that over-optimizing for legibility can subtly constrain organizations to what models can read, producing “machine average” strategy and taste rather than differentiated intuition (“soul”). [1]

## Decision Tests

Heuristics proposed for deciding what should remain less legible: [1]

1. **Vendor test**: If a vendor sold an agent tomorrow that did this exact thing, would your competitive position weaken? If yes, avoid making it fully legible.
2. **Departure test**: If the person carrying this knowledge left, would you lose a capability that takes six months to rebuild? If yes, you need *some* legibility for continuity, but not necessarily full playbook exposure.
3. **Replication test**: Could a smart competitor read the legible version and rebuild the underlying capability? If yes, reduce what’s captured or abstract it.

## Practical Patterns (Inference)

Operational patterns that follow from Halligan’s argument (inference from [1]):

- **Two-tier documentation**: document “what” and “how” for execution; keep “why we do it this way” at a higher abstraction level where possible.
- **Redact or abstract price/negotiation playbooks**: capture guardrails and principles; avoid fully structured concession ladders and floors.
- **Capture continuity without full strategy exposure**: write “failure recovery” and “handoff” docs that preserve capability without encoding the differentiating recipe.
- **Separate internal vs vendor-visible artifacts**: assume anything in a third-party agent platform may become a pattern-mined signal; keep sensitive strategy in systems with tighter access and audit.
- **Invest in embodied learning loops**: keep some edge in practice, feedback, and relationships rather than purely in codified procedures.

## Sources

1. [Brian Halligan: “The Case for Strategic Illegibility” (X post)](https://x.com/bhalligan/status/2051388275756339493)
