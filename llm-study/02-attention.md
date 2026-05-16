# Module 02: The Attention Mechanism

**The one-sentence version:** Attention lets every token in a sequence ask every other token "how relevant are you to me right now?" and then pull information from the most relevant ones.

---

## Mental Model

**Attention is a soft, learnable lookup table.**

Imagine you're at a dinner party trying to understand a sentence someone just said. You don't treat every word equally — some words are load-bearing for the meaning you're building.

Sentence: *"The animal didn't cross the street because it was too tired."*

When you try to understand what "it" refers to, you scan back and vote: "animal" gets 70% of your attention, "street" gets 5%, "tired" gets 20%, etc. You then pull meaning from those words proportionally.

That's attention. The model learns which words to "look at" when processing each token. It's not rule-based ("pronoun refers to the nearest noun") — it's learned weights that encode millions of linguistic patterns from training data.

---

## Core Concept

### The problem attention solves

Before Transformers, the dominant approach (RNNs) processed tokens one at a time, left to right, compressing everything into a single fixed-size vector at each step. By the time you're at word 200, the "memory" of word 3 has been crushed through 197 compressions.

**Attention eliminates this information bottleneck** by letting any token directly attend to any other token, regardless of distance.

### Queries, Keys, and Values

Attention is built on three matrices: **Q** (Query), **K** (Key), **V** (Value).

Think of it like a search engine:
- **Query** = what you're searching for ("what does 'it' refer to?")
- **Key** = the index entries of all other tokens ("I am the animal token")
- **Value** = the actual content to retrieve ("here's everything I encode about 'animal'")

Each token generates its own Q, K, and V vector by multiplying its embedding by three learned weight matrices.

**Step 1: Compute compatibility scores**
Each token's Query is dot-producted with every other token's Key:
```
score("it", "animal")  → 8.7   (high — they're related)
score("it", "the")     → 0.3   (low — "the" doesn't help)
score("it", "street")  → 2.1   (medium)
```

**Step 2: Softmax into probabilities**
```
attention_weights = softmax([8.7, 0.3, 2.1, ...])
                 → [0.70, 0.01, 0.12, ...]   ← sum to 1.0
```

**Step 3: Weighted sum of Values**
```
output("it") = 0.70 × value("animal") + 0.01 × value("the") + 0.12 × value("street") + ...
```

The output for "it" now contains 70% "animal" information — the token has been contextually updated.

### Self-attention vs. Cross-attention

**Self-attention:** Every token in a sequence attends to every other token *in the same sequence*. This is what happens inside most LLMs (decoder-only models like GPT).

**Cross-attention:** Tokens in one sequence attend to tokens in a *different* sequence. Used in encoder-decoder models (like the original Transformer for translation) where the decoder attends to the encoder's output.

### Causal masking (why you can't attend to the future)

In language generation, the model must predict the next token without "seeing" future tokens. During training, a **causal mask** is applied:

```
"The cat sat on the"  →  predicting "mat"
                      The  cat  sat  on   the
Token: The             ✓    ✗    ✗    ✗    ✗
Token: cat             ✓    ✓    ✗    ✗    ✗
Token: sat             ✓    ✓    ✓    ✗    ✗
Token: on              ✓    ✓    ✓    ✓    ✗
Token: the             ✓    ✓    ✓    ✓    ✓
```

Each token can only attend to itself and previous tokens. This is what makes the training task valid — no cheating by peeking at the answer.

### Multi-head attention

One attention "head" captures one type of relationship. Multi-head attention runs several independent attention operations in parallel, each with different Q/K/V weight matrices:

- Head 1 might learn to track syntactic subjects and objects
- Head 2 might learn to track coreference (what "it" refers to)
- Head 3 might learn positional proximity
- Head 4 might learn semantic similarity

The outputs of all heads are concatenated and projected back to the model dimension. This is why multi-head attention is so powerful — different heads specialize in different linguistic relationships simultaneously.

---

## Why It Matters

**Attention is why LLMs can handle long-range dependencies.** "The president of the United States, who gave a speech last Tuesday, said..." — a model using attention can track "president" through a 20-word intervening clause and correctly link it to what follows.

**Attention patterns explain model behavior.** Researchers can inspect which tokens attend to which during a forward pass. This has revealed that some heads specialize in tracking pronouns, some in subject-verb agreement, some in parenthetical structure.

**Attention is the compute bottleneck.** The softmax over all token pairs is O(n²) in sequence length. This is why doubling your context window quadruples inference cost and latency. It's also the target of most architectural optimizations (FlashAttention, linear attention variants).

---

## Common Misconceptions

**"The model reads left to right during inference."** True for generation (one token at a time), but during training and for tokens already generated, attention looks at all positions simultaneously. It's parallel, not sequential.

**"More attention heads = better."** More heads provide more diversity in what the model can attend to, but beyond a certain point you're adding parameters without information gain. The optimal number of heads is an empirical hyperparameter.

**"Attention tells you what the model is 'thinking about'."** Attention weights show information routing, not reasoning. A model can attend heavily to a token and still make the wrong inference. Interpretability via attention is a research area, not a solved problem.

**"Attention knows where tokens are."** Attention by itself is position-agnostic — the dot product of Q and K doesn't encode position. That's why positional embeddings (covered in Module 01) are necessary. Attention answers "what relates to what?"; positional encoding answers "where is what?"

---

## Active Recall

1. What are the three components of attention (Q, K, V) and what does each represent in the "search engine" analogy?

2. In the sentence "The trophy didn't fit in the suitcase because it was too big," how would you expect attention weights to distribute for the token "it"?

3. Why does attention scale quadratically (O(n²)) with sequence length?

4. What is causal masking and why is it necessary for language model training?

5. If a 16-head attention layer has head specialization, what are two different types of linguistic relationships that different heads might learn?

---

## Optional Deep Dive: The Math

### Scaled Dot-Product Attention

The full attention computation for a sequence of $n$ tokens with embedding dimension $d$:

$$\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^T}{\sqrt{d_k}}\right) V$$

Where:
- $Q \in \mathbb{R}^{n \times d_k}$ — query matrix (one row per token)
- $K \in \mathbb{R}^{n \times d_k}$ — key matrix
- $V \in \mathbb{R}^{n \times d_v}$ — value matrix
- $d_k$ — dimension of keys/queries (a hyperparameter; e.g., 64 per head)

### Why the $\sqrt{d_k}$ scaling?

Without scaling, for large $d_k$, the dot products grow large in magnitude, pushing the softmax into saturation (outputs near 0 or 1). This creates near-zero gradients and kills learning. Dividing by $\sqrt{d_k}$ keeps the variance of the dot products at approximately 1, regardless of dimensionality.

### Multi-head projection

For $h$ heads, each head $i$ learns its own projection matrices $W_i^Q, W_i^K, W_i^V \in \mathbb{R}^{d \times d_k}$:

$$\text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V)$$

The heads are concatenated and projected:

$$\text{MultiHead}(Q,K,V) = \text{Concat}(\text{head}_1, \ldots, \text{head}_h) W^O$$

where $W^O \in \mathbb{R}^{h \cdot d_v \times d}$ projects back to model dimension $d$.

### Causal mask implementation

The causal mask is an upper-triangular matrix of $-\infty$ added to the attention scores before softmax:

$$M_{ij} = \begin{cases} 0 & \text{if } j \leq i \\ -\infty & \text{if } j > i \end{cases}$$

Adding $-\infty$ to a score and then applying softmax produces exactly $0$ attention weight for that position ($e^{-\infty} = 0$).

### KV Cache (preview — covered in Module 07)

During autoregressive generation, K and V for all previously seen tokens don't change. The KV cache stores them so they don't need to be recomputed. Only the new token's Q needs to be compared against cached K/V. This reduces per-step cost from O(n²) to O(n) — critical for long-context generation performance.
