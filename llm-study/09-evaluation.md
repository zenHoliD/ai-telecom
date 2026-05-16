# Module 09: Evaluation

**The one-sentence version:** Evaluating LLMs is hard because "better" is multidimensional and context-dependent — most benchmarks measure narrow proxies that don't transfer to real-world task performance, and frontier models routinely game them.

---

## Mental Model

**Evaluating an LLM is like evaluating a doctor using only a written multiple-choice exam.**

The exam measures something real (medical knowledge), and high scorers are probably better than low scorers. But it misses bedside manner, diagnostic reasoning under uncertainty, the ability to communicate with a scared patient, or whether they'd order the right test for a case not in the textbook.

LLM benchmarks are the written exam. They're useful signals, not ground truth. And when a hospital (model lab) knows they'll be judged on the exam score, they optimize for the exam — sometimes at the expense of the actual skill.

---

## Core Concept

### The evaluation hierarchy

**1. Perplexity (intrinsic)**
Measures how well the model predicts held-out text. Calculated directly from the model, no task required.
- Lower = better
- Very fast and cheap to compute
- Good for comparing models trained the same way on similar data
- Tells you nothing about task performance — a model with perplexity 3 can still hallucinate badly

**2. Task benchmarks (extrinsic)**
Measure accuracy on specific tasks: reading comprehension, question answering, math, code, etc.

**3. Human evaluation**
Real humans rate or rank model outputs. Gold standard but slow, expensive, noisy, and hard to reproduce.

**4. LLM-as-judge**
Use a frontier model (GPT-4, Claude) to rate outputs. Cheap, scalable, somewhat principled. Inherits biases of the judge model.

**5. Real-world performance**
Does the model actually solve the user's problem? The only measure that fully matters, but the hardest to collect.

### Key benchmarks (what they actually test)

**MMLU (Massive Multitask Language Understanding)**
57-subject multiple-choice test covering STEM, humanities, law, medicine. Tests breadth of world knowledge.
- Frontier model scores: 85–95%
- Limitation: multiple choice with 4 options — guessing gets 25%, format gaming is rampant
- Not diagnostic for generation quality

**HumanEval**
164 Python programming problems, each with a docstring and unit tests. The model generates code; tests check correctness (pass@k metric).
- Frontier scores: 80–95%
- Good for code generation capability
- Limitation: narrow coverage (Python, algorithmic problems), not representative of real coding tasks

**GSM8K / MATH**
Grade school math word problems (GSM8K) and competition math (MATH). Tests arithmetic reasoning.
- GSM8K frontier scores: 90–99%
- MATH is harder (50–85% at frontier)
- Limitation: many problems have leaked into training data; scores are inflated

**HellaSwag**
Commonsense reasoning: choose the most likely sentence completion in physical activity descriptions.
- Near-saturated: frontier models score ~95%
- Originally designed as hard for models, now mostly tests memorization

**MMLU-Pro / BigBench Hard**
Harder variants designed as models started saturating the originals. Arms race between evaluation difficulty and model capability.

**GPQA (Graduate-Level Google-Proof Q&A)**
PhD-level questions in biology, physics, chemistry that experts verify Google can't solve by search.
- Expert human score: ~65%
- Frontier models: 55–75%
- Designed to be genuinely hard and resistant to contamination

**LiveBench**
New questions added monthly; all questions are post-model-cutoff so they can't be in training data. The only major benchmark with contamination resistance by design.

### Goodhart's Law in LLM evaluation

*"When a measure becomes a target, it ceases to be a good measure."*

As soon as MMLU became the standard metric, labs began:
- Training on MMLU-format data
- Including questions similar to MMLU in SFT/RLHF pipelines
- Optimizing for the 4-choice format specifically

The result: MMLU scores improved dramatically, but gains on MMLU don't transfer to other measures of breadth of knowledge. The benchmark was teaching to the test.

This pattern repeats for every benchmark that becomes a standard.

### Data contamination

If questions from a benchmark appeared in the training data, the model may have memorized answers rather than reasoning to them. This is called **contamination**.

Signs of contamination:
- Model scores much higher on a benchmark than on equivalent problems not in the benchmark
- Model generates the exact wording from benchmark answer keys
- Performance on a benchmark drops sharply on newly-released questions (LiveBench pattern)

Contamination is hard to prevent at scale — the internet contains most things, and cleaning it is impractical. Most frontier model evaluations include contamination disclaimers.

### LLM-as-judge biases

Using GPT-4 or Claude to evaluate outputs is increasingly common (Chatbot Arena, AlpacaEval). Known biases:

- **Verbosity bias:** Longer responses are rated higher, independent of quality
- **Self-preference bias:** GPT-4 rates GPT-4-like responses higher; Claude rates Claude-like responses higher
- **Position bias:** In pairwise comparison (which is better, A or B?), the first option is preferred
- **Style bias:** Confident, well-structured responses rate higher even if the content is wrong

Mitigation: randomize order, use multiple judges, include blind human spot-checks.

### Chatbot Arena (LMSYS)

The best available proxy for real user preference: users interact with two anonymous models simultaneously, choose which is better, scores are calculated via Elo rating.

- Based on millions of human preference votes
- Resistant to gaming (you don't know which model you're optimizing for)
- Not controlled by any single lab
- Limitation: biased toward conversational tasks, English-speakers, free/low-stakes usage

Currently the most trusted signal for overall model quality.

### Building your own eval

For production applications, build a **task-specific eval** against your actual use case:

1. Collect 50–200 representative inputs from your real users
2. Define success criteria (exact match, partial credit, rubric, binary pass/fail)
3. Create a ground truth for each input (human labels or deterministic checks)
4. Run candidate models, score outputs against ground truth
5. Automate and run on every model change

This beats any general benchmark for predicting performance in your application.

---

## Why It Matters

**Don't choose models based on headline benchmark scores.** Always evaluate on your specific task with representative inputs. A model that scores 5% higher on MMLU may perform worse on your customer support task.

**Evals are a forcing function for specification.** The hardest part of building an eval is defining what "good" means for your use case. This is valuable independent of evaluation — it forces you to clarify requirements.

**Regression testing is as important as capability testing.** Every time you update your prompt, system prompt, or model version, run your eval to check for regressions. Improvements on one metric often degrade another.

---

## Common Misconceptions

**"Higher benchmark score = better in practice."** Only if the benchmark is well-matched to your task and not contaminated. Almost always true at the very bottom of the scale; unreliable at the top.

**"Perplexity is a good quality signal."** For the specific task of next-token prediction on similar text, yes. For everything else, no. A model can have very low perplexity and hallucinate constantly.

**"Human evaluation is always best."** Human evaluation has its own problems: inter-rater disagreement, evaluator fatigue, cultural and linguistic biases, consistency across sessions. It's expensive and slow, and "what raters prefer" may not match "what actually helps users."

**"If models pass my eval, they're production-ready."** Evals cover the inputs you thought to include. Production surfaces inputs you didn't anticipate. Monitoring production behavior and maintaining a live eval set is necessary for any serious deployment.

---

## Active Recall

1. Name three things a benchmark can measure and three things it can't. Which category matters most for production deployment?

2. What is data contamination in the context of benchmarks, and why is it hard to prevent?

3. What is Goodhart's Law and how does it apply to LLM evaluation?

4. You're deploying an LLM for legal document review. Would you trust MMLU or a custom eval you built from 100 real client documents? Why?

5. LLM-as-judge is used instead of human evaluation in your pipeline. Name two biases you'd need to control for and how.

---

## Optional Deep Dive: Evaluation Metrics

### Pass@k for code generation

For code benchmarks, a single sample may be right or wrong by luck. Pass@k measures: "what's the probability of at least one correct solution in $k$ samples?"

$$\text{pass@}k = 1 - \frac{\binom{n-c}{k}}{\binom{n}{k}}$$

Where $n$ = total samples, $c$ = correct samples.

Typical reporting: pass@1 (single shot), pass@10 (10 attempts, take best). The latter is much higher for all models — which is why reporting matters.

### Bradley-Terry model (Elo rating)

Chatbot Arena uses Bradley-Terry, a probabilistic model for ranking:

$$P(A \text{ beats } B) = \frac{e^{\beta_A}}{e^{\beta_A} + e^{\beta_B}}$$

Where $\beta$ is the model's strength parameter. This is similar to chess Elo but adapted for the case where many items are compared and each comparison is noisy.

### G-Eval and reference-free evaluation

Traditional NLP metrics (BLEU, ROUGE) measure similarity to a reference answer — bad for open-ended generation where there are many valid responses. G-Eval (Liu et al., 2023) uses chain-of-thought LLM evaluation:

1. Give the model a rubric with criteria (fluency, coherence, consistency, relevance)
2. Ask it to evaluate each criterion step-by-step
3. Aggregate into an overall score

Correlates better with human judgment than BLEU/ROUGE across most tasks, but inherits LLM biases.
