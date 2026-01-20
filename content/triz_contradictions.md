# TRIZ Analysis: Green LLM Contradictions

This document defines the core technical and physical contradictions identified in the quest for ethical, low-impact LLM usage, applying TRIZ (Theory of Inventive Problem Solving) principles to move toward Phase 3 (Ideation).

---

## 1. Contradiction: Performance vs. Energy

### The "Small Model" Fallacy
A common suggestion for reducing energy is simply "using a smaller model" (e.g., Llama-3-8B instead of GPT-4). However, this is not a complete solution due to:
*   **Accuracy Loss:** Smaller models often fail on complex reasoning tasks, leading to "wasted" energy on incorrect or hallucinated outputs.
*   **The Recursive Overhead:** Solving a complex task with a small model often requires multiple "loops" or agentic retries, potentially consuming more total energy/tokens than a single high-precision call to a large model.
*   **Human Intervention:** Errors by smaller models require human correction, which is an external "energy cost" (cognitive load and system uptime).

### Technical Contradiction (Altshuller's Matrix)
*   **Improving Parameter:** *Accuracy of Measurement / Reliability* (The system must correctly solve the user's problem).
*   **Worsening Parameter:** *Waste of Energy / Productivity* (Improving accuracy typically requires more parameters, more FLOPS, and higher power draw).

### Technical Contradiction (Simplified)
> If we increase the model size to improve reasoning accuracy, then energy consumption increases.
> If we decrease the model size to save energy, then reasoning accuracy and reliability decrease.

---

## 2. Contradiction: Utility vs. Ethics

### The Data Provenance Gap
The most "capable" models (e.g., GPT-4o, Claude 3.5 Sonnet) are generally trained on massive, non-consensual datasets (Common Crawl, Books3, etc.).
*   **High Utility:** Derived from the richness of "the whole internet" (proprietary code, copyrighted literature).
*   **Ethical Debt:** Violates the principle of "ethical provenance" and consensual data usage.

### Technical Contradiction
*   **Improving Parameter:** *Adaptability / Versatility* (Model can handle any query, from creative writing to niche coding).
*   **Worsening Parameter:** *Harmful Side Effects* (Legal risk, ethical violation, exploitation of creators).

### Technical Contradiction (Simplified)
> If we use models trained on all available data to maximize utility, then ethical provenance is sacrificed.
> If we use models trained only on ethical/consensual data, then the model’s versatility and reasoning depth (Utility) decrease.

---

## 3. Applying TRIZ Inventive Principles

Based on the expert findings in `content/expert_audit.md` and `content/ethical_datasets.md`, we can map potential solutions to TRIZ Inventive Principles:

### Principle 1: Segmentation
*   **Application:** Break a monolithic task into a **Multi-Agent Swarm** (Cohen).
*   **Resolution:** Instead of one large model doing everything, use several specialized, tiny models. This resolves the Performance vs. Energy contradiction by optimizing the "energy-per-subtask."

### Principle 10: Preliminary Action
*   **Application:** **RAG (Retrieval-Augmented Generation)**.
*   **Resolution:** Perform the "heavy lifting" of data retrieval using traditional, low-energy vector search *before* the LLM is called. This keeps the LLM's context window small and focused, reducing token consumption.

### Principle 35: Parameter Change
*   **Application:** **Model Pruning and Quantization** (Burggraaf).
*   **Resolution:** Change the "physical state" of the model (from FP16 to INT4). This drastically reduces energy/memory requirements while maintaining most of the reasoning capability.

### Principle 22: "Blessing in Disguise" (Turn Harm into Benefit)
*   **Application:** **Synthetic Data Generation** (PleIAs SYNTH).
*   **Resolution:** Use a large, "unethical" model to generate high-quality instruction-tuning data once, then use that data to train a smaller, "ethical" model. The "harm" of the initial training is used to create a permanent, clean asset.

### Principle 13: The "Other Way Round"
*   **Application:** **Compiled Workflows / Deterministic Logic**.
*   **Resolution:** Instead of asking an LLM to "think" through a process every time (ReAct), use the LLM once to *generate code* that solves the problem deterministically. Move the reasoning from the energy-heavy LLM to energy-light traditional code.

---

## 4. Synthesis for Phase 3 (Ideation)

The "Ideal Final Result" (IFR) in TRIZ is a system that provides the benefit without the cost or harm. For this project, the IFR is:
> **An agentic system that provides high-accuracy solutions using zero-carbon energy and 100% ethically sourced data, where the "model" itself is as small as possible.**

**Key Ideation Vectors:**
1.  **Local-First Agentics:** How can we run 1B-3B models locally (zero cloud energy) using RAG to bridge the utility gap?
2.  **The "Ethical Distillation" Pipeline:** How to use the best models to "teach" ethical, public-domain models without permanent dependency?
3.  **Measurement-Driven Orchestration:** Integrating the GSF Impact Framework to automatically choose the "greenest" model for a specific task.
