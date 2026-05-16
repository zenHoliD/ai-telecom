# Module 03: Transformer Architecture

**The one-sentence version:** A Transformer is a deep stack of identical layers — each one refines every token's representation using attention (global context) and then a small per-token neural network (local reasoning).

---

## Mental Model

**Think of it as a document going through an editorial pipeline.**

You have a rough draft (the input token embeddings). It passes through 96 editorial desks (layers). At each desk, two things happen:
1. Each word looks at every other word and updates its meaning based on context (**attention**)
2. Each word is independently processed through a small expert system (**feed-forward network**)

By the time the document reaches the end of the pipeline, each word's representation contains rich, contextual, layered understanding — not just its original meaning, but its meaning in this specific context, given everything around it.

---

## Core Concept

### The three model types

The Transformer paper introduced an **encoder-decoder** architecture for translation. Since then, three variants dominate:

| Type | Architecture | Examples | Best for |
|------|-------------|----------|----------|
| Encoder-only | Processes full context bidirectionally | BERT, RoBERTa | Classification, embeddings, understanding |
| Decoder-only | Causal (left-to-right) attention only | GPT-4, Llama, Claude | Generation, chat, reasoning |
| Encoder-Decoder | Bidirectional encoder + causal decoder | T5, BART, original Transformer | Translation, summarization |

**Most frontier LLMs (GPT-4, Claude, Llama) are decoder-only.** Knowing this matters because it means: they can't see future tokens during generation, all context is processed causally, and they're optimized for next-token prediction.

### Anatomy of one Transformer layer

A single layer has exactly two sub-operations:

```
Input: x (token representations, shape [n_tokens × d_model])
         ↓
[Multi-Head Self-Attention]     ← global: tokens talk to each other
         ↓ + residual
[Layer Normalization]
         ↓
[Feed-Forward Network]          ← local: each token processed independently
         ↓ + residual
[Layer Normalization]
         ↓
Output: x' (updated representations, same shape)
```

The output has the exact same shape as the input — each layer refines, rather than transforms the structure.

### Residual connections

Notice "+ residual" in the diagram above. After each sub-operation, the **original input is added back** to the output:

```
output = sub_operation(x) + x
```

This is a residual connection (also called a skip connection). It looks minor but it's critical for two reasons:

1. **Gradient flow:** In a 96-layer network, gradients (used in training) must propagate 96 layers backward. Without residuals, they vanish or explode. With residuals, the gradient can "skip" directly from output to early layers.

2. **Preserve information:** If a layer learns nothing useful, the residual ensures the original information isn't destroyed. Each layer only needs to learn *adjustments*, not a complete re-representation.

### Feed-forward network (FFN)

After attention, each token is passed through a small feed-forward network **independently** (no token-to-token interaction here). Typically:

```
x → Linear(d_model → 4×d_model) → GELU activation → Linear(4×d_model → d_model)
```

The intermediate layer is 4× wider than the model dimension. For GPT-3 ($d = 12{,}288$), that's 49,152 dimensions in the FFN's hidden layer.

**What does the FFN do?** Research suggests the FFN layers act as key-value stores — they've been interpreted as "storing facts." Attention routes information; the FFN retrieves and applies knowledge. This is why factual editing (changing what a model "knows") often targets FFN parameters.

### Layer normalization

Applied after attention and after FFN, layer normalization ensures the activations don't drift out of a useful range as they propagate through 96+ layers. Without it, values would grow exponentially and the model would fail to train.

Modern models (GPT-2+) use **Pre-LN** (normalization applied before each sub-operation, not after), which is more stable than the original Post-LN design.

### The full forward pass

For a decoder-only model generating one token:

```
1. Tokenize input → integer sequence [15339, 11, 1917, ...]
2. Embed each token → [n × d_model] matrix
3. Add positional encodings
4. Pass through Layer 1 → Layer 2 → ... → Layer N (each modifies the representation)
5. Apply final layer norm
6. Multiply by "unembedding matrix" (inverse of embedding table, shape [d_model × vocab_size])
7. Get logits: a score for every token in the vocabulary
8. Apply softmax → probability distribution over next tokens
9. Sample from distribution → output token
```

The unembedding step (step 6) projects from the model's internal representation back to vocabulary space, answering: "given what I know about the context, which token should come next?"

### Scale: What "larger" actually means

When you go from GPT-2 (1.5B params) to GPT-4 (est. ~1.8T params), you're mostly:
- Adding more layers (depth)
- Increasing $d_{\text{model}}$ (wider representations)
- Using more attention heads
- Adding more FFN capacity

Larger models don't "think differently" — they have more capacity to route information (attention) and store facts (FFN). Scaling laws (Chinchilla, Kaplan) show that performance improves predictably and smoothly with these increases.

---

## Why It Matters

**Understanding the two-phase structure (attention + FFN) helps you reason about failure modes.** If a model gets facts right but reasoning wrong, that's often an FFN issue. If it loses track of earlier context, that's often an attention issue.

**Architecture shapes what's possible with fine-tuning.** Methods like LoRA (Low-Rank Adaptation) target specific parameter matrices in the Transformer. Knowing which layers encode which types of knowledge helps you choose what to fine-tune.

**Residual connections explain why very large models can be reliably trained.** The "deep learning works" answer for depth > 100 layers is almost entirely due to residual connections. Without them, modern LLMs wouldn't be trainable.

---

## Common Misconceptions

**"The Transformer was designed for LLMs."** No. It was designed for machine translation (2017). The key insight that it could be scaled for general language modeling came from GPT-1 (2018) and BERT (2018).

**"Deeper = always better."** Depth helps up to a point, but it's a compute/memory tradeoff. Wide-and-shallow can outperform deep-and-narrow at the same parameter count for some tasks.

**"The FFN is just a simple linear layer."** The FFN has an intermediate layer that's 4× wider with a nonlinear activation. This makes it a universal approximator within each position. The claim that "FFN layers store factual associations" has significant empirical support.

**"Each layer learns higher-level concepts."** Partially true but oversimplified. In vision models, early layers learn edges and later layers learn concepts. In LLMs, the hierarchy is less clean — syntactic and semantic information are distributed across layers in complex ways.

---

## Active Recall

1. What are the two main sub-operations in each Transformer layer, and what's the functional difference between them?

2. Why are residual connections important for training very deep networks?

3. What is the purpose of the unembedding matrix at the final step of the forward pass?

4. If you needed to edit a model to change a specific factual claim (e.g., "the capital of France is Berlin" → "Paris"), would you target the attention layers, the FFN layers, or the embedding matrix? Why?

5. A decoder-only model and an encoder-only model both process the same sentence. What fundamental structural difference affects what each model can "see" about each token?

---

## Optional Deep Dive: The Math

### Full layer computation

Let $x \in \mathbb{R}^{n \times d}$ be the input to a Transformer layer.

**Attention sub-layer (with Pre-LN):**
$$x' = x + \text{MultiHead}(\text{LN}(x))$$

**FFN sub-layer:**
$$x'' = x' + \text{FFN}(\text{LN}(x'))$$

where $\text{FFN}(z) = W_2 \cdot \text{GELU}(W_1 z + b_1) + b_2$

### Parameter count per layer

For $d = d_{\text{model}}$:
- Q, K, V projection matrices: $3 \times d \times d$ each = $3d^2$ params
- Output projection: $d^2$
- Total attention: $4d^2$
- FFN (W1, W2): $d \times 4d + 4d \times d = 8d^2$
- Total per layer: $\approx 12d^2$

For $d = 12{,}288$ (GPT-3): $12 \times 12288^2 \approx 1.8\text{B}$ params per layer × 96 layers ≈ 175B total (matching GPT-3's published count).

### Scaling laws intuition

Chinchilla (DeepMind, 2022) found that optimal training uses roughly equal compute for model size and training data:

$$\text{Optimal tokens} \approx 20 \times \text{model parameters}$$

A 7B parameter model should train on ~140B tokens for compute-optimal training. Many early models were undertrained (GPT-3 trained 175B params on 300B tokens — now considered undertrained by this standard).
