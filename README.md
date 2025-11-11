# RAG-Pipelines-n8n

A **production-ready n8n workflow** that automates **ingestion, update, and deletion** of documents in a **Retrieval-Augmented Generation (RAG)** system using **Google Drive** and **Supabase Vector Store**.

---

## Features

- **Pipeline 1: Upload** – New file in `RAG` folder → download as PDF → embed with **OpenAI** → insert into Supabase `documents` with metadata (`fileName`, `date`).
- **Pipeline 2: Update** – File modified → delete old vectors by filename → re-download, re-embed, re-insert.
- **Pipeline 3: Delete** – File moved to `Recycling Bin` → permanently remove all associated vectors.
- **RAG Chat Agent** – Live Q&A using **OpenRouter LLM** + Supabase vector retrieval for accurate, real-time answers.

---

## Setup Instructions

1. **Google Drive**
   - Connect your account via OAuth
   - Set folders:
     - `RAG`: `1rP7a--_p5TVL0sl4PMWXpkdnLeyXAKmy`
     - `Recycling Bin`: `1r2Mquv0KFvwzVVsvTb3stzRugQdbDc72`

2. **Supabase**
   - Create table `documents`:
     ```sql
     CREATE EXTENSION IF NOT EXISTS vector;
     CREATE TABLE documents (
       id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
       content TEXT,
       embedding VECTOR(1536),
       metadata JSONB
     );
Add API credentials in n8n


OpenAI
Add API key → used for embeddings (text-embedding-3-small recommended)

OpenRouter
Connect API key → powers the chat agent (supports Llama, Mistral, etc.)

Test the Flow
Upload a doc → verify in Supabase
Edit file → confirm update
Move to Recycling Bin → vectors deleted
Use chat agent → get accurate, sourced answers
Component,Tool / Service
Automation,n8n (self-hosted or cloud)
File Source,Google Drive API + PDF export
Vector DB,Supabase (pgvector)
Embeddings,OpenAI
LLM,OpenRouter
Framework,LangChain Nodes (n8n-native)

