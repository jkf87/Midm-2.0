# 🧩 Plug Mi:dm 2.0 into Open WebUI

This section explains how to integrate **Mi:dm 2.0** with [Open WebUI](https://github.com/open-webui/open-webui), a powerful, browser-based UI for managing and interacting with language models.

This guide covers how to:
- Launch Open WebUI using Docker
- Connect Mi:dm 2.0 via [Ollama](https://ollama.com/)
- Enable advanced features like **Model Context Protocol (MCP)** and **Retrieval-Augmented Generation (RAG)**

---

## 🚀 1. Run Open WebUI

Open WebUI supports multiple installation methods.
We recommend using **Docker** for a quick and consistent setup.

### 🧱 Option A: Run Open WebUI Only

> Use the official Docker image with default settings:

```bash
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui --restart always \
  ghcr.io/open-webui/open-webui:main
```

> For CUDA acceleration, use the following command:

```bash
docker run -d -p 3000:8080 \
  --gpus all \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui --restart always \
  ghcr.io/open-webui/open-webui:cuda
```

### 🧰 Option B: Run with MCP Server

> To enable Mi:dm 2.0’s tool-calling capabilities, launch Open WebUI with a pre-configured MCP server.
Please refer to the detailed setup guide in [MCP Exercise](./mcp/README.md)

Once the container is running, you can access Open WebUI at http://localhost:3000/ or http://127.0.0.1:3000/.

<br>

## 🔌 2. Connect Mi:dm 2.0 via Ollama

Mi:dm 2.0 supports OpenAI-compatible APIs and can be served using platforms like llama.cpp or LM Studio.
For seamless integration with Open WebUI, we recommend using **Ollama**.

### 🛠️ Setup Steps

1. Install Ollama on your local machine.

2. Download the `.gguf` model file from Hugging Face.
<!--
* [Midm 2.0-Base (GGUF)](https://huggingface.co/KT-AI/Midm-2.0-Base-Instruct-GGUF)
* [Midm 2.0-Mini (GGUF)](https://huggingface.co/KT-AI/Midm-2.0-Mini-Instruct-GGUF)
-->

3. Place the `.gguf` file in the same directory as the `Modelfile`.
* [Midm 2.0 Modelfile](./modelfile/Modelfile)

4. Create the Ollama model with:
    ```bash
    ollama create midm-2.0:base -f Modelfile
    ```

    > **📝 Note:**  
    > Ensure the `.gguf` file name exactly matches the `FROM` line in the `Modelfile`.

<br>

## 🧠 3. Try MCP & RAG with Mi:dm 2.0

We provide advanced interaction capabilities in Mi:dm 2.0 through the following components:

- 📁 [`mcp/`](./mcp) – Use external tools via function calling
- 📁 [`rag/`](./rag) – Incorporate document-based knowledge into responses

Each folder contains step-by-step tutorials, configuration examples, and sample prompts to guide you through the process.

> **📝 Note:**  
> These tutorials have been tested with: Ollama 0.9.0 · Open WebUI 0.6.15 · MCP 1.10.1 · MCPO 0.0.16  
> Newer versions are expected to work as well, but refer to these if needed.

---

🐾 *Now that Mi:dm 2.0 is plugged into Open WebUI, you’re ready to dive in and see what it can do!*
