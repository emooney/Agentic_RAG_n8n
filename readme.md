# 📚 Working Agentic RAG Workflow (n8n)

This project implements a fully operational **Agentic Retrieval-Augmented Generation (RAG)** system built on [n8n](https://n8n.io), combining document intelligence, real-time chat interaction, and vector-based knowledge retrieval using LangChain, OpenAI, Supabase, and PostgreSQL.

---

## 🚀 Features

- 📥 **Multi-format Document Ingestion**
  - Supports PDFs, Excel, CSV, and Google Docs
  - Automatically triggered on file creation or update in Google Drive

- 🧠 **Agentic RAG AI**
  - Automatically chooses between:
    - Embedding search (RAG)
    - SQL queries on structured data
    - Full document content lookup
  - Backed by LangChain’s agent framework

- 📈 **Vector & Tabular Storage**
  - Vector embeddings stored in Supabase with `pgvector`
  - Tabular data saved in Postgres JSONB format (no per-file tables)

- 🔁 **Continuous Updates**
  - Watches a Google Drive folder for new/updated documents
  - Auto-updates document records and removes stale data

- 💬 **Chat Interfaces**
  - Supports real-time queries via:
    - Webhook endpoint
    - LangChain Chat UI trigger

---

## 📁 Document Ingestion Pipeline

1. **Trigger:**
   - Google Drive Triggers (`fileCreated` / `fileUpdated`)
2. **Metadata Extraction:**
   - Extracts and sets: file ID, name, type, link
3. **Routing Based on File Type:**
   - `application/pdf` → Extract PDF Text
   - `.xlsx` → Extract from Excel
   - `.csv` → Extract from CSV
   - Google Docs → Extract Document Text
4. **Embeddings & Storage:**
   - OpenAI Embeddings (`text-embedding-3-small`)
   - Text chunks → Supabase vectorstore
   - Tabular rows → Postgres `document_rows` table
   - Metadata → Postgres `document_metadata` table

---

## 🤖 RAG AI Agent

### Tools Available to the Agent

| Tool Name | Function |
|-----------|----------|
| `documents` | Embedding-based semantic search |
| `List Documents` | Lists uploaded files with metadata |
| `Get File Contents` | Returns full document text by file ID |
| `Query Document Rows` | Executes SQL on tabular row data |

### AI Agent Behavior

- Follows a system prompt to:
  - Prioritize vector search (RAG)
  - Fall back to SQL if necessary
  - Avoid hallucinations
  - Clarify when an answer can't be found

---

## 🗄️ Database Setup (Supabase / Postgres)

Run these nodes **once** to set up your tables:

1. `Create Documents Table and Match Function`
2. `Create Document Metadata Table`
3. `Create Document Rows Table (for Tabular Data)`

These create:
- A vector-enabled table for document embeddings
- Metadata and tabular storage in Postgres

---

## 🧠 Memory Management

- Persistent memory per session using `Postgres Chat Memory`
- Supports multi-turn conversations

---

## ✅ Requirements

- n8n v1.44+ (or latest)
- Supabase with `pgvector` enabled
- OpenAI account (API key and embedding model)
- Google Drive credentials
- PostgreSQL with proper schemas

---

## 🔒 Security Notes

- Webhook endpoints should be protected with header authentication
- Credentials must be scoped with least privilege (e.g., read-only access to Drive)

---

## ✍️ Author

Created by [Cole Medin](https://www.youtube.com/@ColeMedin)  
Extended and analyzed by N8N A.I Assistant (By Nskha)

---

## 🛠️ Customization Ideas

- Add document summarization during ingest
- Integrate alternative vector DBs (e.g., Pinecone, Weaviate)
- Enable local/offline LLM support (e.g., Ollama or LM Studio)
- Expand file triggers to support Dropbox, S3, etc.

---

## 📌 Status

**✅ Active** — fully functional Agentic RAG pipeline  
**🧪 Extensible** — customize it for your own knowledge base!

---