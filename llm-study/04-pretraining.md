# Module 04: Pretraining

**The one-sentence version:** Pretraining is the process of predicting the next token in trillions of text samples — a deceptively simple task that forces the model to learn language, facts, logic, and world knowledge as instrumental skills for prediction.

---

## Mental Model

**The model is a PhD student who read everything ever published, optimizing for one goal: always knowing what comes next.**

The student never told to "learn history" or "learn math." They were just told: predict the next word, over and over, for three years, across every book, website, paper, and forum thread ever written.

To get good at prediction, you have to learn everything. You have to learn grammar (otherwise you'll predict wrong word forms). You learn facts (wrong facts produce bad predictions). You learn logical structure (logical fallacies produce inconsistent predictions). You learn code syntax. You learn social conventions.

No one designed these skills in. They emerged as instrumental competencies required to minimize prediction error.

---

## Core Concept

### The training objective: Next Token Prediction

The pretraining task is exactly one thing:

```
Input:  "The capital of France is"
Target: "Paris"
```

Given all previous tokens, predict the next one. Repeat for every position in every document.

The model scores each token in its vocabulary (50,000+ options) and is penalized via **cross-entropy loss** for assigning a low probability to the actual next token. Gradient descent adjusts the model's billions of parameters to reduce this penalty.

This is called **self-supervised learning** — the training signal comes from the data itself (the next token). No human labels required.

### What the training data looks like

The internet, essentially — but curated and deduplicated:
- Web pages filtered for quality (Common Crawl → C4, FineWeb)
- Books (Books3, Project Gutenberg)
- Scientific papers (ArXiv)
- Code (GitHub)
- Wikipedia
- Curated high-quality sources (The Pile, Dolma, RedPajama)

GPT-3 trained on ~300B tokens. Llama 3 trained on ~15T tokens. The data is typically tokenized, shuffled, and packed into fixed-length chunks.

**Data quality > data quantity.** The "Textbooks Are All You Need" paper (Phi-1, 2023) trained a 1.3B model on 1B tokens of synthetic textbook data and matched or outperformed models trained on 100B tokens of web data on coding tasks. What's in the training set matters enormously.

### Scale: the three axes

Performance in pretraining improves along three axes simultaneously:

1. **Model size** (parameters) — more capacity to represent patterns
2. **Training tokens** — more exposure to the data distribution
3. **Compute** (FLOPs) — determines how much you can do on axes 1 and 2

**Scaling laws** (Kaplan et al., 2020; Chinchilla, 2022) show that loss decreases as a smooth power law across all three axes. This means:
- There are no plateaus — more compute always helps (within known ranges)
- The improvement is predictable — you can forecast model quality before training
- The optimal allocation between model size and token count is roughly 1:20 (params:tokens)

### What "learning" actually happens

What does a model actually learn by predicting tokens?

**Linguistic structure:** Grammar, syntax, morphology — prediction requires getting word forms right.

**World knowledge:** Facts get encoded in FFN layers. "The Eiffel Tower is in ____" → "Paris" requires knowing the fact. Repeated exposure across many documents encodes thousands of such associations.

**Reasoning patterns:** "If A → B and B → C, then ____" appears in math, logic, law, and argument. Predicting the conclusion requires learning the pattern.

**Code execution simulation:** Code samples in training data often include outputs and comments. To predict `# output: 42` after `print(6 * 7)`, the model must implicitly simulate execution.

**Style and register:** Academic writing, casual conversation, legal language — each has distinct prediction patterns that the model learns to match.

None of this is explicitly taught. It all emerges from minimizing prediction error at scale.

### The base model

The output of pretraining is a **base model** (also called a foundation model or pretrained model). It:
- Can complete any text with high coherence
- Knows an enormous amount about the world
- Is not an assistant — it will complete "Tell me how to make a bomb" with a story about bomb-making, not a refusal
- Has no notion of turns, instructions, or helpful behavior

Base models are rarely deployed directly. They go through alignment (Module 05) to become useful assistants.

### Emergent abilities

As models scale up, abilities that were absent at smaller scales suddenly appear. These aren't gradual improvements — they're discontinuous jumps:

- **Chain-of-thought reasoning:** Models suddenly began to benefit from "let's think step by step" prompts above ~60B parameters
- **Arithmetic:** Reliable multi-step math appeared at scale despite never being explicitly trained
- **In-context learning (few-shot):** The ability to learn from examples in the prompt without any gradient updates

This emergence is poorly understood. One explanation: some capabilities require a threshold number of "circuits" (learned features working together), and that threshold is only reached at scale.

---

## Why It Matters

**The pretraining distribution determines the model's priors.** A model trained mostly on English will struggle with Thai. A model trained on 2021 data won't know about events in 2024. These aren't bugs — they're direct consequences of the training distribution.

**You cannot fine-tune in knowledge that wasn't in pretraining.** Fine-tuning adjusts behavior and style; it does not inject significant new factual knowledge. If you want the model to know about your proprietary documents, RAG (retrieval) is the tool, not fine-tuning.

**Deduplication matters.** If a document appears 1,000 times in training data, the model memorizes it. This is a privacy risk (GDPR cases have found that models reproduce training data verbatim) and a quality issue (over-representation of specific sources biases the model).

---

## Common Misconceptions

**"Pretraining teaches the model to follow instructions."** No. Pretraining teaches the model to predict tokens. Instruction-following is a separate capability added through alignment (fine-tuning + RLHF). See Module 05.

**"More parameters = smarter model."** Parameters set an upper bound on what the model can represent, but a model with 70B undertrained parameters can underperform a well-trained 7B model. Model size and training quality both matter.

**"The model understands what it learns."** This is contested. The model learns statistical associations and generalizations that produce the right predictions. Whether this constitutes "understanding" is a philosophical question, not an engineering one.

**"Pretraining is finished and fixed."** Training is ongoing. Most frontier labs are continuously training or periodically releasing new checkpoint versions as they incorporate more data.

---

## Active Recall

1. What is the training signal in pretraining, and why is it described as "self-supervised"?

2. If a model was trained on data up to April 2024, what happens when you ask it about events in October 2024?

3. Why can't you just fine-tune a model on your proprietary documents to get it to "know" them reliably?

4. Scaling laws say performance improves smoothly along three axes. What are those axes?

5. A 70B parameter model was trained on 140B tokens (2× parameters). According to Chinchilla scaling laws, is this model optimally trained? Why or why not?

---

## Optional Deep Dive: The Math

### Cross-entropy loss

Given a predicted probability distribution $P$ over the vocabulary and the true next token $y$:

$$\mathcal{L} = -\log P(y)$$

If the model assigned probability 0.01 (1%) to the correct token: $\mathcal{L} = -\log(0.01) = 4.6$
If it assigned 0.9 (90%): $\mathcal{L} = -\log(0.9) = 0.11$

The model is penalized logarithmically — being wrong with high confidence is very expensive.

### Perplexity

Perplexity is the exponentiated average cross-entropy loss — a more interpretable metric:

$$\text{PPL} = \exp\!\left(\frac{1}{N}\sum_{i=1}^{N} -\log P(x_i \mid x_{<i})\right)$$

Perplexity of 10 means: on average, the model is as uncertain as if it had 10 equally likely next tokens. Lower is better. State-of-the-art models achieve PPL < 5 on held-out text.

### Chinchilla scaling law

$$L(N, D) = E + \frac{A}{N^\alpha} + \frac{B}{D^\beta}$$

Where:
- $N$ = number of model parameters
- $D$ = number of training tokens
- $E$ = irreducible entropy (theoretical minimum loss)
- $\alpha \approx 0.34$, $\beta \approx 0.28$, $A, B$ = empirically fit constants

The optimal compute-efficient allocation under a fixed FLOP budget $C = 6ND$:

$$N_{\text{opt}} \propto C^{0.5}, \quad D_{\text{opt}} \propto C^{0.5}$$

Both scale with the square root of compute — roughly equal allocation between parameters and tokens.
