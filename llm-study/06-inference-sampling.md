# Module 06: Inference & Sampling

**The one-sentence version:** During inference, the model generates text by repeatedly running a forward pass to get a probability distribution over the next token, then sampling from that distribution — and every parameter you set (temperature, top-p) shapes how that sampling works.

---

## Mental Model

**At each step, the model rolls a weighted die.**

After every forward pass, the model outputs a probability distribution: "there's a 40% chance the next word is 'the', 20% chance it's 'a', 15% chance it's 'an', etc." Then it rolls this weighted die to pick one.

**Temperature** controls how biased the die is:
- Low temperature (0.1): the die is extremely biased toward the highest probability — nearly deterministic, very predictable
- Temperature 1.0: use the raw probabilities as-is
- High temperature (1.5): the die is flatter — lower-probability tokens have more chance, outputs become more varied and surprising

This single insight explains most API parameter behavior.

---

## Core Concept

### Autoregressive generation

LLMs generate text **one token at a time**, left to right, each token conditioned on everything that came before:

```
"The"  → forward pass → sample "cat"
"The cat"  → forward pass → sample "sat"
"The cat sat"  → forward pass → sample "on"
...
```

Each step is a full forward pass through all layers. For a 96-layer model with 1,000 tokens of context, that's 96 × 1,000 = 96,000 operations per token. This is why inference is slow and expensive at long contexts.

This is also why LLMs can't "go back and change an earlier word" — generation is strictly sequential. Each token is committed before the next one begins.

### From forward pass to token

The final layer produces a vector of logits — one number per vocabulary token:

```
Logits: [2.3, -1.5, 4.1, 0.7, ..., -0.2]   ← 100,000+ values
```

High logit = model thinks this is a likely next token.

**Step 1: Apply temperature**
Divide all logits by temperature $T$:
```python
scaled_logits = logits / temperature
```

- $T < 1$: Logits spread further apart → more peaked distribution → more deterministic
- $T > 1$: Logits compressed together → flatter distribution → more random
- $T = 0$: Greedy decoding (always pick the highest-logit token)

**Step 2: Apply softmax**
Convert to probabilities that sum to 1:
```python
probs = softmax(scaled_logits)
```

**Step 3: Filtering (Top-k or Top-p)**
Optionally restrict the set of tokens that can be sampled.

**Step 4: Sample**
Draw one token from the probability distribution.

### Greedy decoding vs. sampling

**Greedy decoding** ($T=0$): always pick the highest-probability token. Deterministic, fast, but often produces repetitive, boring text — the model gets stuck in loops.

**Sampling** ($T>0$): sample from the distribution. Produces more varied text. Standard for creative tasks.

**Beam search**: maintain $k$ candidate sequences simultaneously, extend each, keep the top $k$ at each step. Used in translation where you want the globally best sequence, not just locally best choices. Rarely used in modern chat models.

### Top-k sampling

Only sample from the top $k$ tokens by probability. Everything else gets zeroed out.

```
k=50: Only the 50 most likely tokens are considered.
```

Prevents very unlikely tokens from ever being sampled. But "top 50" means different things at different positions — sometimes the distribution is very flat (many plausible tokens) and sometimes very peaked (only one token makes sense).

### Top-p (nucleus) sampling

A smarter version: include the smallest set of tokens whose cumulative probability exceeds $p$.

```
p=0.9: include tokens until their cumulative probability ≥ 90%
```

In a peaked distribution (clear next word): top-p might include just 3 tokens.
In a flat distribution (many options): top-p might include 500 tokens.

Top-p adapts to the distribution shape, which is why it outperforms top-k in practice.

**Typical production settings:** temperature 0.7–1.0 + top-p 0.9 for chat. Temperature ~0 for code or factual tasks.

### Why hallucinations happen

LLMs hallucinate because **they generate, they don't retrieve.** The model is always producing the most-plausible continuation of the prompt given its weights. If it doesn't have the right information, it produces a plausible-sounding but wrong continuation.

Three hallucination mechanisms:
1. **Missing knowledge:** The fact wasn't in training data → the model fills in something plausible
2. **Conflicting training data:** Multiple conflicting facts in training → model "averages" them
3. **Distributional shift:** The prompt pattern matches something else in training → wrong answer, right format

Temperature doesn't prevent hallucinations — the wrong answer often has the highest probability in the first place. The fix is retrieval (RAG), grounding, or self-verification via chain-of-thought.

### Stopping conditions

Generation stops when:
- The model samples a special `<|endoftext|>` or `<|im_end|>` token (trained to appear at conversation end)
- `max_tokens` is reached
- A user-defined stop sequence appears

Without a stop condition, the model would generate indefinitely.

---

## Why It Matters

**Temperature is the most impactful parameter in your API calls.** Understanding why (the biased die model) lets you reason about whether your task needs low-T (factual extraction, code, structured output) or higher-T (brainstorming, creative writing, diverse options).

**Determinism is a myth at T>0.** Setting `seed` gives reproducibility, but even with a fixed seed, different batch sizes or hardware can change results. For applications requiring exact reproducibility (e.g., audit trails), log the raw outputs.

**Token-by-token generation explains latency patterns.** First-token latency (TTFT) is affected by prompt length. Subsequent-token latency (inter-token delay) is affected by context length (KV cache) and model size. For real-time UX, streaming is required — you can't wait for the whole response.

**Sampling introduces variance.** If you need consistent quality, don't evaluate a model on a single output — sample multiple times and take the best. This is called "pass@k" evaluation in code generation.

---

## Common Misconceptions

**"Temperature controls creativity."** Temperature controls the randomness of the sampling process, which can feel like creativity. But it also controls how often the model makes mistakes. High temperature = higher variance, which includes both surprisingly good and surprisingly bad outputs.

**"Temperature 0 is deterministic."** Mostly true but depends on floating-point implementation. Different hardware or precision can produce different results even at T=0.

**"Larger models need higher temperature."** No systematic relationship. Larger models tend to have more peaked distributions (they're more confident), so you might want slightly higher temperature to get diversity, but it depends heavily on the task.

**"Top-p 1.0 means sample anything."** At top-p=1.0, you sample from the full vocabulary with raw probabilities. This rarely samples very unlikely tokens because their probabilities are very small. Top-p=1.0 is basically unfiltered sampling.

---

## Active Recall

1. Why does generating a 1,000-token response require 1,000 separate forward passes?

2. If temperature is set to 0.1, what happens to the distribution of logits before softmax? How does this affect output predictability?

3. What is the key difference between top-k and top-p sampling, and why is top-p generally preferred?

4. Explain why a language model hallucinates even when it "knows" the subject area.

5. You're building a medical information tool that extracts structured data from patient notes. What temperature/top-p settings would you choose, and why?

---

## Optional Deep Dive: The Math

### Temperature scaling

Given logits $z_i$ for each vocabulary token $i$, temperature-scaled softmax:

$$P(x_i) = \frac{\exp(z_i / T)}{\sum_j \exp(z_j / T)}$$

As $T \to 0$: the highest-logit token gets probability → 1 (greedy)
As $T \to \infty$: all tokens get equal probability (uniform random)

### Entropy of the output distribution

$$H = -\sum_i P(x_i) \log P(x_i)$$

Higher temperature → higher entropy → less certain → more surprising outputs. This is a direct mathematical formalization of "creativity."

### Top-p implementation

```python
def nucleus_sample(logits, p=0.9, temperature=1.0):
    probs = softmax(logits / temperature)
    sorted_probs, sorted_indices = sort(probs, descending=True)
    cumulative_probs = cumsum(sorted_probs)
    # Remove tokens with cumulative probability > p
    sorted_probs[cumulative_probs > p] = 0
    sorted_probs /= sorted_probs.sum()  # renormalize
    return sample(sorted_indices, weights=sorted_probs)
```

### Perplexity and sampling temperature

Interestingly, perplexity (Module 04) is the exponential of cross-entropy, which equals the inverse of the geometric mean token probability. A model with perplexity 10 is "as uncertain as a 10-sided die" on average. This is the evaluation-time equivalent of the sampling concept — and it's why low-perplexity models can often be sampled at lower temperatures and still produce diverse outputs.
