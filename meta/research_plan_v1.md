# Research Plan: First Pass

This plan outlines the initial research phase focusing on identifying ethical and sustainable LLM usage patterns.

HughG NOTE: As research progresses, this plan and the resources.md may be freely edited and expanded upon.

## Phase 1: Landscape Mapping & Resource Expansion
- [x] **Expert Audit:** Review publications and projects from identified people (Burggraaf, Cohen, Masraff) for specific green software/agentic techniques.
- [x] **Dataset Provenance:** Identify models trained on explicitly licensed or public domain data (e.g., Common Corpus, models from Hugging Face with "Open Data" focus).
- [x] **Open Training Research:** Deep dive into "Open Training Data" models that publish full data mixtures and curation recipes (e.g., OLMo, Pythia, DCLM-Baseline) to verify reproducibility.
- [x] **Infrastructure Scanning:** Catalog energy-efficient inference providers (e.g., those using 100% renewable energy or innovative cooling) and local execution tools (Ollama, LM Studio).

## Phase 2: Technical & Ethical Baseline
- [x] **Impact Metrics:** Establish baseline energy consumption figures for common operations (Token generation costs, idle power of local vs. cloud).
- [x] **Contradiction Definition (TRIZ):** Explicitly document the "Performance vs. Energy" and "Utility vs. Ethics" contradictions.
- [x] **Tailpipe.ai Analysis:** Deep dive into tailpipe.ai to understand their carbon tracking methodology for AI.

## Phase 3: Problem Solving & Ideation
- [x] **Ideal Final Result (IFR) Modeling:** Describe what the "Zero-Impact LLM Assistant" looks like based on current tech.
- [x] **Segmented Approach:** Explore if small, specialized models (e.g., Mistral-7B, Phi-3) can replace large models for specific "life admin" tasks.
- [x] **Resource Analysis:** Identify "waste" or "hidden" resources like underutilized local GPUs or off-peak electricity windows.

## Phase 4: Initial Synthesis
- [ ] **"Green Stack" Draft:** Create a preliminary recommendation for a hardware/software stack for ethical dev work.
- [ ] **Life Admin Pilot Design:** Define a simple test case for booking a holiday using the most sustainable tools identified.
