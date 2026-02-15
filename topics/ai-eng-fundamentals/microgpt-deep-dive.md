[← Back to Topic](README.md) | [← Home](../../README.md)

# MicroGPT Deep Dive

Andrej Karpathy's minimal GPT implementation strips away all framework abstractions to reveal the core algorithm. In ~150 lines of pure Python with zero dependencies, it demonstrates that modern language models are fundamentally just matrix operations, automatic differentiation, and careful bookkeeping.

## Index

- [Why This Matters](#why-this-matters)
- [The Autograd Engine](#the-autograd-engine)
  - [The Value Class](#the-value-class)
  - [Forward Operations](#forward-operations)
  - [Backward Pass](#backward-pass)
  - [Why This Design is Elegant](#why-this-design-is-elegant)
- [Tokenization](#tokenization)
- [Model Architecture](#model-architecture)
  - [Parameter Initialization](#parameter-initialization)
  - [The State Dictionary](#the-state-dictionary)
- [The Forward Pass](#the-forward-pass)
  - [Embeddings](#embeddings)
  - [RMSNorm](#rmsnorm)
  - [Multi-Head Self-Attention](#multi-head-self-attention)
  - [MLP Feed-Forward](#mlp-feed-forward)
  - [Output Projection](#output-projection)
- [Training](#training)
  - [Cross-Entropy Loss](#cross-entropy-loss)
  - [Adam Optimizer](#adam-optimizer)
  - [The Training Loop](#the-training-loop)
- [Inference](#inference)
  - [Temperature Sampling](#temperature-sampling)
  - [KV Cache Reuse](#kv-cache-reuse)
- [Key Insights](#key-insights)
- [What's Missing vs Production GPT](#whats-missing-vs-production-gpt)
- [Sources](#sources)

---

## Why This Matters

This code embodies Karpathy's philosophy: "This file is the complete algorithm. Everything else is just efficiency." Production models like GPT-4 add CUDA kernels, distributed training, mixed precision, and engineering optimizations—but the fundamental algorithm is exactly what's shown here.

Understanding this implementation provides:
- **Demystification**: GPT is not magic, it's matrix multiplication and gradients
- **Foundation**: Every optimization in production builds on these primitives
- **Debugging intuition**: When frameworks fail, understanding the algorithm helps

---

## The Autograd Engine

The heart of any neural network is automatic differentiation. This implementation builds a complete autograd engine in ~50 lines.

### The Value Class

```python
class Value:
    __slots__ = ('data', 'grad', '_children', '_local_grads')

    def __init__(self, data, children=(), local_grads=()):
        self.data = data
        self.grad = 0
        self._children = children
        self._local_grads = local_grads
```

**What's interesting:**

1. **`__slots__`**: Memory optimization that prevents dynamic attribute creation. In a model with thousands of Values, this matters.

2. **`data`**: The actual scalar value (float)

3. **`grad`**: Accumulated gradient during backprop. Starts at 0.

4. **`_children`**: Tuple of Value objects that produced this Value. Forms the computation graph.

5. **`_local_grads`**: The partial derivatives ∂(this)/∂(child) for each child. This is the key insight—we store the local gradient at creation time, not during backprop.

### Forward Operations

Each operation creates a new Value with its children and local gradients pre-computed:

```python
def __add__(self, other):
    other = other if isinstance(other, Value) else Value(other)
    return Value(self.data + other.data, (self, other), (1, 1))
```

For addition `z = x + y`:
- `∂z/∂x = 1`
- `∂z/∂y = 1`

```python
def __mul__(self, other):
    other = other if isinstance(other, Value) else Value(other)
    return Value(self.data * other.data, (self, other), (other.data, self.data))
```

For multiplication `z = x * y`:
- `∂z/∂x = y`
- `∂z/∂y = x`

```python
def __pow__(self, other):
    return Value(self.data**other, (self,), (other * self.data**(other-1),))
```

For power `z = x^n`:
- `∂z/∂x = n * x^(n-1)` (power rule)

```python
def log(self):
    return Value(math.log(self.data), (self,), (1/self.data,))

def exp(self):
    return Value(math.exp(self.data), (self,), (math.exp(self.data),))

def relu(self):
    return Value(max(0, self.data), (self,), (float(self.data > 0),))
```

For these:
- `∂log(x)/∂x = 1/x`
- `∂exp(x)/∂x = exp(x)` (the beautiful self-derivative)
- `∂relu(x)/∂x = 1 if x > 0 else 0`

### Backward Pass

```python
def backward(self):
    topo = []
    visited = set()

    def build_topo(v):
        if v not in visited:
            visited.add(v)
            for child in v._children:
                build_topo(child)
            topo.append(v)

    build_topo(self)
    self.grad = 1
    for v in reversed(topo):
        for child, local_grad in zip(v._children, v._local_grads):
            child.grad += local_grad * v.grad
```

**What's happening:**

1. **Topological sort**: Build a list where children always appear before parents. This ensures we process nodes in the correct order during backprop.

2. **Seed gradient**: Set `self.grad = 1` because ∂loss/∂loss = 1.

3. **Reverse traversal**: Process nodes from output to input.

4. **Chain rule**: `child.grad += local_grad * v.grad`
   - `v.grad` is ∂loss/∂v (how much loss changes per unit change in v)
   - `local_grad` is ∂v/∂child (how much v changes per unit change in child)
   - Product gives ∂loss/∂child via chain rule

5. **Accumulation with `+=`**: Critical for nodes used multiple times. If x is used twice, gradients from both uses must sum.

### Why This Design is Elegant

The local gradients are computed during the forward pass and stored. The backward pass is just multiplication and addition—no need to know what operation created each node. This separation of concerns makes the code remarkably clean.

---

## Tokenization

```python
docs = [l.strip() for l in open('input.txt').read().strip().split('\n') if l.strip()]
uchars = sorted(set(''.join(docs)))
BOS = len(uchars)
vocab_size = len(uchars) + 1
```

**What's interesting:**

1. **Character-level**: Each character is a token. Simpler than BPE/SentencePiece but works for names.

2. **Vocabulary**: `uchars` is the sorted unique characters. Sorting ensures deterministic token IDs.

3. **BOS token**: "Beginning of Sequence" marker. Uses index `len(uchars)`, so it's always the last token ID.

4. **No EOS**: Uses BOS as both start and end marker. When the model generates BOS, sampling stops.

5. **Vocab size**: Characters + 1 (for BOS). For a names dataset, this might be ~27 (a-z + BOS).

---

## Model Architecture

### Parameter Initialization

```python
n_embd = 16      # Embedding dimension
n_head = 4       # Number of attention heads
n_layer = 1      # Transformer layers
block_size = 16  # Maximum sequence length
head_dim = n_embd // n_head  # = 4

matrix = lambda nout, nin, std=0.08: [[Value(random.gauss(0, std)) for _ in range(nin)] for _ in range(nout)]
```

**What's interesting:**

1. **Tiny dimensions**: Real GPT-2 uses 768 embedding, 12 heads, 12 layers. This uses 16/4/1. Enough to demonstrate the algorithm, trainable in minutes.

2. **`head_dim = n_embd // n_head`**: Each head operates on a slice of the embedding. With 16 dims and 4 heads, each head sees 4 dimensions.

3. **Gaussian initialization**: `std=0.08` is small enough to avoid exploding gradients at start but large enough to break symmetry.

4. **Matrix as list-of-lists**: Each matrix is `[row][col]` where each element is a Value. No numpy, just nested lists.

### The State Dictionary

```python
state_dict = {
    'wte': matrix(vocab_size, n_embd),      # Token embeddings
    'wpe': matrix(block_size, n_embd),       # Position embeddings
    'lm_head': matrix(vocab_size, n_embd)    # Output projection
}

for i in range(n_layer):
    state_dict[f'layer{i}.attn_wq'] = matrix(n_embd, n_embd)
    state_dict[f'layer{i}.attn_wk'] = matrix(n_embd, n_embd)
    state_dict[f'layer{i}.attn_wv'] = matrix(n_embd, n_embd)
    state_dict[f'layer{i}.attn_wo'] = matrix(n_embd, n_embd)
    state_dict[f'layer{i}.mlp_fc1'] = matrix(4 * n_embd, n_embd)
    state_dict[f'layer{i}.mlp_fc2'] = matrix(n_embd, 4 * n_embd)
```

**Components:**

1. **`wte` (word token embeddings)**: Each token ID maps to an embedding vector. Shape: [vocab_size, n_embd].

2. **`wpe` (word position embeddings)**: Each position (0 to block_size-1) has a learned embedding. This is how the model knows token order.

3. **`lm_head` (language model head)**: Projects embeddings back to vocabulary logits for prediction.

4. **Per-layer attention weights**:
   - `wq, wk, wv`: Query, Key, Value projections
   - `wo`: Output projection after attention

5. **Per-layer MLP weights**:
   - `mlp_fc1`: Expand from n_embd to 4*n_embd (the "up" projection)
   - `mlp_fc2`: Contract from 4*n_embd to n_embd (the "down" projection)

---

## The Forward Pass

### Embeddings

```python
def gpt(token_id, pos_id, keys, values):
    tok_emb = state_dict['wte'][token_id]
    pos_emb = state_dict['wpe'][pos_id]
    x = [t + p for t, p in zip(tok_emb, pos_emb)]
```

**What's happening:**

1. Look up token embedding (which row of wte)
2. Look up position embedding (which row of wpe)
3. Add them element-wise

This is the "embedding" that the transformer operates on. Position information is injected here, not via sinusoidal functions (learned positions instead).

### RMSNorm

```python
def rmsnorm(x):
    ms = sum(xi * xi for xi in x) / len(x)
    scale = (ms + 1e-5) ** -0.5
    return [xi * scale for xi in x]
```

**What's interesting:**

1. **Root Mean Square**: Computes `sqrt(mean(x^2))` and scales by its inverse.

2. **No mean subtraction**: Unlike LayerNorm, RMSNorm doesn't subtract mean. Simpler and works just as well.

3. **`1e-5` epsilon**: Prevents division by zero when all elements are near zero.

4. **Why normalize?**: Keeps activations in a reasonable range throughout the network. Without this, values would explode or vanish through layers.

### Multi-Head Self-Attention

This is the core innovation of transformers:

```python
x = rmsnorm(x)
q = linear(x, state_dict[f'layer{li}.attn_wq'])
k = linear(x, state_dict[f'layer{li}.attn_wk'])
v = linear(x, state_dict[f'layer{li}.attn_wv'])
keys[li].append(k)
values[li].append(v)
```

**Step 1: Compute Q, K, V**

- **Query (q)**: "What am I looking for?"
- **Key (k)**: "What do I contain?"
- **Value (v)**: "What information do I provide?"

The linear projections are learned transformations that create these representations.

```python
x_attn = []
for h in range(n_head):
    hs = h * head_dim  # Head start index
    q_h = q[hs:hs+head_dim]
    k_h = [ki[hs:hs+head_dim] for ki in keys[li]]
    v_h = [vi[hs:hs+head_dim] for vi in values[li]]
```

**Step 2: Split into heads**

Each head operates on a different slice of the embedding dimensions. Head 0 sees dims [0:4], head 1 sees [4:8], etc.

```python
    attn_logits = [
        sum(q_h[j] * k_h[t][j] for j in range(head_dim)) / head_dim**0.5
        for t in range(len(k_h))
    ]
```

**Step 3: Compute attention scores**

This is the dot product between query and all keys: `Q · K^T / sqrt(d_k)`

- Dot product measures similarity: higher score = more relevant
- `/ sqrt(head_dim)`: Scaling prevents dot products from getting too large (which would make softmax too peaky)

```python
    attn_weights = softmax(attn_logits)
```

**Step 4: Softmax to get probabilities**

Converts scores to a probability distribution. Each position attends to all previous positions with weights summing to 1.

```python
    head_out = [
        sum(attn_weights[t] * v_h[t][j] for t in range(len(v_h)))
        for j in range(head_dim)
    ]
    x_attn.extend(head_out)
```

**Step 5: Weighted sum of values**

Each output dimension is a weighted sum of the corresponding value dimension across all positions. Positions with higher attention weights contribute more.

```python
x = linear(x_attn, state_dict[f'layer{li}.attn_wo'])
x = [a + b for a, b in zip(x, x_residual)]
```

**Step 6: Output projection and residual**

- `wo` projects concatenated head outputs back to model dimension
- Add residual connection: allows gradients to flow directly through

**Why multi-head?** Different heads can learn different relationship types (syntax, semantics, position, etc.). They look at the same sequence but through different learned transformations.

### MLP Feed-Forward

```python
x_residual = x
x = rmsnorm(x)
x = linear(x, state_dict[f'layer{li}.mlp_fc1'])  # up: n_embd → 4*n_embd
x = [xi.relu() for xi in x]
x = linear(x, state_dict[f'layer{li}.mlp_fc2'])  # down: 4*n_embd → n_embd
x = [a + b for a, b in zip(x, x_residual)]
```

**What's happening:**

1. **Expand**: Project to 4x dimension (gives more capacity)
2. **Non-linearity**: ReLU adds non-linear transformation
3. **Contract**: Project back to original dimension
4. **Residual**: Add skip connection

**Why 4x expansion?** Empirically found to work well. The expansion gives the network more parameters to learn intermediate representations before compressing.

### Output Projection

```python
logits = linear(x, state_dict['lm_head'])
return logits
```

Projects final hidden state to vocabulary logits. Each logit corresponds to the unnormalized probability of that token being next.

---

## Training

### Cross-Entropy Loss

```python
for pos_id in range(n):
    token_id, target_id = tokens[pos_id], tokens[pos_id + 1]
    logits = gpt(token_id, pos_id, keys, values)
    probs = softmax(logits)
    loss_t = -probs[target_id].log()
    losses.append(loss_t)

loss = (1 / n) * sum(losses)
```

**What's happening:**

1. For each position, predict the next token
2. Convert logits to probabilities via softmax
3. Take negative log probability of the correct target
4. Average across all positions

**Why negative log?**
- If model is confident and correct: `prob ≈ 1 → log(1) = 0 → loss ≈ 0`
- If model is wrong: `prob ≈ 0 → log(0) → -∞ → loss is large`

This directly optimizes for assigning high probability to correct tokens.

### Adam Optimizer

```python
learning_rate, beta1, beta2, eps_adam = 0.01, 0.85, 0.99, 1e-8
m = [0.0] * len(params)  # First moment (momentum)
v = [0.0] * len(params)  # Second moment (adaptive LR)
```

```python
lr_t = learning_rate * (1 - step / num_steps)  # Linear decay
for i, p in enumerate(params):
    m[i] = beta1 * m[i] + (1 - beta1) * p.grad
    v[i] = beta2 * v[i] + (1 - beta2) * p.grad ** 2
    m_hat = m[i] / (1 - beta1 ** (step + 1))  # Bias correction
    v_hat = v[i] / (1 - beta2 ** (step + 1))
    p.data -= lr_t * m_hat / (v_hat ** 0.5 + eps_adam)
    p.grad = 0
```

**Key components:**

1. **`m` (momentum)**: Exponential moving average of gradients. Smooths updates and helps escape local minima.

2. **`v` (adaptive LR)**: EMA of squared gradients. Parameters with large gradients get smaller effective learning rates.

3. **Bias correction**: Early in training, m and v are biased toward zero. `/(1 - beta^t)` corrects this.

4. **Update rule**: `param -= lr * m_hat / sqrt(v_hat)`. Combines momentum with per-parameter adaptive learning rates.

5. **Reset grad**: `p.grad = 0` clears for next iteration.

### The Training Loop

```python
num_steps = 1000
for step in range(num_steps):
    doc = docs[step % len(docs)]
    tokens = [BOS] + [uchars.index(ch) for ch in doc] + [BOS]
    n = min(block_size, len(tokens) - 1)

    keys, values = [[] for _ in range(n_layer)], [[] for _ in range(n_layer)]
    losses = []
    ...
    loss.backward()
```

**What's interesting:**

1. **One document per step**: Simple batching (batch size = 1). Production would process multiple documents.

2. **BOS wrapping**: `[BOS] + chars + [BOS]` means model learns to start and end with BOS.

3. **Fresh KV cache**: Reset for each document. Training doesn't reuse across documents.

4. **Truncation**: `min(block_size, ...)` handles documents longer than context window.

---

## Inference

### Temperature Sampling

```python
temperature = 0.5
logits = gpt(token_id, pos_id, keys, values)
probs = softmax([l / temperature for l in logits])
token_id = random.choices(range(vocab_size), weights=[p.data for p in probs])[0]
```

**What's happening:**

1. **Temperature scaling**: Divide logits by temperature before softmax
   - `temp < 1`: Sharpens distribution, more confident/repetitive
   - `temp = 1`: Original distribution
   - `temp > 1`: Flattens distribution, more random/creative

2. **Sampling**: `random.choices` samples from the probability distribution. Unlike greedy (always take max), this introduces randomness.

### KV Cache Reuse

```python
keys, values = [[] for _ in range(n_layer)], [[] for _ in range(n_layer)]
for pos_id in range(block_size):
    logits = gpt(token_id, pos_id, keys, values)
    # keys and values are appended inside gpt()
```

**What's interesting:**

The KV cache persists across generation steps. When generating token 10, we don't recompute keys/values for tokens 0-9—they're already cached. This is crucial for efficient inference (quadratic → linear).

---

## Key Insights

1. **Everything is differentiable**: The entire forward pass builds a computation graph. Calling `backward()` on loss propagates gradients through everything.

2. **Attention is just weighted averaging**: Despite the mystique, attention computes similarity scores and uses them to weight-average values.

3. **Residual connections are critical**: Without `x = x + f(x)`, gradients would vanish through deep networks.

4. **Normalization stabilizes training**: RMSNorm keeps activations bounded, preventing explosion.

5. **Position matters**: Without position embeddings, the model couldn't distinguish "abc" from "cba".

---

## What's Missing vs Production GPT

| Feature | MicroGPT | Production |
|---------|----------|------------|
| Precision | float64 | bf16/fp8 |
| Parallelism | Single-threaded | Tensor/Pipeline parallel |
| Attention | Naive O(n²) | FlashAttention |
| Memory | Python objects | Contiguous tensors |
| Batching | 1 doc/step | Thousands |
| Layers | 1 | 32-96+ |
| Embedding dim | 16 | 4096-12288 |
| Regularization | None | Dropout, weight decay |
| Layer norm | Post-norm | Pre-norm with RMS |
| Activation | ReLU | SwiGLU |

But the algorithm—embedding, attention, MLP, residual, normalize, softmax, cross-entropy, Adam—is identical.

---

## Sources

1. [gist.github.com/karpathy/8627fe009c40f57531cb18360106ce95](https://gist.github.com/karpathy/8627fe009c40f57531cb18360106ce95)
