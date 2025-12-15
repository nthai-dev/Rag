# Confluence RAG – High‑level Architecture & Browser Extension Plan

## Overview
This document describes the end‑to‑end design for a Retrieval‑Augmented Generation (RAG)
system that reads from Confluence and is delivered via a browser extension capable of
interacting with the current UI.

---
## High‑level Architecture
User → Browser Extension (Sidebar/Popup) → Backend API →
Hybrid Retrieval (Vector + Keyword, ACL‑filtered) → Reranker →
LLM → Answer with Citations → Optional Safe UI Actions

---
## Ingestion & Indexing
- Source: Confluence REST API / Webhooks
- Processing: HTML → clean text → section‑aware chunking
- Storage:
  - Postgres (Supabase)
  - pgvector for embeddings
  - GIN tsvector for keyword search
- ACL mirrored from Confluence (users/groups)

---
## Retrieval Flow
1. Query rewrite (optional)
2. Hybrid search (dense + sparse)
3. Merge & dedupe
4. Cross‑encoder rerank
5. Context assembly (token‑budget aware)
6. LLM generation with citations

---
## Browser Extension (MV3)
Components:
- Popup (quick ask)
- Sidebar (chat + sources)
- Content script (DOM context + actions)
- Background service worker (auth, routing)

Capabilities:
- Read page context (URL, title, selection, visible text)
- Show inline citations & highlights
- Execute safe, user‑approved UI actions

---
## Security & Privacy
- OAuth2 PKCE (extension → backend)
- ACL‑filtered retrieval (RLS)
- Explicit user approval for UI actions
- Full audit logging

---
## Delivery Plan
Phase 1:
- Ingestion + basic RAG
- Read‑only browser extension (Q&A)

Phase 2:
- Hybrid search + reranker
- Inline citations & highlights

Phase 3:
- Safe UI action framework
- Advanced UX & evaluation metrics


## References:
- https://www.pinecone.io/learn/retrieval-augmented-generation/
