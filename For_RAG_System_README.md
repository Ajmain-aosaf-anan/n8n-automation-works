# 📁 RAG Automation System 2

This project is an automated **Retrieval-Augmented Generation (RAG)** pipeline built using [n8n](https://n8n.io/). It fetches campaign documents from Google Drive, processes them, converts their contents into embeddings using Ollama, stores them in Supabase, and allows querying through a chat agent.

---

## 🚀 Features

- ⏱️ **Hourly Automation** using a Cron trigger
- 📂 Retrieves files from **Google Drive**
- 📄 Extracts structured content from files
- 🧠 Embeds documents using **Ollama (nomic-embed-text)**
- 🗃️ Stores vectors in **Supabase vector store**
- 💬 Enables AI-powered Q&A via **Ollama Chat + PostgreSQL memory**
- ✅ Filters files by name (e.g., `"Copy of Campaign data"`)

---

## 📡 Endpoints & Nodes Used

| Service | n8n Node | Description |
|--------|----------|-------------|
| Google Drive | `Google Drive` | Search and download campaign files |
| Supabase | `Supabase Vector Store` | Store and retrieve document embeddings |
| Ollama | `Embeddings Ollama`, `Ollama Chat Model` | Text embeddings and chat model |
| PostgreSQL | `Postgres Chat Memory` | Store contextual chat memory |
| LangChain Agent | `AI Agent` | Respond to user queries using embedded knowledge |
| Trigger | `Schedule Trigger`, `When Chat Message Received` | Runs hourly or when chat is received |

---

## 🔐 API Keys / Credentials Setup

| Service | Required Credential Name |
|--------|---------------------------|
| Google Drive | `Google Drive account` |
| Supabase | `Supabase account 2` |
| Ollama | `Ollama account` |
| PostgreSQL | `Postgres account 2` |

To configure:
1. Open [n8n](https://n8n.io/)
2. Navigate to **Credentials** tab
3. Add each service using the names listed above
4. Paste your actual API keys or OAuth details

---

## ⚙️ Configuration

### 1. Google Drive Folder
Set the folder ID in the `Google Drive` node:
1-XOMgeYY9mbEL3g_rqvZun9EYFmEe8Ve

### 2. File Filter Condition
In the `If` node:
```js
file_name === "Copy of Campaign data"
```

{
  "Campaign ID": "12345",
  "Campaign name": "Campaign 1",
  "Revenue": "138.65",
  ...
}

## Chat Agent Integration
- Trigger: When Chat Message Received
- Model: mistral:latest
- Memory: via PostgreSQL (context window: 10)
- Response System Message:
"You are a helpful assistant. You will use the vector database to retrieve useful information and respond to the user's query."

## How to Use
- Clone or import the workflow into your n8n instance.
- Set all required credentials.
- Set the Google Drive folder to watch.
- Let it run on schedule or trigger chat via webhook.
- Start asking questions via your chat interface.

## 📈 Workflow Diagram
graph TD
    A[Schedule Trigger] --> B[Search Google Drive]
    B --> C[Extract File Info]
    C --> D[Download File]
    D --> E[Filename Check]
    E --> F1[Extract from File]
    F1 --> G1[Embed via Ollama]
    G1 --> H1[Store in Supabase]
    Chat[Chat Trigger] --> Agent[LangChain AI Agent]
    Agent --> Model[Ollama Chat Model]
    Model --> Memory[Postgres Chat Memory]
    Agent --> Tool[Query Vector Store]

## 📌 Notes
- Embedding model: nomic-embed-text:latest
- Chat model: mistral:latest
- Vectors stored in documents table in Supabase
- File ID metadata is saved for traceability

## 🧩 Future Improvements
- Add real-time Slack/Telegram integration
- Add support for PDF, Excel, CSV files
- Improve search ranking using RAG scoring logic
- Build UI frontend for manual document upload
