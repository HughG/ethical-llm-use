# Dataset Provenance: Ethical and Transparent LLM Curation

This document catalogs LLM datasets and models with high standards for data provenance, focusing on explicitly licensed, public domain, and transparently curated data as of early 2026.

## 1. Catalog of Ethical and Transparent Datasets

| Dataset | Provider | Size | License/Provenance | Focus |
| :--- | :--- | :--- | :--- | :--- |
| **Common Corpus** | PleIAs / Etalab | ~2 Trillion Tokens | Public Domain (CC0 / Expired Copyright) | Multilingual, cultural heritage, government, and science. |
| **FineWeb / FineWeb-Edu** | Hugging Face | 15+ Trillion Tokens | Common Crawl (Curated/Transparent) | High-quality web data with rigorous filtering and deduplication. |
| **RedPajama-v2** | Together AI | 30 Trillion Tokens | Open (Apache 2.0) / Transparent | Focus on quality signals and community-driven weighting. |
| **Common Pile v0.1** | Open Community | 233M Documents | Openly Licensed / Permissive | Research articles, code, and legal texts from 30+ sources. |
| **DCLM-Baseline** | DataComp (HF/Apple/etc) | Varied | Transparent Research-first | Systematic dataset evaluation and open-source curation recipes. |

### Notable Provenance Issues & Resolutions
*   **The Pile (v1/v2):** Historically faced challenges due to the inclusion of `Books3` (shadow library data). Modern iterations focus on filtering out "shadow" sources in favor of Project Gutenberg and licensed publisher partnerships.
*   **Public Domain Expansion:** The Common Corpus project leverages the expiration of copyright (e.g., in the US and Europe) to digitize and tokenize hundreds of billions of words from 19th and early 20th-century literature.
*   **Open Training Data Research Point:** Distinguish between models that are "Open Weights" (code and weights available) vs. "Open Training" (full data mixtures, filtering logs, and curation recipes published). Future research will focus on models like **OLMo** and **DCLM-Baseline** that allow for full auditability of what the model has "read."

## 2. Models Trained on Ethical Datasets

Several "Sovereign AI" initiatives now prioritize legal compliance (EU AI Act) and ethical sourcing.

### PleIAs Model Family
*   **PleIAs 1.0 (350M, 1.2B, 3B):** Base models trained exclusively on Common Corpus data.
*   **PleIAs-RAG (Pico/Nano):** Specialized reasoning models optimized for Retrieval-Augmented Generation.
    *   **Performance:** The PleIAs-nano-1.2B-RAG model reportedly matches or exceeds larger models (e.g., Llama-3.2-3B) on multilingual RAG benchmarks.
*   **Baguettotron:** A sovereign French model focused on administrative and cultural transparency.

### Open-Data Benchmarking
*   **DCLM Models:** Models trained as part of the DataComp for LLMs initiative serve as transparent baselines for performance comparisons, showing that better filtering can compensate for smaller dataset sizes.

## 3. TRIZ Analysis: The "Utility vs. Ethics" Contradiction

**Contradiction:** To achieve high reasoning capabilities ("Utility"), models traditionally require the high-density information found in copyrighted books, news, and proprietary code ("Pirated Data"). However, "Ethical" models often rely on public domain or government data, which can be repetitive or lower in informational density.

### Observations on the Gap
*   **Performance Lag:** While "Ethical" models are highly competitive in specialized domains (RAG, administrative tasks, legal analysis), they often lag in general-purpose creative writing or broad cultural nuances found only in contemporary copyrighted works.
*   **Bridging the Gap:**
    *   **Synthetic Data (The "Synths"):** Projects like **PleIAs SYNTH** use "clean" models to generate high-quality reasoning traces and instructions, bridging the gap between historical public domain data and modern reasoning needs.
    *   **Instruction Tuning:** Focusing on high-quality instruction datasets (which are often more easily licensed or created with consent) allows smaller "ethical" base models to over-perform on utility benchmarks.
    *   **Domain Specificity:** The transition from "General Purpose" to "Task Specific" models reduces the need for the "entire internet" of data, making ethical sourcing more feasible.

## 4. Summary of Evidence
As of 2026, the release of 2T+ token public domain datasets like **Common Corpus** has proved that it is possible to build competitive, legally compliant LLMs without relying on "murky" data provenance. The focus has shifted from "Max Scale" to "Max Transparency," driven by the regulatory requirements of the EU AI Act and the desire for sovereign, trustworthy AI systems.
