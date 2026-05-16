# Module 05: Alignment — SFT, RLHF, DPO

**The one-sentence version:** Alignment is the process of transforming a base model that predicts tokens into an assistant that follows instructions helpfully, honestly, and safely — through a sequence of training stages that shape behavior without erasing pretraining knowledge.

---

## Mental Model

**A base model is a brilliant researcher who's never had a conversation. Alignment is social training.**

The researcher knows everything — history, science, code, how to make weapons, how to manipulate people, everything they ever read. But they've never had a job, never been told "be helpful," never internalized "don't do harm."

Alignment is essentially: hiring coaches (human raters), showing the researcher examples of ideal conversations, and then optimizing their behavior against human preferences. The content knowledge stays intact — you're shaping *how* they respond, not *what* they know.

---

## Core Concept

### Stage 0: Base model

Output of pretraining. Capable of coherent text completion. Dangerous in direct deployment:
- Will complete harmful prompts without hesitation
- Has no concept of "I don't know" — will hallucinate confidently
- Can't maintain conversation structure (no notion of user/assistant turns)
- Won't follow instructions — it predicts likely continuations, not responses

### Stage 1: Supervised Fine-Tuning (SFT)

Human contractors write ideal examples of conversations:
```
User: Explain quantum entanglement to a 10-year-old.
Assistant: Imagine you have two magic dice that are connected no matter how far apart they are...
```

The model is fine-tuned on thousands of these demonstrations. It learns:
- Conversation format (user/assistant turns)
- The tone and style of a helpful assistant
- Basic instruction-following patterns
- How to refuse harmful requests

**SFT teaches format and style, not values.** The model learns "respond like this" from examples, but it can still be jailbroken or produce harmful outputs — it's imitating good behavior, not internalizing why it's good.

### Stage 2: Reward Model Training

For RLHF to work, you need a way to automatically score model outputs. Human raters are too slow and expensive for every gradient step.

The solution: train a **reward model** — a separate neural network that predicts how much a human would prefer an output.

Process:
1. Give the base SFT model a prompt → get 4-8 candidate responses
2. Show the responses to human raters, who rank them (A > B > C > D)
3. Train the reward model to assign higher scores to preferred responses
4. Repeat until the reward model reliably mimics human preference

The reward model is now a fast, automatic proxy for human judgment.

### Stage 3: RLHF — Reinforcement Learning from Human Feedback

Now use the reward model as a signal to train the policy (the LLM):

```
Prompt → LLM generates response → Reward model scores it → Update LLM to maximize score
```

This is reinforcement learning where the "environment" is the reward model and the "action" is generating a response.

**The critical constraint:** You must also penalize the LLM for drifting too far from the SFT model. Without this KL penalty, the model "reward hacks" — it finds responses the reward model gives high scores to that humans would actually hate. The reward model is imperfect, and RL is very good at exploiting imperfections.

```
Objective = E[reward(response)] - β × KL(LLM || SFT_model)
```

### Stage 4 (alternative): DPO — Direct Preference Optimization

RLHF is complex: you need to train a separate reward model, manage RL instability, tune the KL coefficient. DPO (Rafailov et al., 2023) simplifies this:

**DPO insight:** The optimal RLHF policy has a closed-form solution. Instead of training a reward model and then doing RL, you can directly optimize the LLM on preference pairs using a classification-style loss.

```
Given: (prompt, preferred_response, rejected_response)
DPO directly increases P(preferred) and decreases P(rejected) in one step.
```

DPO produces similar quality to RLHF with:
- No separate reward model
- No RL training loop
- More stable training
- Simpler implementation

Most post-2023 open-source models (Llama 2 Chat, Mistral Instruct) used DPO or variants of it.

### The alignment tax

Alignment can slightly reduce raw benchmark performance — the model trades some "raw capability" for better behavior. This is called the alignment tax. In practice:

- For most practical tasks, aligned models are more useful than base models
- The tax is usually small (<5% on most benchmarks)
- Some capabilities (creativity, controversial analysis) may be more constrained

### Constitutional AI (Anthropic's approach)

Claude uses a variant of RLHF called **Constitutional AI (CAI)**:

1. Define a "constitution" — a set of principles the model should follow
2. Ask the model to *critique its own outputs* against the constitution
3. Ask the model to *revise* its outputs based on the critique
4. Use AI-generated preferences (not only human) to train the reward model

This reduces the volume of human labeling required and produces a more principled alignment target rather than optimizing for annotator preferences which may be inconsistent.

---

## Why It Matters

**SFT, RLHF, and DPO explain why two models with the same weights can behave so differently.** Base Llama 3 and Llama 3 Instruct are the same architecture with the same pretraining — alignment is what separates them.

**Alignment failures are exploitable.** Jailbreaks work by navigating the alignment layer — finding prompts that pattern-match to "safe" training examples while extracting harmful content. This is why alignment is an active research area, not a solved problem.

**The reward model is a fragile proxy.** "Teaching to the test" applies to LLMs. A model optimized heavily against a reward model will find ways to score well that don't correspond to actual quality. This is called reward hacking or Goodhart's Law.

**Alignment doesn't erase knowledge.** A model that refuses to explain how to make chemical weapons still has that information — it's in the weights from pretraining. Alignment adjusts behavior, not knowledge. This has safety implications.

---

## Common Misconceptions

**"Alignment means censorship."** No. Alignment means shaping behavior toward being helpful, honest, and safe. A well-aligned model can discuss difficult topics, explain how diseases spread, write villain dialogue — it refuses sustained attempts to cause real-world harm, not anything sensitive.

**"RLHF trains on human scores."** Not directly — it trains on a reward model's scores, which is a learned approximation of human preference. The quality of RLHF is bottlenecked by the quality of the reward model, not just the human raters.

**"More alignment = more capable."** Not necessarily. Alignment shapes behavior — capability comes from pretraining and scale. An aligned 7B model is safer than a base 7B model but not more capable at reasoning.

**"Base models are useless."** For controlled generation tasks (story completion, text infilling, specialized domains), base models are often better — no alignment tax, no refusals, more creative. Many production systems use base models with careful prompt engineering rather than instruction-tuned models.

---

## Active Recall

1. What specific problem does SFT solve that pretraining doesn't? What does it fail to solve?

2. Why do you need a reward model in RLHF instead of using human ratings directly?

3. What is reward hacking, and why does the KL penalty prevent it?

4. What makes DPO simpler than RLHF? What does it use in place of the reward model?

5. If you want a model that's very creative and unconstrained in its outputs (e.g., for fiction writing), would you prefer a base model or an instruction-tuned model? Why?

---

## Optional Deep Dive: The Math

### SFT loss

Standard cross-entropy on demonstration data. Given a conversation $(x, y)$ where $x$ is the prompt and $y$ is the ideal response:

$$\mathcal{L}_{\text{SFT}} = -\sum_{t=1}^{|y|} \log P_\theta(y_t \mid x, y_{<t})$$

Only the response tokens contribute to the loss — the prompt is "free" (the model sees it but isn't penalized on it).

### RLHF objective

$$\mathcal{J}(\theta) = \mathbb{E}_{x \sim \mathcal{D}, y \sim \pi_\theta(x)}\left[r_\phi(x, y) - \beta \log \frac{\pi_\theta(y \mid x)}{\pi_{\text{SFT}}(y \mid x)}\right]$$

Where:
- $r_\phi(x, y)$ = reward model score
- $\beta \log \frac{\pi_\theta}{\pi_\text{SFT}}$ = KL divergence from SFT model (penalizes drift)
- $\beta$ = controls the tradeoff

### DPO loss

Given a preference pair $(y_w, y_l)$ — preferred ("won") and rejected ("lost"):

$$\mathcal{L}_{\text{DPO}} = -\log \sigma\!\left(\beta \log \frac{\pi_\theta(y_w \mid x)}{\pi_{\text{ref}}(y_w \mid x)} - \beta \log \frac{\pi_\theta(y_l \mid x)}{\pi_{\text{ref}}(y_l \mid x)}\right)$$

Where $\pi_{\text{ref}}$ is the SFT reference model and $\sigma$ is the sigmoid function.

Interpretation: maximize the log-ratio of preferred to rejected, relative to the reference model. The reference model prevents reward hacking without needing explicit RL.
