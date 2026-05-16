# LLM Fundamentals — Study Guide

**Audience:** Practitioners with API/prompting experience, little or no transformer internals background  
**Goal:** Build durable mental models — not memorize facts  
**Math style:** Intuition-first; each module ends with an optional deep dive for rigor

---

## Learning Map

```
Tokens → Embeddings → Attention → Transformer → Training → Alignment → Inference → Context → Capabilities → Evaluation
   01          01          02           03            04          05           06          07          08             09
```

Each concept builds on the previous. Read in order the first time.

---

## Modules

| # | Module | Core Question |
|---|--------|---------------|
| [01](01-tokens-embeddings.md) | Tokens & Embeddings | How does text become something a model can compute on? |
| [02](02-attention.md) | The Attention Mechanism | How does the model know which words relate to which? |
| [03](03-transformer-architecture.md) | Transformer Architecture | How do all the pieces fit into one forward pass? |
| [04](04-pretraining.md) | Pretraining | What does the model actually learn from the internet? |
| [05](05-alignment.md) | Alignment (RLHF, SFT, DPO) | How does a base model become an assistant? |
| [06](06-inference-sampling.md) | Inference & Sampling | How does the model produce output one token at a time? |
| [07](07-context-memory.md) | Context Windows & Memory | Why does context size matter, and what are the limits? |
| [08](08-capabilities-limits.md) | Capabilities & Limitations | What can LLMs really do, and why do they fail? |
| [09](09-evaluation.md) | Evaluation | How do we measure whether a model is actually good? |

---

## How to Use This Guide

### First pass (build the skeleton)
Read each module straight through. Focus on the **Mental Model** and **Core Concept** sections. Skip the Optional Deep Dives. Should take ~3–4 hours total.

### Second pass (test yourself)
Come back to each module after 24 hours. Cover the content. Answer the **Active Recall** questions from memory. Check your answers. Any gaps → re-read that section only.

### Third pass (go deep where it matters)
Identify the 2–3 modules most relevant to your work. Read the Optional Deep Dive sections for those.

### Spaced repetition schedule
- Day 0: Read modules 01–03
- Day 1: Recall 01–03; read 04–06
- Day 3: Recall 01–06; read 07–09
- Day 7: Recall all 9 modules
- Day 14: Recall all — review any weak spots

---

## Prerequisites

You should be comfortable with:
- What a neural network is (layers, weights, outputs) — even vaguely
- Calling an LLM API and understanding `temperature`, `max_tokens`, `system_prompt`
- Basic probability: distributions, sampling from a distribution

You do **not** need:
- PyTorch / JAX experience
- Calculus for the intuition sections (only for the deep dives)
- Prior reading of "Attention Is All You Need"
