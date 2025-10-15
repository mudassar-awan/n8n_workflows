# Agentic Workflow using Supabase

**Description:**  
This project implements an **agentic AI workflow** using **Supabase** and **n8n**.  
It allows automated document ingestion, embedding generation, and intelligent retrieval-augmented chat using **Google Gemini** models.

Each uploaded document (like PDF or text) is automatically:
1. Converted into embeddings using **Google Gemini Embeddings**.
2. Stored in a **Supabase Vector Database**.
3. Queried by an **AI Agent** that responds to user input using relevant document context.

---

## 🧠 Workflow Overview

**Workflow Name:** `Agentic Supabase Workflow`  
**Platform:** [n8n.io](https://n8n.io)  
**Database:** [Supabase](https://supabase.com)  
**AI Model:** [Google Gemini](https://ai.google.dev/)  
**Embedding Storage:** Supabase Vector Store  

This workflow uses a **Retrieval-Augmented Generation (RAG)** pattern where the agent can call the Supabase vector database to fetch relevant data before generating intelligent responses.

---

## ⚙️ Key Features

- 🔹 File ingestion from **Google Drive**
- 🔹 Document parsing and binary data handling
- 🔹 Embedding generation using **Google Gemini**
- 🔹 Vector storage in **Supabase**
- 🔹 AI Agent with tool-calling and context awareness
- 🔹 Chat memory using **PostgreSQL**
- 🔹 Webhook-based chat interface for real-time queries

---

## 🧩 Workflow Steps (Simplified)

1. **Trigger Node** – Starts the execution (manual or via webhook).  
2. **Google Drive Node** – Fetches the uploaded file.  
3. **Document Loader Node** – Reads the binary content.  
4. **Gemini Embeddings Node** – Generates embeddings from the file text.  
5. **Supabase Node** – Stores embeddings and metadata into the vector DB.  
6. **Webhook Trigger** – Listens for chat queries.  
7. **Agent Node** – Uses Gemini + Supabase retrieval to generate context-aware responses.  
8. **Database Node** – Logs and manages chat history.

---

## 🧰 Prerequisites

| Requirement | Description |
|--------------|-------------|
| n8n Account | [https://n8n.io](https://n8n.io) |
| Supabase Project | [https://supabase.com](https://supabase.com) |
| Google AI API Key | Needed for Gemini models |


## 🚀 Usage

1. Clone the repository:  
   ```bash
   git clone https://github.com/mudassar-awan/n8n_workflows.git
