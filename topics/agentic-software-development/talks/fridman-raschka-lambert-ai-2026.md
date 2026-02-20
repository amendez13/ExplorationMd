[← Back to Topic](../README.md) | [← Home](../../../README.md)

# State of AI 2026: Fridman, Raschka, Lambert

A wide-ranging conversation on Lex Fridman's podcast with Sebastian Raschka (author of *Build a Large Language Model from Scratch*) and Nathan Lambert (post-training lead at Allen Institute for AI, author of the RLHF book). Covers the competitive landscape, scaling laws, post-training advances, open models, and the changing nature of software development.

## Index

- [The Competitive Landscape](#the-competitive-landscape)
  - [US vs China](#us-vs-china)
  - [Why Chinese Labs Release Open Models](#why-chinese-labs-release-open-models)
  - [Model Recommendations by Use Case](#model-recommendations-by-use-case)
- [Architecture: Less Changed Than You Think](#architecture-less-changed-than-you-think)
  - [GPT-2 to Today](#gpt-2-to-today)
  - [Where the Gains Come From](#where-the-gains-come-from)
- [Scaling Laws: Still Holding](#scaling-laws-still-holding)
  - [Three Axes of Scaling](#three-axes-of-scaling)
  - [Pre-Training Economics](#pre-training-economics)
  - [The Data Quality Frontier](#the-data-quality-frontier)
- [Post-Training: The Current Focus](#post-training-the-current-focus)
  - [RLVR and the Aha Moment](#rlvr-and-the-aha-moment)
  - [RLHF vs RLVR](#rlhf-vs-rlvr)
  - [The GRPO Algorithm](#the-grpo-algorithm)
- [Agentic Development Today](#agentic-development-today)
  - [Tool Preferences](#tool-preferences)
  - [Claude Code's Advantage](#claude-codes-advantage)
  - [The Control Spectrum](#the-control-spectrum)
- [Tool Use as the Next Frontier](#tool-use-as-the-next-frontier)
  - [Why Tool Use Matters](#why-tool-use-matters)
  - [Recursive Language Models](#recursive-language-models)
  - [The Trust Problem](#the-trust-problem)
- [The Jagged Frontier](#the-jagged-frontier)
  - [Superhuman at Some Things, Bad at Others](#superhuman-at-some-things-bad-at-others)
  - [The Automated Software Engineer Question](#the-automated-software-engineer-question)
- [Timeline Predictions](#timeline-predictions)
  - [Near-Term (2026)](#near-term-2026)
  - [Medium-Term (Years)](#medium-term-years)
  - [Long-Term (Decades)](#long-term-decades)
- [Learning and Skill Development](#learning-and-skill-development)
  - [Build From Scratch](#build-from-scratch)
  - [Go Narrow After Fundamentals](#go-narrow-after-fundamentals)
  - [The Struggle Matters](#the-struggle-matters)
- [Sources](#sources)

---

## The Competitive Landscape

### US vs China

There won't be a clear technology winner because researchers rotate between labs and ideas flow freely. The differentiating factors are budget, hardware, and organizational culture—not proprietary knowledge [1].

**Anthropic's current position**: Claude Opus 4.5 hype is "almost a meme"—organic enthusiasm from the Claude Code experience. Anthropic is culturally known for "betting very hard on code," which is paying off [1].

**Google's trajectory**: Gemini 3 had high marketing impact at launch, but Claude Opus 4.5 eclipsed the conversation. Google has structural advantages (TPU economics, data center lead time, vertical integration) that may compound over time [1].

**Chinese labs beyond DeepSeek**: DeepSeek "kicked off a movement" but is losing its crown. Z.ai (GLM models), MiniMax, and Kimi Moonshot have shown strongly in recent months. These labs are filing IPO paperwork and doing Western outreach [1].

### Why Chinese Labs Release Open Models

Open weights are a strategy to influence and capture market share when enterprises won't pay for Chinese API subscriptions due to security concerns [1]:

> "They're smart and realize the same constraints: a lot of top US tech companies and other IT companies won't pay for an API subscription to Chinese companies for security concerns."

The government sees open models building international influence. Consolidation pressure will come eventually, but "more open model builders throughout 2026 than there were in 2025" [1].

**License advantage**: Chinese open-weight licenses are "friendlier"—truly unrestricted open source versus Llama/Gemma strings-attached licenses with user count thresholds and reporting requirements [1].

### Model Recommendations by Use Case

**Nathan's usage pattern** [1]:
- ChatGPT GPT-5.2 Thinking or Pro: Information queries, paper searches, code references
- Gemini: Fast tasks, things you could sometimes Google
- Claude Opus 4.5 (always with extended thinking): Code and philosophical discussion
- Grok: Real-time information, finding things on AI Twitter
- Grok 4 Heavy: Hardcore debugging the other models can't solve

**Sebastian's usage pattern** [1]:
- Quick model (ChatGPT auto): Fast lookups, daily tasks
- Pro mode: Thorough checks—references, formatting, figure numbers (can queue and come back later)
- Codeium plugin for VS Code: Code assistance with repository access, less agentic than Claude Code

**Key insight**: "You use it until it breaks, then you change the LLM." Like browsers or text editors—only explore alternatives when something fails [1].

---

## Architecture: Less Changed Than You Think

### GPT-2 to Today

The fundamental transformer architecture remains remarkably stable [1]:

> "You can still start with GPT-2, and you can add things to that model to make it into this other model. So it's all still kind of like the same lineage."

**Changes from GPT-2 to gpt-oss-120b** [1]:
- Mixture of Experts (MoE): Multiple expert feedforward networks with routing
- Group Query Attention: Tweak to the attention mechanism
- RMSNorm instead of LayerNorm
- Different nonlinear activation functions

These are incremental tweaks, not architectural revolutions. You can convert one model to another by "just adding these changes" [1].

### Where the Gains Come From

If architecture is stable, where does progress originate?

1. **Systems improvements**: FP8/FP4 training, better communication, tokens-per-second-per-GPU optimization [1]
2. **Training phases**: Pre-training → mid-training → post-training pipeline [1]
3. **Data quality**: OCR for PDFs, deduplication, filtering, synthetic rephrasing [1]
4. **Algorithmic advances**: RLVR, inference-time scaling [1]

> "The GPUs are different but you probably train gpt-oss-120b way faster in wall-clock time than GPT-2 was trained at the time."

---

## Scaling Laws: Still Holding

### Three Axes of Scaling

All three scaling approaches still work [1]:

1. **Pre-training compute**: Log-increase in compute → linear increase in held-out prediction accuracy
2. **Reinforcement learning**: DeepSeek showed scaling plot where log-increase in RL training compute → linear increase in evaluation performance
3. **Inference-time compute**: More tokens generated → better answers (enabled by RLVR training)

> "It's held for 13 orders of magnitude of compute, why would it ever end?"

### Pre-Training Economics

Pre-training has become a matter of financial viability rather than technical limitation [1]:

**Training costs**:
- DeepSeek V3: ~$5M at cloud market rates
- OLMo 3: ~$2M including engineering overhead and multiple seeds

**Serving costs dominate**:
- Serving to hundreds of millions of users costs billions
- Training is a fixed cost; serving is per-query
- This shifts the calculus toward inference-time scaling

**The inference vs training trade-off** [1]:
- Pre-training: Fixed cost, capability permanently embedded
- Inference scaling: Per-query cost, but avoids large upfront investment
- "How long is my model gonna be on the market if I replace it in half a year?"

### The Data Quality Frontier

Data quality has become as important as data quantity [1]:

**Synthetic data for pre-training**:
- OCR models (DeepSeek OCR, AI2's Almost-OCR) extract trillions of tokens from PDFs
- Rephrasing messy content into structured Q&A format
- Not "purely AI-made"—transformation of existing content

**The filtering funnel**:
- Pre-training datasets: 5-50 trillion tokens (potentially 100T for closed labs)
- Common Crawl is the raw material (hundreds of trillions of tokens)
- Actual training data is a small percentage after filtering

**Scientific method for mixing**:
- Sample small amounts from different sources (GitHub, Stack Exchange, Reddit, Wikipedia)
- Train small models on each mix, measure performance
- Linear regression gives optimal dataset composition
- "If your evaluations change, your dataset changes a lot"

---

## Post-Training: The Current Focus

### RLVR and the Aha Moment

Reinforcement Learning with Verifiable Rewards (RLVR) was the breakout method of 2025 [1]:

**How it works**:
1. Give the model a question with a known correct answer
2. Model generates attempts without tight constraints on intermediate steps
3. Grade completion as correct or incorrect
4. Use that binary signal as reward for RL

**The emergence phenomenon** [1]:
- Models develop step-by-step explanations organically
- Self-correction emerges: "The model itself recognized it made a mistake and then said, 'Ah, I did something wrong, let me try again.'"
- Longer training → longer responses (more tokens)
- This happens without explicitly teaching chain-of-thought

> "This is just so cool that this falls out of just giving it the correct answer and having it figure out how to do it."

**Sebastian's verification experiment** [1]:
- Qwen 3 base model: 15% accuracy on MATH-500
- After just 50 RLVR steps: 50% accuracy
- "You can't tell me it's learning anything fundamentally about math in 50 steps"
- The knowledge was already there from pre-training—RLVR unlocks it

### RLHF vs RLVR

**RLHF** (Reinforcement Learning from Human Feedback) [1]:
- Signal comes from learned reward model of human preferences
- Used for style, tone, formatting, usefulness
- "Finishing touch" that made ChatGPT magical
- **Does not scale**: No scaling law where log-increase in compute gives linear performance
- Seminal paper is literally titled "Scaling Laws for Reward Model Over-Optimization"

**RLVR** [1]:
- Signal comes from verifiable correctness (math, code)
- **Does scale**: OpenAI's o1 showed the scaling plot
- Can allocate much more compute because there's always signal in harder problems
- Domains: Math, code, rubrics (LLM-as-judge with structured evaluation criteria)

> "The simple thing is: the US models are currently better, and we use them."

### The GRPO Algorithm

Group Relative Policy Optimization is popular because the reward is relative to other attempts at the same problem [1]:

**Key property**: If all attempts get the same answer, there's no signal. This means:
- Labs must find harder problems as models improve
- Problems where models succeed 100% of the time become useless for training
- Frontier pushes toward scientific domains, harder software problems

---

## Agentic Development Today

### Tool Preferences

**Sebastian uses Codeium plugin for VS Code** [1]:
- Chat interface with repository access
- Helps but doesn't take over
- "The sweet spot where it is helping me, but it is not taking completely over"

**Why Sebastian avoids more agentic tools** [1]:
- "Maybe I'm a control freak, but I still would like to see a bit what's going on"
- Codeium is less agentic than Claude Code

**Lex's approach** [1]:
- Half-and-half Cursor and Claude Code
- "Fundamentally different experiences and both are useful"
- Uses Claude Code partly to build the skill of "programming with English"

### Claude Code's Advantage

Claude Code appears to utilize Claude Opus 4.5 better than alternatives [1]:

> "You can have Claude Code open, you can have Cursor open, you can have VS Code open, and you can select the same models on all of them—and ask questions, and it's very interesting. Claude Code is way better in that domain. It's remarkable."

The interface design matters. Claude Code makes building feel "warm and engaging" compared to alternatives that are "rough around the edges" [1].

**Real examples of Claude Code utility** [1]:
- Launching a vibe-coded website—Claude "crushed all the issues" from the previous model version
- Data analysis: "Claude was just like, 'Yeah, I've made use of that data, no problem.' And I was like, 'That would've taken me days.'"

### The Control Spectrum

There's a spectrum from micro-control to full delegation [1]:

**Micro-control (Codeium/Cursor)**:
- Looking at diffs
- Understanding code deeply as you progress
- Manually altering generated code

**Macro-control (Claude Code)**:
- Thinking in design space
- Guiding at macro level
- Less reading of the generated code

Both are valid—it's about the skill you want to build and the task at hand.

---

## Tool Use as the Next Frontier

### Why Tool Use Matters

Tool use is "a huge unlock" that could significantly reduce hallucinations [1]:

**The argument**:
- LLMs try to memorize information they shouldn't have to memorize
- For math: use a calculator or Python interpreter
- For facts: do a web search instead of recalling from training

**Current state** [1]:
- gpt-oss-120b was "kind of the first public or open weight model that was really trained with tool use in mind"
- Mostly on the proprietary LLM side currently
- "Not fully utilized yet by the open-source, open-weight ecosystem"

### Recursive Language Models

A December 2025 paper introduced recursive model calling for long-context tasks [1]:

**The approach**:
- Instead of one model processing huge context, break into sub-tasks
- Have the LLM decide what's a good sub-task
- Recursively call LLMs to solve sub-tasks
- Stitch results together

**Result**: Better accuracy than having the LLM try everything at once [1].

**Opportunity**: Each sub-task could involve tool calls (web search, code execution), enabling compound capability.

### The Trust Problem

Tool use requires giving LLMs permission to act in the world [1]:

> "You want to unlock things like having an LLM answer emails for you—or not even answer, but just sort them for you or select them for you. I don't know if I would today give an LLM access to my emails. This is a huge risk."

**Required infrastructure**:
- Containerization so tools can't "wipe your hard drive or whatever"
- Permission systems
- Trust verification

---

## The Jagged Frontier

### Superhuman at Some Things, Bad at Others

Nathan's framing of AI capability [1]:

> "AI is so-called 'jagged,' which will be excellent at some things and really bad at some things."

**Where models excel**:
- Traditional ML systems
- Frontend development
- Data analysis
- Websites and tooling refresh

**Where models struggle**:
- Distributed ML (very little training data on large-scale distributed learning)
- Novel code that "has never been written before"
- Repository-specific styles and customs

### The Automated Software Engineer Question

The AI 2027 report predicts a "superhuman coder" milestone, but Nathan disagrees with the framing [1]:

> "I think it's assigning completeness to something where the models are already superhuman at some types of code."

**The more likely future**:
- Models continue getting better at specific coding skills
- Humans fill in the gaps where models fail
- "Those lines lead to what we already see"
- No discrete "superhuman coder" moment—continuous improvement with persistent gaps

**On the software automation question** [1]:
- "By the end of this year, the amount of software that'll be automated will be so high"
- But distributed ML, complex production systems, novel research code remain hard
- "A year ago we didn't have Claude Code and we didn't really have reasoning models"

---

## Timeline Predictions

### Near-Term (2026)

**Model releases** [1]:
- Expect bigger compute clusters coming online (contracts signed 2022-2023)
- "$2,000 subscription this year" likely (10x the current $200 tier)
- More open-weight model builders than 2025, many from China

**Software development** [1]:
- Software engineering driven more to "system design and goals of outcomes"
- Many more people can create software without looking at code
- "Industrialization of software when anyone can just create software with their fingerprints"

**What won't happen** [1]:
- Computer use still "sucks"—Claude can use your computer, OpenAI had Operator, "they all suck"
- Taking over the whole screen harder than backend API calls

### Medium-Term (Years)

**Tool use as unlock** [1]:
- More models trained with tool use in mind
- Recursive language models for complex tasks
- Trust and containerization infrastructure

**Consolidation** [1]:
- Some $10B+ acquisitions likely (Cursor, Perplexity mentioned as possibilities)
- AI startups have very high premiums
- IPOs less likely while fundraising remains easy

**The dream changing** [1]:
- "One model to rule everything" ethos is "kind of dying"
- Different models for different tasks
- Video generation, image generation, code—all becoming specialized

### Long-Term (Decades)

**Continual learning** [1]:
- The "missing piece" for AGI-like capability
- No LLM equivalent of sleeping and distilling experiences
- Could be a breakthrough that makes transformers work much better
- Or consistent increases over time via context and memory systems

**100 years out** [1]:
- Computing as the umbrella term likely remembered
- Deep learning likely a remembered term
- Individual model architectures probably forgotten
- Physical goods and events at higher premium due to "slop" saturation

---

## Learning and Skill Development

### Build From Scratch

Sebastian's recommendation for learning LLMs [1]:

> "The goal is not, when you build a model from scratch, to have something for everyday use. It's to see what exactly goes into the LLM, what comes out, and how the pre-training works on your own computer."

**The verification advantage**:
- Code either works or doesn't—no ambiguity
- Can unit test against reference implementations
- Load pretrained weights and verify outputs match Transformers library

**The Transformers library caveat** [1]:
- ~400 models, huge codebase
- "Understanding the part you want to understand is finding the needle in the haystack"
- But provides working reference implementations to reverse-engineer

### Go Narrow After Fundamentals

Nathan's career advice [1]:

> "I think people trying to go narrow after doing the fundamentals is good."

**The opportunity**:
- Many research areas have only 3 papers
- Authors will probably email you back (if you put effort into understanding)
- Best people move on to bigger problems, leaving narrow areas under-explored

**Example**: A student reached out about character training (making models funny/sarcastic/serious). "There's like two or three people in the world that were very interested in this." The paper now exists [1].

**Maximum impact with minimum compute**:
- Evaluation work: find something frontier models fail at
- If Claude 4.5's next version includes your evaluation in the blog post, "there's your career rocket ship"

### The Struggle Matters

Both guests emphasized the importance of struggle for learning [1]:

> "If you just fully use [LLMs] for something you love to do, the thing you love to do is no longer there."

**The debugging analogy** [1]:
- Finding a bug after hunting for it: "the best feeling in the world"
- If you go directly to the LLM, you never have that feeling
- But there's a Goldilocks zone—struggle shouldn't be infinite

**How to learn with LLMs** [1]:
- Sebastian: First pass offline without LLMs, second pass with LLMs
- Lex: Use LLM at beginning to lay out full context, but avoid clicking into Twitter/blogs
- "Dedicated offline time where you study two hours a day, and the rest of the day use LLMs"

---

## Sources

1. [Lex Fridman Podcast - State of AI 2026 with Sebastian Raschka and Nathan Lambert](https://www.youtube.com/watch?v=EV7WhVT270Q)
