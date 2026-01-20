# Phase 1, Task 3: Infrastructure Scanning Report

This report catalogs and evaluates the current state of "Green AI" infrastructure, comparing cloud providers, local execution tools, and hardware efficiency for sustainable LLM usage.

## 1. Catalog of Energy-Efficient Cloud Providers

Cloud-based inference is often the most accessible path for large models, but its environmental impact depends heavily on data center PUE (Power Usage Effectiveness) and the carbon intensity of the local energy grid.

### Specialized "Green AI" Clouds
*   **Green AI Cloud (EU/Sweden)**: 
    *   **Sustainability Focus**: Claims CO2-negative operations. Operates in northern regions to utilize natural cooling and 100% renewable energy.
    *   **Innovation**: Uses heat-recovery systems to feed data center waste heat back into local district heating grids.
    *   **Hardware**: Offers NVIDIA B200 and A100 instances.
*   **Exoscale (Switzerland/Germany/Austria)**:
    *   **Sustainability Focus**: Powered by 100% renewable energy. Part of the A1 Group, with transparent ESG reporting since 2021.
    *   **Sovereignty**: European-owned, ensuring data residency and lower regulatory hurdles for ethical research.

### Decentralized Compute Providers
*   **Akash Network**:
    *   **Green Potential**: Operates as a "DePIN" (Decentralized Physical Infrastructure Network). By utilizing existing, underused GPU capacity across the globe, it minimizes the "embodied carbon" of building new data centers.
    *   **Verifiability**: While not inherently "green," specific providers on the network can be tagged or filtered for renewable energy usage.
*   **Gensyn**:
    *   **Focus**: Verifiable deep learning. While still in early stages, it aims to democratize compute by allowing edge devices to contribute to training and inference, potentially utilizing residential solar surpluses.

## 2. Evaluation of Local Execution Tools

Local inference eliminates cloud latency and data transfer energy but shifts the burden to consumer hardware.

| Tool | Backend | Key Efficiency Feature |
| :--- | :--- | :--- |
| **MLX** | Apple MLX | Native optimization for Apple Silicon Unified Memory; extremely high tokens-per-watt. |
| **Llama.cpp** | C++ (Custom) | Minimal dependencies, highly portable, supports 4-bit and 2-bit quantization (GGUF) to save memory/power. |
| **Ollama** | Llama.cpp/vLLM | Ease of use. Wrapper for llama.cpp; overhead is minimal, but background services can idle with high RAM usage. |
| **LM Studio** | MLX/Llama.cpp | GUI-based. Best for rapid testing; supports multi-model loading with shared weights to reduce VRAM swaps. |

### Carbon-Aware Scheduling for Local Tasks
To minimize operational carbon, tasks should be "time-shifted" to coincide with high renewable energy availability in the local grid.
*   **Grid Intensity CLI**: A tool from the Green Web Foundation to check real-time grid carbon intensity.
*   **snag**: A Python-based scheduler that uses the UK National Grid Carbon Intensity API (and others) to run tasks when the grid is "cleanest."
*   **EcoServe (Research)**: An emerging framework for carbon-aware inference that dynamically adjusts model size or scheduling based on energy signals.

## 3. Hardware Efficiency Benchmarks

*   **Apple M-Series (M3/M4 Max)**:
    *   **Efficiency**: Leads in "Intelligence per Watt." The SoC (System on a Chip) design reduces energy loss between CPU/GPU and Memory.
    *   **Sweet Spot**: 7B to 30B parameter models using 4-bit quantization.
*   **NVIDIA RTX 4090**:
    *   **Efficiency**: Lower than Apple M-series per token at low batch sizes, but highly efficient for high-throughput (multiple requests) due to massive CUDA core density.
    *   **Caveat**: High idle power draw (~30W-50W) and peak draw (450W).
*   **NPUs (Neural Processing Units)**:
    *   **Efficiency**: Designed specifically for low-power AI (e.g., Snapdragon X Elite).
    *   **Current State**: Excellent for always-on background tasks (STT, TTS) but currently limited by low VRAM for large LLM research.

## 4. User Profile: The "Framework Hybrid" Use Case

This section documents the specific hardware constraints and hybrid strategy for the primary user.

### Current System Status
*   **Operating System**: Windows 11.
*   **Discrete GPU (dGPU)**: **NONE** (currently absent).
*   **Future Upgrade Path**: NVIDIA GeForce RTX 5070 (planned/potential).

### Hardware Specification: Framework 16 Laptop
*   **CPU**: AMD Ryzen 7840HS (8 cores, 16 threads).
*   **Integrated Graphics (iGPU)**: Radeon 780M (RDNA 3) - **Primary Local Engine**.
*   **Significance**: On Windows, the Ryzen 7840HS includes an **AMD Ryzen AI (NPU)**, offering a low-power path for background agent tasks via **ONNX Runtime** or **OpenVINO**. The Radeon 780M iGPU is the primary accelerator for LLM inference.

### Hybrid Inference Strategy
To balance the "Utility vs. Energy" contradiction, a hybrid model is proposed:
1.  **Local (Thin/Edge)**:
    *   **Tasks**: Initial planning, routing, text summarization, and simple "life admin" tasks.
    *   **Engine**: Llama.cpp or Ollama running on the Ryzen 7840HS (CPU/iGPU/NPU).
    *   **Models**: Ultra-compact models (e.g., Phi-3, Gemma-2B, or specialized 7B models).
2.  **Cloud/Hosted (Thick/Research)**:
    *   **Tasks**: Complex software engineering, deep research synthesis, and high-context multi-document analysis.
    *   **Engine**: Green Cloud providers (Green AI Cloud, Exoscale) or a hosted VM with a high-end GPU.
    *   **Strategy**: Only "burst" to the cloud when local thresholds for accuracy or speed are not met.

## 5. Synthesis: The "Greenest" Setup Today

Based on current benchmarks and provider claims, the hierarchy of green LLM usage is:

1.  **GOLD STANDARD: Local Framework 16 (Ryzen AI) during Solar/Wind Peaks.**
    *   Using **NPU-optimized** models (1B-3B) for background agents.
    *   Scheduled via **Windows Task Scheduler** and **Carbon Aware SDK** (via PowerShell) to match renewable grid peaks.
    *   *Why*: Zero data transfer carbon, utilizes low-power NPU silicon, and 100% renewable local energy.

2.  **SILVER STANDARD: Specialized Green Cloud (e.g., Green AI Cloud).**
    *   *Why*: Utilizes industrial-scale heat recycling and 100% renewable energy that may not be available to residential users. Ideal for "thick" research tasks that exceed iGPU capacity.

3.  **BRONZE STANDARD: Quantized Local Inference on Radeon 780M iGPU.**
    *   Using **LM Studio (DirectML/Vulkan)** with **IQ4_XS** GGUF models.
    *   Applying Windows "Efficiency Mode" to background workers to improve power-per-token metrics.

---
*Date: 2026-01-20*
*Research Phase: 1 (Infrastructure)*
