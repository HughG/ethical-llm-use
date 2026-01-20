# Framework 16 (Ryzen 7840HS) LLM Optimization Guide

This guide outlines Phase 3, Tasks 2 & 3 of the ethical LLM research: **Segmented Approach & Resource Analysis**. It is specifically tailored for the Framework 16 laptop equipped with the AMD Ryzen 7 7840HS processor.

---

## 1. Segmented Approach (Task 2)

Efficient LLM usage requires matching task complexity to model size. For the Framework 16, models are segmented into three tiers to optimize performance per watt.

### A. Model Tiers
| Tier | Size | Recommended Models | Primary Tasks |
| :--- | :--- | :--- | :--- |
| **Ultra-Small** | 135M – 3B | SmolLM2-1.7B, Gemma-2B, Llama-3.2-1B | Intent routing, spell-check, basic summarization, local file indexing. |
| **Small (Local)** | 7B – 9B | OLMo-7B-Instruct, Mistral-7B-v0.3, Llama-3.1-8B | Complex coding assistance, long-form drafting, structured data extraction. |
| **Large (Cloud)** | 70B+ | Claude 3.5 Sonnet, GPT-4o, Llama-3-70B (Cloud) | Multi-step reasoning, high-stakes verification, final polish of critical documents. |

### B. Ethical & Open Training Models
For this setup, prioritize models with high transparency:
*   **OLMo-7B (AI2):** Fully open weights, data (Dolma), and training code. Ideal for "auditable" local research.
*   **PleIAs Common Corpus:** Models trained on the *Common Corpus* (2T+ tokens of public domain/permissively licensed data). This addresses the "ethical sourcing" mandate.
*   **SmolLM2 (HuggingFace):** State-of-the-art efficiency with transparent data curation for on-device use.

### C. The "Chain of Efficiency" Workflow
Instead of invoking a cloud model immediately, the Framework 16 uses a tiered routing logic:
1.  **Router (Llama-3.2-1B):** Analyzes query intent. 
    *   *Simple?* Handle locally on NPU.
    *   *Technical/Creative?* Pass to 7B iGPU.
    *   *Extreme Reasoning?* Flag for Cloud.
2.  **Executor (OLMo-7B):** Performs the heavy lifting locally on the Radeon 780M iGPU.
3.  **Verifier (Cloud - Optional):** Only used if the 7B output fails local unit tests or requires an "expert audit."

---

## 2. Resource Analysis (Task 3)

The Ryzen 7840HS provides a heterogeneous architecture that allows for "waste" utilization—running tasks on low-power silicon during periods of low activity.

### A. Hardware Utilization Strategy
*   **Ryzen AI NPU (Neural Processing Unit):** 
    *   **Optimization:** Use for background tasks (e.g., local RAG embedding, semantic search indexing).
    *   **Advantage:** Designed for high efficiency (low TOPS/Watt). Keeping the NPU busy for background "agent" tasks (via ONNX/OpenVINO) avoids spiking the main CPU/iGPU power draw.
*   **Radeon 780M iGPU:**
    *   **Optimization:** Use for active LLM generation. 12 RDNA3 cores provide ~10–12 tokens/sec for 7B models.
    *   **Advantage:** significantly faster than CPU-only inference while sharing system memory (up to 32GB+ depending on Framework configuration).

### B. Scheduling & "Renewable-Peak" Execution
To minimize the carbon footprint of local compute:
*   **Off-Peak Batching:** Use `Compute Gardener` or `Carbon Aware SDK` to trigger non-urgent tasks (like document summarization or code linting) when the local power grid's carbon intensity is lowest.
*   **Thermal/Power Throttling:** Under Linux, use `Energy Aware Scheduling (EAS)` to pin background LLM workers to "efficiency" cores or the NPU, ensuring thermals stay low and fan noise is minimized.

### C. The Hybrid "Burst" Model
*   **Local (80% of tasks):** Privacy-first, zero-cost, carbon-minimized.
*   **Cloud Burst (20% of tasks):** Triggered only when:
    *   Context exceeds local VRAM (32GB+).
    *   Intelligence threshold is not met by 8B models.
    *   Local energy is currently high-carbon (e.g., during peak evening hours on a coal-heavy grid) and cloud regions with 100% renewable energy are available.

---

## 3. Decision Matrix: Framework 16 Hybrid User

| Task Type | Complexity | Model Size | Execution Location | Energy Source |
| :--- | :--- | :--- | :--- | :--- |
| Email Drafting | Low | 1.7B | **Local NPU** | Any (Low Draw) |
| Feature Implementation | Medium | 7B/8B | **Local iGPU** | Solar/Off-peak |
| Security Audit | High | 70B+ | **Cloud (Renewable)** | Verified Green DC |
| File Indexing | Background | 135M-1B | **Local NPU** | "Waste" Power |
| Research Synthesis | High | 8B -> 70B | **Local -> Cloud** | Hybrid |

---

## 4. Implementation Recommendations
1.  **Software Stack:** Use **Ollama** with `ROCm` (Linux) or **Ryzen AI Software** (Windows) for iGPU/NPU acceleration.
2.  **Automation:** Implement a local script using the `Carbon Aware SDK` to delay heavy local fine-tuning or batch inference until "Green" windows.
3.  **Quantization:** Use `Q4_K_M` or `IQ4_XS` GGUF formats to balance 7B model quality with the 7840HS's memory bandwidth limits.
