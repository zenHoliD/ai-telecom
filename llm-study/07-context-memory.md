# Module 07: Context Windows & Memory

**The one-sentence version:** The context window is the model's entire working memory — everything it can "see" right now — and its size determines what the model can reason about, at a cost that grows quadratically.

---

## Mental Model

**The context window is a whiteboard. The model can only reason about what's written on the whiteboard right now.**

Everything not on the whiteboard doesn't exist for the model in this moment. Its weights contain long-term knowledge (from pretraining), but everything contextual — the conversation history, the document you pasted, your instructions — must fit on the whiteboard.

When you scroll too far up in a conversation and old messages fall off, that's the model's whiteboard running out of space. The erased information is gone for this inference step.

**The KV cache is an optimization that makes writing on the whiteboard faster** — it remembers previously computed work so you don't redo it each step.

---

## Core Concept

### What is the context window?

The context window is the maximum number of tokens the model can process in a single forward pass. It includes:
- System prompt
- Entire conversation history
- Any documents or tool outputs pasted in
- The model's response so far

**Common sizes (2024–2025):**
- GPT-4o: 128k tokens (~100,000 words)
- Claude 3.7 Sonnet: 200k tokens (~150,000 words)
- Gemini 1.5 Pro: 1M tokens (~750,000 words)
- Llama 3 70B: 128k tokens

At 4 chars/token, 128k tokens ≈ 512k characters ≈ ~200 pages of text.

### Why context size has a quadratic cost

Attention requires comparing every token to every other token:

```
n=1,000 tokens:   1,000² = 1M comparisons
n=10,000 tokens:  10,000² = 100M comparisons
n=100,000 tokens: 100,000² = 10B comparisons
```

This isn't just slow — it also requires storing the full attention matrix in GPU memory. For $n = 128{,}000$ tokens with 32 heads at 16-bit precision, the attention matrix alone is ~100GB. This is why:
- Long-context inference is expensive
- Long-context inference is slow
- Long-context models require special attention approximations (FlashAttention, sparse attention)

### The KV cache

During autoregressive generation, the model generates one token at a time. At each step, it needs to run attention over all previous tokens. Without optimization, this would recompute the Key and Value matrices for every previous token at every step — that's O(n²) computations per token generated.

**The KV cache** stores Key and Value matrices for all previous tokens:

```
Step 1: Compute K, V for all tokens 0–99. Cache them.
Step 2: Generate token 100. Compute new Q. Look up cached K, V. Compute attention. ✓
Step 3: Generate token 101. Compute new Q. Look up cached K, V. Compute attention. ✓
```

With KV caching:
- First token (pre-filling the cache): O(n²)
- Each subsequent token: O(n) — linear in current context length

This makes inference tractable at long contexts, at the cost of GPU memory proportional to $n \times \text{layers} \times d$.

For a 128k-token context, 96-layer, 12k-dim model, the KV cache uses ~50GB of GPU memory. This is why long-context inference requires high-end hardware.

### "Lost in the middle" — attention over long contexts

Counterintuitively, **models perform worse on information in the middle of long contexts**. Experiments show:

```
[Very start]  →  high recall (model starts here)
[Middle]      →  low recall (information "buried")
[Very end]    →  high recall (most recent, still in attention)
```

This appears across many models and is not fully solved by larger context windows. Practical implication: **put critical information at the beginning or end of your prompt**, not buried in the middle.

### Types of memory beyond context

Context windows are one memory tier. A complete picture:

| Type | Mechanism | Size | Persists across turns? |
|------|-----------|------|----------------------|
| In-context | The current context window | Up to 1M tokens | No — reset each session |
| In-weights | Pretraining knowledge | ~10B+ "concepts" | Yes — baked into model |
| KV cache | Computed attention states | Session-only | Sometimes (prefix caching) |
| Retrieval (RAG) | External vector database | Unlimited | Yes — your system |
| External state | Conversation history stored externally | Unlimited | Yes — your system |

**None of these are true persistent memory** in the human sense. For LLMs to have persistent memory across sessions, you need external retrieval systems that feed relevant past information into the context window.

### Prefix caching

Many API providers (Anthropic, OpenAI) now offer **prefix caching**: if your system prompt is the same across many requests, the K/V states for it are cached server-side. You're charged less for cached tokens and get lower latency.

This matters for production systems with long system prompts — always put static content (instructions, documents) before dynamic content (user input) so the static portion can be cached.

### Retrieval-Augmented Generation (RAG)

RAG is the main architectural workaround for limited context windows and knowledge cutoffs:

```
User query
    ↓
Embedding model converts query to vector
    ↓
Search vector database for relevant documents
    ↓
Retrieve top-k documents
    ↓
Inject documents into context window as "retrieved context"
    ↓
LLM answers based on retrieved context + query
```

RAG converts the problem from "can the model remember this?" to "can you retrieve the right document and fit it in context?" — a much more tractable engineering problem.

**RAG tradeoffs:**
- Retrieval quality determines answer quality — garbage in, garbage out
- You're limited to what you thought to put in the database
- Multi-hop reasoning across multiple retrieved chunks is hard
- Still bounded by context window size

---

## Why It Matters

**Context window is your primary tool for steering behavior, not fine-tuning.** Most LLM engineering challenges are context engineering challenges: what to put in, in what order, how to structure it.

**Long context ≠ long memory.** A 200k-token context window doesn't mean the model can reliably use 200k tokens of content. "Lost in the middle" applies. Effective usable context is typically much less than the maximum.

**KV cache budget is a production constraint.** For systems with many concurrent users with long conversations, KV cache memory is the binding resource. This drives many production decisions: context compression, conversation summarization, prefix sharing.

---

## Common Misconceptions

**"Longer context = better performance."** Not necessarily. Beyond some threshold, more context adds noise. Irrelevant information in context actively hurts performance — it's not neutral.

**"The model remembers past conversations."** Without explicit architecture for it (external storage, memory systems), the model has zero memory of past conversations. Each API call starts fresh.

**"RAG is just retrieval + paste."** Simple RAG is. But production RAG involves chunking strategy, embedding model choice, reranking, query expansion, and filtering. Getting it right at scale requires significant engineering.

**"Context compression doesn't lose information."** It does — by definition. Summarizing a 50k-token conversation into 500 tokens loses information. The question is whether the compressed version preserves the information needed for the next task.

---

## Active Recall

1. What is the KV cache, and what computational problem does it solve?

2. If you have a 10,000-token prompt and a 100,000-token prompt, roughly how much more memory does the attention computation require for the longer one?

3. What is the "lost in the middle" problem, and how should you structure prompts to work around it?

4. Name the four types of LLM memory. Which one is the responsibility of the application developer rather than the model provider?

5. Why does prefix caching only work if static content comes before dynamic content in the prompt?

---

## Optional Deep Dive: The Math

### KV cache memory consumption

For a model with:
- $L$ = number of layers
- $H$ = number of heads
- $d_k$ = key/value dimension per head
- $n$ = current sequence length
- Stored at 16-bit (2 bytes)

$$\text{KV cache memory} = 2 \times L \times H \times d_k \times n \times 2 \text{ bytes}$$

For Llama 3 70B ($L=80$, $H=8$ GQA groups, $d_k=128$, $n=128{,}000$):
$$= 2 \times 80 \times 8 \times 128 \times 128{,}000 \times 2 \approx 42 \text{ GB}$$

This is why long-context inference requires high-end GPUs.

### Grouped Query Attention (GQA)

Modern models (Llama 2+, Mistral) use **Grouped Query Attention** — instead of one K, V pair per head, multiple heads share K and V:

- Multi-Head Attention (MHA): $H$ K/V pairs
- Multi-Query Attention (MQA): 1 shared K/V pair
- Grouped Query Attention (GQA): $G$ groups, $H/G$ heads per group

GQA reduces KV cache memory by factor $G$ with minimal quality loss — the primary reason why open-source models can serve longer contexts affordably.

### FlashAttention

Standard attention stores the full $n \times n$ attention matrix in memory, requiring $O(n^2)$ memory. FlashAttention (Dao et al., 2022) reorders the computation to compute attention in tiles that fit in fast GPU cache (SRAM), never materializing the full matrix in slow GPU memory (HBM):

- Memory: $O(n)$ instead of $O(n^2)$
- Speed: 2–4× faster due to reduced memory bandwidth
- Numerically identical to standard attention (exact, not approximate)

This is the main innovation enabling practical 100k+ token contexts.
