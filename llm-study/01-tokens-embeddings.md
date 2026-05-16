# Module 01: Tokens & Embeddings

**The one-sentence version:** LLMs don't read words — they read numerical chunks called tokens, each represented as a point in a high-dimensional space where meaning is encoded as geometry.

---

## Mental Model

**Tokens are atoms. Embeddings are their position in meaning-space.**

Imagine a vast 3D space (actually ~4,000D, but bear with it). Every word, phrase, or concept is a point in that space. Words with similar meanings cluster together. "King" and "Queen" are close to each other. "Paris" is to "France" as "Berlin" is to "Germany" — that relationship is literally encoded as a geometric vector.

Now imagine the model can't read letters at all — it only sees numbered coordinates. That's what embeddings are: the coordinates that represent each token's meaning.

---

## Core Concept

### What is a token?

A **token** is the basic unit of text that the model processes. It's not a word — it's a subword chunk determined by a compression algorithm run on the training corpus.

Examples:
```
"Hello, world!"  →  ["Hello", ",", " world", "!"]
"unbelievable"   →  ["un", "bel", "iev", "able"]
"GPT"            →  ["G", "PT"]       ← rare word, split more
"the"            →  ["the"]           ← common word, one token
```

This approach (called **Byte Pair Encoding** or BPE) is a compromise:
- Character-level: too slow (long sequences)
- Word-level: vocabulary explodes (every inflection is a new entry)
- Subword: frequent patterns get their own token, rare words split

**Practical implication:** 1 token ≈ 4 characters in English. A 4,000-word essay ≈ ~5,000 tokens. Non-English languages are often less efficient — some languages take 2–5x more tokens per word because the model trained on less data in that language.

### What is a vocabulary?

The model has a fixed **vocabulary** — a lookup table of all valid tokens, each assigned an integer ID.

GPT-4's tokenizer (cl100k_base) has ~100,000 tokens. Llama 3 uses ~128,000. The model never sees text as strings — only as sequences of integers like `[15339, 11, 1917, 0]`.

### What is an embedding?

When the model sees token ID `15339` (which represents "Hello"), it looks it up in an **embedding table** — a giant matrix where each row is a vector of numbers, one row per token.

```
Token ID 15339  →  [-0.23, 0.87, 0.11, 0.52, ..., -0.04]
                   ←— one vector of ~4,096 floats —→
```

This vector is the token's "meaning" as the model has learned it. Two important properties emerge from training:

**1. Semantic proximity:** Words with similar meanings have similar vectors.
```
cosine_similarity(embed("cat"), embed("kitten"))  → 0.92  (high)
cosine_similarity(embed("cat"), embed("accounting")) → 0.11 (low)
```

**2. Relational geometry:** Relationships between concepts become arithmetic.
```
embed("King") - embed("Man") + embed("Woman") ≈ embed("Queen")
embed("Paris") - embed("France") + embed("Italy") ≈ embed("Rome")
```

This is why LLMs can do analogical reasoning — not because they're "thinking" about the analogy, but because analogical relationships are encoded geometrically.

### Where embeddings come from

Embeddings aren't hand-crafted. They're learned during training. Every time the model makes a prediction, it adjusts the embedding vectors so that its predictions improve. Over billions of training steps on trillions of tokens, the embedding space self-organizes into a geometry that mirrors the structure of human language.

---

## Why It Matters

**Token limits are a hard architectural constraint, not a UI decision.** When you hit a context limit, you're not just hitting a product cap — you're hitting the maximum sequence length the model was trained to handle. That's why workarounds (chunking, RAG) are architectural, not just optimizations.

**Token efficiency affects cost and performance.** Prompts with unnecessary verbosity or inefficient formatting cost more and may degrade quality because you're using up the context window with low-information tokens.

**The embedding layer is the model's first interpretation pass.** Before any attention or reasoning happens, every token becomes a vector. If the model's embedding space doesn't have a good representation of your domain (e.g., highly specialized medical notation), it'll struggle from the first layer.

---

## Common Misconceptions

**"LLMs read words."** No. They read token IDs. "don't" might be one token; "do not" is two. This matters for tokenization edge cases — numbers, special characters, and code are often tokenized differently than you'd expect.

**"Embeddings are lookup tables that don't change during inference."** The initial embedding lookup is fixed, but the representation of each token changes dramatically as it passes through the model's layers. The output of layer 20 for "bank" in "river bank" vs. "bank account" is completely different.

**"Longer context = linearly more cost."** Attention is quadratic in sequence length (O(n²)), not linear. That's why 128k-context models are expensive and slow — 4x the tokens means 16x the attention computation.

---

## Active Recall

Test yourself without looking. Write your answers before checking.

1. What is the difference between a token and a word? Give an example of a word that is more than one token.

2. A Spanish word that appears frequently in the training data will take how many tokens compared to an equally frequent English word? Why?

3. What does it mean geometrically that `embed("King") - embed("Man") + embed("Woman") ≈ embed("Queen")`?

4. If you send a 100,000-token prompt to a 128k-context model, and attention is O(n²), roughly how much more computation does that require compared to a 50,000-token prompt?

5. Why would a model fine-tuned only on English struggle with a medical abbreviation like "ICH" (intracranial hemorrhage) even if the abbreviation appears in English text?

---

## Optional Deep Dive: The Math

### Embedding matrix dimensions

If vocabulary size is $V$ and embedding dimension is $d$, the embedding matrix $E$ has shape $[V \times d]$.

For GPT-3: $V = 50{,}257$, $d = 12{,}288$ → embedding matrix has ~618M parameters, about 10% of the entire model.

### Cosine similarity

The "closeness" of two embeddings is typically measured by cosine similarity, not Euclidean distance:

$$\text{sim}(a, b) = \frac{a \cdot b}{\|a\| \|b\|}$$

Range: $[-1, 1]$. Values near 1 = semantically similar; near -1 = opposite meaning.

### Why cosine, not Euclidean?

Embeddings can have very different magnitudes for unrelated reasons (frequency of the word in training data, etc.). Cosine similarity normalizes this out — it measures direction, not length.

### Positional encoding

One problem: the embedding lookup doesn't encode *where* a token appears in the sequence. "Dog bites man" and "Man bites dog" have identical per-token embeddings before positional encoding.

The fix: add a **positional encoding** vector to each token's embedding. The original Transformer paper used sine/cosine functions at different frequencies. Modern models (RoPE, ALiBi) use learned or relative position encodings that generalize better to longer sequences.

$$x_i' = x_i + \text{PE}(i)$$

where $\text{PE}(i)$ is a position-specific vector that makes token 5 distinguishable from token 500.
