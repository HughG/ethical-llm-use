# LLM Impact Metrics & Analysis

This document outlines the baseline environmental "costs" of Large Language Model (LLM) operations, covering energy, water, and hardware efficiency. Data is synthesized from recent research (2024-2025) and provider disclosures.

## 1. Baseline Energy Consumption (Task 1)

Energy consumption scales primarily with model parameter count and token throughput.

### Energy Cost per 1000 Tokens (Inference)
*Based on benchmarks from TokenPowerBench (2025) and Luccioni et al. (2024).*

| Model Size | Energy (kWh) | Context/Notes |
| :--- | :--- | :--- |
| **Small (7B - 8B)** | 0.005 – 0.01 | Highly efficient for local RAG and basic tasks. |
| **Medium (70B - 80B)** | 0.045 – 0.08 | The "sweet spot" for complex reasoning on high-end local hardware. |
| **Frontier (400B+)** | 0.25 – 0.50+ | Estimated for GPT-4/Llama 3.1 405B class models in cloud environments. |

### Carbon Intensity Equivalents
*   **Average GPT-4 Query:** ~3 grams of CO2e (equivalent to a 60W lightbulb running for 3 minutes).
*   **Image Generation (DALL-E 3/Midjourney):** ~0.1 - 0.5 kWh per image (orders of magnitude higher than text).

---

## 2. Hardware Efficiency: Local vs. Cloud

The choice of hosting environment significantly impacts "idle" waste and total energy footprint.

### Idle Power Draw (Baseline)
| System | Idle Power (Watts) | Annual "Vampire" Cost (Idle 24/7) |
| :--- | :--- | :--- |
| **Apple M4 Max (Mac Studio)** | ~6 W | ~52 kWh |
| **Apple M3 Ultra (Mac Studio)** | ~9 W | ~79 kWh |
| **PC + NVIDIA RTX 4090/5090** | 30 – 50 W | 260 – 438 kWh |
| **Cloud Server (A100/H100)** | 150 – 300 W | 1,300 – 2,600+ kWh |

**Key Finding:** Apple Silicon provides a ~5x–8x improvement in idle efficiency compared to high-end NVIDIA workstations. However, cloud systems benefit from higher **Utilization Rates**, where the high idle cost is amortized over thousands of concurrent users.

### Cloud Overhead (PUE)
Data centers typically have a **Power Usage Effectiveness (PUE)** of 1.1 to 1.5. This means for every 1 kWh used by the GPU, an additional 0.1 to 0.5 kWh is consumed by cooling and power delivery. Local "home office" cooling (HVAC) often exceeds this overhead in summer months but is ignored in most local-only metrics.

---

## 3. Water Usage Metrics

Water consumption occurs primarily through data center cooling (evaporative cooling).

*   **Standard Query:** ~10ml to 50ml per exchange.
*   **The "Bottle" Myth:** Sensationalist reports claim "500ml per prompt" (a small water bottle). Recent 2025 research (Laughing et al.) clarifies that while this may happen in inefficient, legacy facilities during peak heat, modern **closed-loop cooling** reduces this to near-zero for the query itself, though global averages remain around **20ml per 50 tokens**.
*   **Geography Matters:** A query processed in a water-stressed region (e.g., Arizona) has a much higher ethical "cost" than one in a region with abundant hydroelectric cooling (e.g., Norway).

---

## 4. Tailpipe.ai Deep Dive (Task 3)

**Tailpipe.ai** is an emerging leader in usage-based carbon accounting for AI.

### Methodology
*   **Framework:** Built on **ISO 21031:2024** and the Green Software Foundation’s **SCI for AI (Software Carbon Intensity)** specification.
*   **Usage vs. Spend:** Unlike traditional "Spend-Based" accounting (e.g., "$100 spent = X tons"), Tailpipe uses **telemetry-based** modeling. They ingest real GPU/CPU utilization data from cloud providers (AWS/Azure/GCP).
*   **Embodied Emissions:** Includes the carbon cost of manufacturing the H100s and servers, amortized over the hardware's lifespan.

### Transparency & Reliability
*   **Data Quality Dashboard:** Tailpipe provides a transparency score for every metric, distinguishing between "Direct Telemetry" (High Confidence) and "Architecture-Based Estimates" (Medium Confidence).
*   **Integration:**
    1.  **API:** Developers can query the `/emissions` endpoint with a model name and token count to get real-time CO2e estimates.
    2.  **Decision Support:** Provides "Location Shifting" recommendations (e.g., "Moving this workload from `us-east-1` to `ca-central-1` would reduce carbon by 70%").

---

## 5. Synthesis: Baseline "Costs" for Decision Making

When evaluating LLM usage, developers should use the following "Rule of Thumb" for an ethically-grounded baseline:

1.  **The 10:1 Rule:** A 70B model uses roughly 10x the energy of a 7B model. Use the smallest model that satisfies the task.
2.  **The Locality Trade-off:** Use **Local (Apple Silicon)** for development and low-concurrency tasks to eliminate cloud PUE overhead and data transit costs.
3.  **The Time-Shift:** If using the cloud, batch non-urgent tasks for "Green Windows" when the local grid's renewable mix is highest (utilizing tools like Tailpipe.ai or CarbonAware SDK).
