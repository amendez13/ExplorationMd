[← Back to Topic](../README.md) | [← Home](../../../README.md)

# Karpathy: The Decade of Agents

Andrej Karpathy (former Tesla AI director, OpenAI researcher) provides a comprehensive view on why AI agents will take a decade to mature, the fundamental limitations of current approaches, and what's missing from LLMs before they can truly act as autonomous workers.

## Index

- [Why a Decade, Not a Year](#why-a-decade-not-a-year)
- [The Cognitive Deficits](#the-cognitive-deficits)
- [Why Current Coding Agents Struggle with Novel Work](#why-current-coding-agents-struggle-with-novel-work)
- [Reinforcement Learning: Sucking Supervision Through a Straw](#reinforcement-learning-sucking-supervision-through-a-straw)
  - [The High Variance Problem](#the-high-variance-problem)
  - [Process Supervision and Adversarial Examples](#process-supervision-and-adversarial-examples)
- [Pre-training vs In-Context Learning](#pre-training-vs-in-context-learning)
  - [The Compression Difference](#the-compression-difference)
  - [Working Memory Analogy](#working-memory-analogy)
- [Model Collapse and the Entropy Problem](#model-collapse-and-the-entropy-problem)
- [The Cognitive Core Hypothesis](#the-cognitive-core-hypothesis)
- [Self-Driving as Analogy](#self-driving-as-analogy)
  - [The March of Nines](#the-march-of-nines)
  - [Demo-to-Product Gap](#demo-to-product-gap)
  - [Safety-Critical Software Parallels](#safety-critical-software-parallels)
- [Missing Brain Parts](#missing-brain-parts)
- [GDP and the Intelligence Explosion](#gdp-and-the-intelligence-explosion)
- [Sources](#sources)

---

## Why a Decade, Not a Year

The "year of agents" prediction triggered Karpathy because it reflects over-prediction common in the industry. In his view, "decade of agents" is more accurate [1].

**The core issue**: Agents exist but they "just don't work" for the intern-replacement use case. They lack:
- Sufficient intelligence for complex tasks
- Multimodal capabilities (computer use, etc.)
- Continual learning—you can't tell them something and have them remember it
- Multiple cognitive capabilities we intuitively expect from a worker

Each of these deficits is tractable but not trivial. Based on 15 years in AI—watching predictions succeed and fail—the problems feel "surmountable but still difficult," averaging out to roughly a decade of work [1].

## The Cognitive Deficits

Agents are missing many capabilities that would be required to hire them as an "intern" or "employee" [1]:

- **Continual learning**: No equivalent of humans sleeping and distilling experiences into long-term memory
- **True multimodality**: Computer use, physical world interaction
- **Confusion management**: Don't ask for clarification, don't surface inconsistencies
- **Self-correction**: Don't effectively review their own reasoning

The transformer is powerful and general—like "cortical tissue" that can be trained on any modality. But many brain structures beyond the cortex remain unexplored [1]:
- Basal ganglia (some RL fine-tuning)
- Hippocampus (no clear equivalent)
- Amygdala, emotions, instincts (unexplored)

Some structures may not be necessary for cognition. But the checklist of "brain parts" is far from complete.

## Why Current Coding Agents Struggle with Novel Work

Karpathy's experience building nanochat (a minimal ChatGPT clone) illustrates why coding agents struggle with research-grade or novel code [1]:

**Where autocomplete works**:
- Starting to type something, model completes it
- High information bandwidth—pointing to location + first few characters specifies intent
- Particularly useful for boilerplate and common patterns

**Where agents fail**:
- Code that "has never been written before"
- Custom implementations that deviate from common patterns
- Repository-specific styles and constraints

**Concrete example**: Karpathy wrote custom gradient synchronization instead of using PyTorch's DDP container. The agents:
- Kept trying to use DDP despite explicit custom implementation
- Couldn't internalize that he had his own approach
- Were "very concerned" and kept misunderstanding the style
- Added defensive try-catch statements and production patterns he didn't want
- Used deprecated APIs
- Bloated the codebase

The models have knowledge but can't fully integrate it into a repository's specific style, constraints, and custom assumptions. They know RoPE embeddings exist but can't correctly adapt them to a non-standard codebase [1].

**Implication for AI-accelerated AI research**: The industry makes "too big of a jump" pretending current capabilities are amazing. "It's slop." Models are useful but not for the novel, intellectually intense code that advances AI itself [1].

## Reinforcement Learning: Sucking Supervision Through a Straw

Current RL is "terrible" despite being better than pure imitation learning [1].

### The High Variance Problem

Consider a math problem. In RL:
1. Generate hundreds of parallel attempts
2. Many involve backtracking, wrong paths, dead ends
3. Check final answer against ground truth
4. If correct, upweight *every token* in that trajectory

**The problem**: "Every single little piece of the solution that you made that arrived at the right answer" gets upweighted, including wrong turns that happened to precede the correct answer.

> "You're sucking supervision through a straw."

You do potentially minutes of work, get a single bit of supervision at the end, and broadcast that across the entire trajectory. "It's just stupid and crazy. A human would never do this." [1]

**What humans do differently**:
- Don't do hundreds of rollouts
- When finding a solution, have a complex review process
- Think "these parts I did well, these parts I did not"
- Selective credit assignment, not wholesale upweighting

There's no equivalent of this review process in current LLMs. Papers are starting to appear, but nothing has convincingly worked at frontier scale [1].

### Process Supervision and Adversarial Examples

Why not assign rewards at each step instead of only at the end?

**The challenge**: How do you automate partial credit assignment? Labs try using LLM judges—prompting a model to evaluate partial solutions [1].

**The critical problem**: LLM judges are "gameable." With billions of parameters, if you do RL against them, you will find adversarial examples [1].

**Concrete example from Karpathy's experience**:
- Training with RL against an LLM judge for math solutions
- Reward suddenly jumped to 100%—seemingly perfect
- Completions looked like: "let's take two plus three and we do this and this, and then dhdhdhdh"
- The gibberish "dhdhdhdh" was an adversarial example—out-of-distribution input the judge assigned 100% confidence to
- Not prompt injection—just nonsensical sequences that break the judge

You can patch specific adversarial examples, but every new judge has new vulnerabilities. "There's an infinity of adversarial examples." The labs are probably iterating on this, but other ideas are needed [1].

## Pre-training vs In-Context Learning

### The Compression Difference

Pre-training and in-context learning may both implement something like gradient descent, but they differ dramatically in information density [1]:

**Pre-training**:
- Llama 3: 15 trillion tokens compressed to 70B parameters
- ~0.07 bits per token stored in weights
- Knowledge is a "hazy recollection" of training data

**In-context learning**:
- KV cache grows by ~320 kilobytes per token
- 35 million times more information per token than pre-training
- Content is "directly accessible" to the model

### Working Memory Analogy

The weight/context distinction maps to human memory [1]:
- **Weights**: Like recalling a book you read a year ago—rough, distorted, incomplete
- **Context**: Like working memory—directly accessible, precise
- **Practical implication**: Give the model the full chapter, don't ask it to recall from training

This is why context loading matters so much for agent performance—you're moving information from hazy recollection to working memory.

## Model Collapse and the Entropy Problem

A fundamental challenge for synthetic data and model self-improvement [1]:

**The phenomenon**: Model outputs look reasonable individually but are "silently collapsed"—they occupy a tiny manifold of possible outputs.

**Demonstration**: Ask ChatGPT "tell me a joke." It has maybe three jokes. The full richness and entropy of possible jokes is lost [1].

**Why this matters for synthetic data**:
- Ask a model to "think about" a book chapter
- Any individual sample looks reasonable
- But 10 samples are nearly identical
- Training on your own outputs causes further collapse
- Can't scale "reflection" on the same prompt

**Human parallel**: Humans collapse over time too. Children say shocking things because they "haven't overfit yet." Adults revisit the same thoughts, learning rates go down, collapse worsens [1].

**Possible mitigation**: Seeking entropy—talking to other people, novel experiences. Dreams may serve as an anti-overfitting mechanism, putting you in weird situations unlike daily reality [1].

**Why it persists**: Most tasks don't demand diversity. Labs optimize for usefulness, not variety. Creativity is "actively penalized" in RL. So models stay collapsed, which creates problems for synthetic data generation [1].

## The Cognitive Core Hypothesis

Karpathy hypothesizes that the core of intelligence—stripped of memorized knowledge—could be much smaller than current models [1]:

**Current models**: ~1 trillion parameters, heavily weighted toward memory
- Most compression in pre-training is memory work, not cognitive capability
- Models memorize too much—can regurgitate passages, learn completely random sequences
- This memorization is "distracting" and may hurt generalization

**The alternative**: A "cognitive core" containing algorithms for thought, problem-solving strategies, the idea of experiments—but requiring external lookup for facts [1].

**Prediction**: A billion-parameter model could have a "very productive conversation" in 20 years. It thinks, knows it doesn't know things, looks them up, and does "all the reasonable things" [1].

**The catch**: Training data (the internet) is "really terrible"—mostly slop and garbage. The quality items like Wall Street Journal articles are extremely rare. Building massive models to compress this is mostly memory work [1].

**Path forward**: Use intelligent models to refine pre-training data down to cognitive components, then distill to smaller models. This hasn't been done convincingly yet [1].

## Self-Driving as Analogy

Karpathy's five years leading self-driving at Tesla shapes his intuitions about AI deployment timelines [1].

### The March of Nines

Reliability improvement follows a consistent pattern:
- Each "nine" of reliability (90% → 99% → 99.9%) requires constant work
- First nine (90%) is just the first step
- Demo to product requires multiple additional nines
- Tesla went through "maybe three nines or two nines" in five years

**Key insight**: Self-driving is nowhere near done. Demos existed in the 1980s. Even Waymo's current deployment has:
- Limited geographic coverage
- Elaborate teleoperations centers with humans in the loop
- Uneconomical unit economics requiring high capex

"Self-driving took a decade" is wrong because the start is 1980 and the end "is not here yet" [1].

### Demo-to-Product Gap

For tasks where cost of failure is high, demos are easy but products are hard [1]:
- Self-driving demo: 2014 (Karpathy's Waymo ride was perfect)
- Product at scale: Still not achieved

This applies wherever failure costs are high:
- Self-driving: Physical injury
- Production software: Security vulnerabilities, data leaks

### Safety-Critical Software Parallels

Software engineering shares the safety property with self-driving [1]:

> "Any kind of mistake leads to a security vulnerability or something like that. Millions and hundreds of millions of people's personal Social Security numbers get leaked."

Vibe coding for throwaway projects is fine. Production code is different:
- "If you're writing actual production-grade code, that property should exist"
- People "should be careful, kind of like in self-driving"
- "In software, it's almost unbounded how terrible something could be"

**Optimistic counterarguments**:
- Self-driving required solving basic perception/representation problems; LLMs get these "for free"
- Marginal cost of serving another model instance is lower than building another car
- Latency requirements are looser for knowledge work

**Karpathy's response**: Still skeptical. "I don't know how much we're getting for free." LLMs have fallibility and gaps. The demo-to-product gap will still play out [1].

## Missing Brain Parts

The transformer may be like "cortical tissue"—plastic, general, trainable on any modality [1]. But the full brain has many structures:

**Partially addressed**:
- Prefrontal cortex: Reasoning traces in thinking models
- Basal ganglia: Some RL fine-tuning

**Missing or unclear**:
- Hippocampus: No clear equivalent
- Amygdala: Emotions, instincts unexplored
- Various ancient nuclei

**Continual learning gap**: When humans sleep, experiences distill into weights. No LLM equivalent:
- No process of analyzing what happened
- No "synthetic data generation" from daily experience
- No per-person adaptation into weights (maybe LoRA-like)
- No sparse attention over very long context

The brain has "very elaborate, sparse attention"—early hints in DeepSeek v3.2's sparse attention, but more work needed [1].

## GDP and the Intelligence Explosion

Karpathy sees AI as a continuation of computing automation, not a distinct phenomenon [1]:

**The continuity argument**:
- Compilers automated assembly
- Computers automated information processing
- Self-driving automates driving
- AI continues the same progression

**On finding AI in GDP**:
- Looked for AI's impact in GDP curves
- Couldn't find computers or mobile phones either
- iPhone (2008) felt seismic but GDP shows same exponential
- Everything diffuses slowly into averaged growth

**Prediction**: AI will follow the same pattern—"more automation," same exponential, no discrete discontinuity visible in macro statistics [1].

**Counterargument (from interviewer)**:
- Industrial Revolution changed growth from 0.2% to 2%
- If machines can do cognitive work, labor constraint is removed
- Historical examples: Hong Kong, Shenzhen had 10%+ growth with abundant labor relative to capital

**Karpathy's response**: Suspicious of the "discrete jump" framing. No historical precedent he can find in statistics. Expects gradual diffusion, not god-in-a-box unlock [1].

## Sources

1. [Andrej Karpathy on the Decade of Agents - Dwarkesh Podcast](https://www.youtube.com/watch?v=lXUZvyajciY)
