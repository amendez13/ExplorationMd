# AI Engineering Fundamentals - Source Log

*Chronological record of sources for this topic*

---

### [MicroGPT - Minimal GPT Implementation](https://gist.github.com/karpathy/8627fe009c40f57531cb18360106ce95)
*2026-02-15T00:00:00Z* | Tags: gpt, transformers, autograd, python, neural-networks, attention

Andrej Karpathy's minimal GPT implementation in pure Python with zero dependencies. Demonstrates that the entire GPT algorithm—tokenization, autograd, multi-head attention, transformer layers, Adam optimizer, and sampling—can be expressed in ~150 lines of code.

**Key Points:**
- Custom autograd engine using a `Value` class with automatic differentiation
- Character-level tokenizer with BOS token for names dataset
- Single-layer transformer with multi-head self-attention and MLP feed-forward
- Training loop with Adam optimizer and linear learning rate decay
- Temperature-controlled sampling for inference

---
