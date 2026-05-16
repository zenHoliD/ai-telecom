# Module 08: Capabilities & Limitations

**The one-sentence version:** LLMs are powerful pattern-completion systems that have acquired world models, reasoning capabilities, and general knowledge as emergent byproducts of scale — but they fail predictably at tasks that require reliable symbol manipulation, current information, or causal interventions.

---

## Mental Model

**An LLM is a very well-read person with perfect recall of what they read, strong pattern-matching, no ability to verify, no working memory outside their notebook, and no way to learn new things mid-conversation.**

Strong at: synthesizing what they know, explaining, writing, reasoning through familiar patterns, recognizing what's relevant.

Weak at: anything requiring verification, counting precisely, tracking long chains of strict logic, knowing today's date, or remembering your last conversation.

---

## Core Concept

### What LLMs are actually good at

**Language tasks (near-human or better):**
- Summarization, paraphrase, translation
- Grammar correction, style editing
- Q&A over provided text
- Classification, extraction, structuring

**Reasoning tasks (surprisingly capable, not reliable):**
- Multi-step reasoning with chain-of-thought
- Mathematical problem-solving (through pattern, not proof)
- Logical deduction in familiar domains
- Analogical reasoning

**Creative tasks (high quality, high variance):**
- Writing in any style
- Brainstorming and ideation
- Code generation in familiar patterns

**Knowledge retrieval (broad but stale):**
- Factual recall from training data
- Synthesis across domains
- Domain-specific question answering

### Emergent capabilities

These abilities appeared sharply at scale — small models can't do them, large models can:

**In-context learning:** Given 3–5 examples in the prompt, adapt to a new task without any training. A 7B model barely does this; a 70B model does it well.

**Chain-of-thought reasoning:** Step-by-step reasoning that produces more accurate answers than direct answers, especially for math and logic. Appears reliably above ~60B parameters.

**Instruction following:** Precisely following complex multi-step instructions with constraints. Quality improves dramatically with scale and alignment.

**Code interpretation:** Understanding and generating code, simulating execution, debugging. Strong at familiar patterns (Python, JavaScript), weaker at niche or new languages.

### Systematic failure modes

**Hallucination**
The model generates plausible-sounding false information. Root causes:
- Missing knowledge: fact wasn't in training data → model fills with something plausible
- Conflicting training data: model "averages" conflicting signals
- Optimization pressure: the model learned that confident-sounding responses get higher reward
- No verification mechanism: the model can't check if what it's saying is true

**Hallucinations are not random.** They're systematic: the model hallucinates things that *should* exist in context (citations that match the right format, authors who work in the right field, studies with plausible-sounding results). The hallucination is shaped by the distribution of real things around it.

**Counting and precise symbol manipulation**
```
"How many 'r's are in 'strawberry'?" → often wrong
"What is 17 × 293?"                 → often wrong without tools
```

LLMs process tokens, not symbols. They don't have a counter. They predict what a person *would write* if asked this question. Since people often don't write out careful counting, the model doesn't reliably either.

**Long-chain strict logic**
Models can do logical reasoning, but error rates compound over long chains. A 5-step argument where each step is 95% reliable has a (0.95)⁵ ≈ 77% success rate. At 20 steps: 36%.

**Knowledge cutoffs**
No knowledge of events after the training data cutoff. Won't know today's stock price, last week's news, or APIs released last month.

**Positional biases**
- Prefer earlier options in multiple choice (position 1 bias)
- Tend to agree with the human's stated position ("sycophancy")
- Give inconsistent answers to the same question phrased differently

**No self-knowledge**
Models can't reliably report what they know vs. don't know, what their true capabilities are, or what's actually in their weights. Self-reports are generated text, not introspection.

### The "stochastic parrot" vs. "world model" debate

**Stochastic parrot view** (Bender et al., 2021): LLMs are sophisticated pattern matchers that stitch together tokens from training data. They don't "understand" anything — they produce statistically likely text.

**World model view** (many researchers): To reliably predict text about physics, you must learn physics. The patterns in language are grounded in patterns in reality. Something model-like must be emerging.

**The pragmatic answer:** Both are partially right. LLMs clearly have learned rich representations that generalize beyond memorization (they can answer questions about situations never described in training). But they also clearly fail in ways that suggest the representation is incomplete or brittle.

For practitioners: assume the model has useful knowledge but unreliable grounding. Design systems that verify, don't trust blindly.

### Scaling doesn't fix everything

Capability improvements from scale:
- Factual accuracy (somewhat)
- Coherence over long responses (yes)
- Reasoning quality (yes, significantly)
- Instruction following (yes)

Capabilities NOT reliably fixed by scale:
- Hallucination rate (still present in all frontier models)
- Counting and precise arithmetic (marginally better, still unreliable)
- Knowledge cutoffs (architectural, not a scale issue)
- Consistent behavior across equivalent phrasings (improved but not solved)

---

## Why It Matters

**"LLM + human verification" is the right mental model for most production uses.** The model is powerful but unreliable for any task where wrong answers have significant cost. Build systems that route uncertain outputs to human review.

**Tool use dramatically expands reliable capability.** Give the model a calculator: arithmetic problems solved. Give it web search: knowledge cutoff solved. Give it code execution: reliable symbol manipulation. Modern LLMs (Claude, GPT-4o, Gemini) are increasingly tool-calling systems, not just text models.

**Hallucination mitigation strategies:**
1. **RAG:** Ground answers in retrieved documents — model cites sources that exist
2. **Chain-of-thought:** Forces step-by-step reasoning, which surfaces inconsistencies
3. **Self-consistency:** Sample multiple answers, take the majority vote
4. **Constitutional/reflexive checking:** Ask the model to verify its own answer
5. **Keep it in-distribution:** The model is most reliable on tasks similar to its training distribution

---

## Common Misconceptions

**"Confident tone = reliable answer."** Hallucinations are often stated confidently. The model optimizes for sounding right, not being right. Confidence calibration is poor and asymmetric — wrong answers aren't consistently less confident.

**"If the model has seen the information in training, it will recall it accurately."** Not reliably. Retrieval from weights is imperfect. Very specific facts (exact numbers, specific dates) are recalled less reliably than general concepts.

**"Chain-of-thought always helps."** It helps for tasks that benefit from decomposition. For tasks where the bottleneck is missing knowledge, chain-of-thought can generate longer hallucination chains. And for very simple tasks, it wastes tokens.

**"LLMs can't reason."** Too strong. They reason over trained patterns effectively. They fail at reasoning outside those patterns or requiring strict logical consistency over many steps. The frontier is moving — GPT-o1/o3/Claude 3.7 with extended thinking show dramatic reasoning improvements.

---

## Active Recall

1. Give two types of tasks where current LLMs are genuinely near-human or better, and two where they fail systematically.

2. Why do LLMs hallucinate things that "almost exist" (e.g., plausible but fake citations) rather than clearly nonsensical things?

3. What is the practical meaning of "emergent capability," and why does it matter for deployment decisions?

4. If you were building a medical diagnosis support tool with an LLM, what two specific limitations would you most need to engineer around?

5. Why does giving an LLM a code execution tool fundamentally change what it can reliably do?

---

## Optional Deep Dive: Mechanistic Interpretability

### Circuits in LLMs

"Mechanistic interpretability" research attempts to reverse-engineer what specific neurons and attention heads compute. Key findings:

**Induction heads** (found in almost all transformers): Two-head circuit that implements in-context pattern completion. If "A B ... A" appears, the induction circuit predicts "B" for the next token. This is the mechanistic basis for in-context learning.

**Attention head specialization:** Some heads reliably track:
- Syntactic positions (subjects, objects)
- Coreference (pronoun → noun)
- Copy behavior (repeat earlier tokens)
- Negative composition (NOT-style logic)

These have been found consistently across models of different sizes and training runs.

### Superposition hypothesis

The model has more "concepts" to represent than it has neurons. The solution: represent multiple concepts simultaneously in the same neurons using different activation directions in high-dimensional space (superposition).

Evidence: neurons activate for multiple seemingly unrelated concepts (e.g., one neuron activates for "science fiction," "2023," "New Zealand"). This makes the model efficient but also makes interpretation hard — you can't just read off "this neuron = this concept."

### Scaling and capability discontinuities

The emergent ability phenomenon may be partly a measurement artifact. BIG-bench (2022) showed that many "emergent" abilities appear smooth when measured with metrics that can capture partial credit — the discontinuity appears with binary accuracy metrics that go from 0% to 100% as the model crosses a threshold. This doesn't mean emergence is fake, but it complicates claims about specific scale thresholds.
