
## 📚 RAG Tutorial: Ask Questions from Your PDFs with OpenWebUI

This tutorial walks you through how to use **OpenWebUI** to interact with your own PDF documents. 
You'll load your documents into OpenWebUI and ask natural language questions based on their contents.

Here’s the overall workflow:

1. Prepare your PDF documents.
2. Create a searchable document space (**Knowledge Base**) in OpenWebUI.
3. Upload your PDFs — OpenWebUI will automatically index their contents.
4. Ask questions through the chat interface and get answers directly from your documents.

> 🧠 **Note:** This tutorial assumes you have already completed the [previous setup](https://github.com/KT-AI-Midm/M-2.0-Beta-test/tree/main/tutorial/03_open-webui) to run OpenWebUI in Docker and run Mi:dm 2.0-base locally using Ollama.

---

### 📁 1. Prepare PDF Documents

For this tutorial, you will use three sample PDF documents located in the `sample_documents` folder of this repository.

Download the following files:

- [`1_edu.pdf`](./sample_documents/1_edu.pdf) — A document related to education  
- [`2_gov.pdf`](./sample_documents/2_gov.pdf) — A document related to government policy  
- [`3_sports.pdf`](./sample_documents/3_sports.pdf) — A document related to sports media

> ✅ **Tip:** You can replace these with your own structured PDF files later, such as reports, policy papers, or academic articles (in Korean or English).

---

### 🧠 2. Creating a Knowledge Base

Create a space within OpenWebUI where you can upload and manage searchable documents. This is referred to as a **Knowledge Base (KB)**.

#### 📌 Step-by-step

1. **Access OpenWebUI**  
   - Go to `http://localhost:3000` (default port), or use your custom port (`http://localhost:<port>`).

2. **Navigate to Knowledge Base creation**  
   - In the sidebar: `Workspace` → `Knowledge`  
   - Click the `+` button on the top-right to open the **"Create a knowledge base"** dialog.

3. **Create a new Knowledge Base**  
   - Example name: `my-pdf-kb`  
   - Add a brief description, then click the **`Create Knowledge`** button.

4. **Upload documents**  
   - Drag and drop your PDF files or use the upload button.  
   - The system will automatically extract and index document content in the background.

5. **Verify upload**  
   - Check that all documents appear in the KB and are saved successfully.

---

### 💬 3. Interacting with Your Documents

Now that your document space is ready, let’s start asking questions using RAG. OpenWebUI integrates documents directly into chat sessions using **Retrieval-Augmented Generation (RAG)**.

#### 🧭 Chat Session Preparation

1. **Start a new chat**  
   - Click **"New Chat"** at the top menu.

2. **Select a model**  
   - In the chat window or sidebar, choose a model from the dropdown.  
   - Models served via Ollama will be listed automatically.  
   - If you've followed the [setup guide](https://github.com/KT-AI-Midm/M-2.0-Beta-test/tree/main/tutorial/03_open-webui), the default model should be `midm:2.0-base`. You can choose another if available.

---

#### 📂 Referencing Documents

There are two main ways to incorporate documents into your queries:

1. **Reference an existing document from your KB**  
   - Type `#` in the chat prompt to see a list of documents in your Knowledge Base.  
   - Select a specific document(e.g., `#1_edu.pdf`) to focus your question.  
   - Alternatively, to query across the **entire Knowledge Base**, select the Knowledge Base name itself (e.g., `#my-pdf-kb`) instead of a single document.

2. **Upload a document on the fly**  
   - Click the **`+` button** next to the chat input to upload a new PDF.  
   - This document won't be added to the KB but can be used for the current session.

---

#### 🧪 Example Interactions

Below are examples of real-world questions you can ask after uploading your documents.  
These queries are written in Korean, assuming the uploaded documents are also in Korean.


---

**Question:**  
> 교사가 원격수업에서 저작물을 이용할 때 반드시 저작권자의 허락을 받아야 하나요?

**Generated Answer:**  
> 교사가 원격수업에서 저작물을 이용할 경우, 일반적으로 저작권자의 허락이 필요할 수 있습니다.  
하지만, 사용하려는 저작물이 「공공저작물 자유이용」 범위에 포함된다면 별도의 허락 없이도 사용할 수 있는 경우가 있을 수 있습니다.  
더 자세한 사항은 관련 지침이나 규정을 확인해 보시는 것이 좋습니다.

---

**Question:**  
> 공공기관에서 만든 모든 저작물을 자유롭게 사용할 수 있나요?

**Generated Answer:**  
> 공공기관이 제작한 모든 저작물을 무조건 자유롭게 사용할 수 있는 것은 아닙니다.  
일반적으로 공공기관에서 만든 자료도 저작권 보호 대상일 수 있어, 사용에 제한이 따를 수 있습니다.   
다만, ‘공공저작물 자유이용’ 정책 등 특별한 경우에는 별도 허락 없이 사용할 수 있도록 허용되기도 합니다.  
이처럼 구체적인 조건은 각 기관의 정책이나 관련 지침에 따라 달라질 수 있으니, 확인이 필요합니다.


---

### 🛠️ 4. Troubleshooting

Common issues and how to resolve them:

- **OpenWebUI not running:** Check Docker logs using `docker logs openwebui`. Make sure no port conflicts exist.
- **PDF upload error:** The file may be corrupted or unsupported. Try a different PDF or verify file integrity.
- **Low-quality RAG answers:** Refine your prompts or verify that your documents actually contain the answer. Consider breaking large documents into smaller ones.

---

### 🚀 5. Conclusion and Next Steps

Through this tutorial, you've learned how to build a local RAG system using OpenWebUI and integrate it with a Hugging Face-compatible model. This setup enables fast, contextual Q&A over custom documents.

#### Suggested Next Steps:

- **Apply to real-world documents:** Use personal or work-related PDFs to make the system practical.
- **Customize OpenWebUI:** Extend functionality via the API or tailor the UI for better usability.

We hope this tutorial was a helpful entry point into document-aware AI systems!

