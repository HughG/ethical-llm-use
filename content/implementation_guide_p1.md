# Implementation Guide: Proposal 1 (The Transparent Localist)

This guide provides a low-cognitive-load, step-by-step setup for a 100% local, ethical LLM workflow on **Windows 11** using the **Framework 16 (Ryzen 7840HS)**.

## Goal
Integrate a local, ethically-trained LLM into VSCode or JetBrains IDEs with minimal "plumbing."

---

## Step 1: Install the Local Inference Engine
We will use **Ollama** because it is a "set and forget" background service for Windows.

1. **Download:** Go to [ollama.com/download](https://ollama.com/download) and install the Windows version.
2. **Launch:** Run Ollama. You will see an icon in your system tray.
3. **Download an Ethical Model:** Open Terminal (PowerShell) and run:
   ```powershell
   ollama run olmo
   ```
   *Note: This downloads **OLMo** (Open Language Model), which has fully transparent training data. Once it finishes downloading, you can type a message to test it, then type `/exit`.*

---

## Step 2: Install the IDE Extension
We will use **Continue**, as it is the most flexible open-source "Copilot" alternative for both VSCode and JetBrains.

1. **VSCode:** Search for "Continue" in the Extensions Marketplace and install it.
2. **JetBrains:** Search for "Continue" in the Plugin Marketplace (Settings > Plugins).
3. **Initial Setup:** Once installed, click the **Continue** icon in the sidebar (usually a small 'C' or play button).

---

## Step 3: Connect Continue to Ollama
You need to tell the IDE to talk to your local machine instead of the cloud.

1. In the Continue sidebar, click the **gear icon (Open Config)**. This opens a `config.json` file.
2. Find the `"models"` array and replace or add an entry for Ollama:
   ```json
   {
     "title": "OLMo (Local)",
     "model": "olmo",
     "provider": "ollama"
   }
   ```
3. Save the file. Continue will automatically detect the local model.

---

## Step 4: Enabling Web Search (Optional but Recommended)
To allow the model to search the web from your IDE:

1. In the same `config.json`, find the `"contextProviders"` section.
2. Add the Google Search or Tavily provider. For a "low-setup" version, you can often use the built-in `docs` provider to index local folders.
3. **Usage:** In the chat box, type `@web` followed by your question (e.g., `@web what is the latest version of the Ryzen AI SDK?`).

---

## Step 5: Optimization for Framework 16
To ensure you aren't draining your battery or causing high fan noise:

1. **Windows Efficiency Mode:** Open Task Manager, find the `ollama_llama_server.exe` process, right-click it, and select **Efficiency Mode**.
2. **iGPU Acceleration:** Ollama should automatically detect your Radeon 780M. If the response feels slow (under 5 tokens/sec), ensure your AMD drivers are up to date.

---

## Success Criteria
- You can highlight code in your IDE and press `Ctrl+L` (or `Cmd+L`) to ask the local OLMo model questions.
- No data is leaving your laptop (unless you explicitly use a web search provider).
- You are using a model with a transparent "data diet."
