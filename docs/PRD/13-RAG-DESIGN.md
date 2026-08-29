# 13-RAG-DESIGN.md
# Retrieval-Augmented Generation (RAG) Architecture

## AI English Speaking Platform

**Version:** 1.0  
**Module:** RAG Engine  
**Framework:** NestJS  
**Database:** PostgreSQL + pgvector  
**AI Providers:** OpenAI, Claude, Gemini  
**Storage:** S3 / Cloudflare R2

---

# 1. Overview

## Purpose

RAG (Retrieval-Augmented Generation) cho phép AI Tutor truy xuất kiến thức từ kho dữ liệu nội bộ thay vì chỉ dựa vào kiến thức có sẵn của LLM.

---

## Benefits

```text
Accurate Answers

Up-to-Date Content

Lower Hallucination Rate

Personalized Learning

Cost Optimization
```

---

# 2. Business Objectives

Cho phép AI:

```text
Answer Grammar Questions

Explain Vocabulary

Retrieve Course Content

Access Learning Materials

Use Student History
```

---

# 3. High-Level Architecture

```text
Knowledge Sources
        ↓
Document Processing
        ↓
Chunking Pipeline
        ↓
Embedding Engine
        ↓
Vector Database
        ↓
Retriever
        ↓
Re-Ranker
        ↓
Prompt Builder
        ↓
LLM
        ↓
Answer
```

---

# 4. Knowledge Sources

Supported Sources:

```text
Courses

Lessons

Vocabulary

Grammar Rules

Speaking Tips

IELTS Materials

Business English Guides

PDF Documents

Markdown Documents
```

---

# 5. Knowledge Categories

```text
Grammar

Vocabulary

Pronunciation

Conversation

IELTS

TOEIC

Business English

Travel English
```

---

# 6. Knowledge Base Architecture

```text
Knowledge Base
├── Categories
├── Documents
├── Chunks
├── Embeddings
└── Metadata
```

---

# 7. Document Lifecycle

```text
Upload
 ↓
Parse
 ↓
Chunk
 ↓
Embed
 ↓
Store
 ↓
Retrieve
```

---

# 8. Supported Document Types

```text
PDF

DOCX

TXT

Markdown

HTML

JSON
```

---

# 9. Document Processing Pipeline

```text
Upload Document
 ↓
Extract Text
 ↓
Clean Text
 ↓
Chunk Text
 ↓
Generate Embeddings
 ↓
Store Vector
```

---

# 10. Chunking Strategy

## Fixed Size

```text
500 Tokens
```

---

## Overlap

```text
100 Tokens
```

---

Benefits:

```text
Context Preservation

Better Retrieval
```

---

# 11. Chunk Example

Document:

```text
Grammar Guide
```

↓

```text
Chunk 1

Chunk 2

Chunk 3
```

---

# 12. Chunk Metadata

```json
{
  "documentId": "uuid",
  "category": "grammar",
  "language": "en",
  "version": 1
}
```

---

# 13. Embedding Engine

Purpose:

```text
Convert Text → Vector
```

---

# 14. Embedding Providers

Priority:

```text
1. OpenAI text-embedding-3-large

2. OpenAI text-embedding-3-small

3. Gemini Embedding

4. Cohere Embedding
```

---

# 15. Embedding Dimensions

Example:

```text
3072 Dimensions
```

---

Stored As:

```text
Vector
```

---

# 16. Vector Database

Recommended:

```text
PostgreSQL + pgvector
```

---

Alternative:

```text
Pinecone

Weaviate

Qdrant
```

---

# 17. Why pgvector?

Advantages:

```text
Simple Architecture

Low Cost

 SQL Integration

 No Extra Service
```

---

# 18. Vector Search Flow

```text
User Query
 ↓
Embedding
 ↓
Vector Search
 ↓
Top K Results
```

---

# 19. Retrieval Pipeline

```text
Question
 ↓
Embedding
 ↓
Vector Search
 ↓
Retrieve Chunks
 ↓
Re-Rank
 ↓
Prompt Builder
 ↓
LLM
```

---

# 20. Top-K Retrieval

Default:

```text
Top 10 Chunks
```

---

Dynamic:

```text
5 - 20 Chunks
```

---

# 21. Hybrid Search

Combine:

```text
Vector Search

+
Keyword Search
```

---

Benefits:

```text
Better Accuracy
```

---

# 22. PostgreSQL Full Text Search

Use For:

```text
Keyword Matching
```

---

Example:

```sql
to_tsvector(content)
```

---

# 23. Hybrid Retrieval Formula

```text
70% Vector Score

30% Keyword Score
```

---

# 24. Re-Ranking Engine

Purpose:

```text
Improve Retrieval Quality
```

---

# 25. Re-Ranking Flow

```text
Top 20 Results
 ↓
Cross Encoder
 ↓
Top 5 Results
```

---

# 26. Re-Ranking Providers

Options:

```text
Cohere Rerank

BGE Reranker

OpenAI Rerank
```

---

# 27. Prompt Augmentation

Prompt Structure:

```text
System Prompt

Retrieved Context

User Question

Instructions
```

---

# 28. Example Prompt

```text
Use only the provided context.

Context:
{retrieved_chunks}

Question:
What is Present Perfect?
```

---

# 29. Hallucination Prevention

Rules:

```text
Use Retrieved Knowledge

Cite Sources

Do Not Invent Facts
```

---

# 30. Knowledge Versioning

Document Versions:

```text
v1

v2

v3
```

---

Reason:

```text
Auditability

Rollback
```

---

# 31. Knowledge Freshness

Update Methods:

```text
Manual Upload

Scheduled Sync

Admin Portal
```

---

# 32. Personal Learning RAG

Separate Index:

```text
User Notes

Assessments

Learning History
```

---

Purpose:

```text
Personalized Responses
```

---

# 33. Personal Context Retrieval

Example:

```text
User Weakness:
Pronunciation
```

↓

AI retrieves:

```text
Past Speaking Scores
```

---

# 34. Multi-Language Support

Languages:

```text
English

Vietnamese

Chinese

Japanese
```

---

Embedding:

```text
Multilingual Models
```

---

# 35. Knowledge Database Schema

## knowledge_documents

```sql
id UUID

title VARCHAR

category VARCHAR

version INTEGER

status VARCHAR

created_at TIMESTAMP
```

---

# 36. Knowledge Chunks Schema

## knowledge_chunks

```sql
id UUID

document_id UUID

chunk_index INTEGER

content TEXT

metadata JSONB
```

---

# 37. Embeddings Schema

## knowledge_embeddings

```sql
id UUID

chunk_id UUID

embedding VECTOR(3072)
```

---

# 38. Search Logs Schema

## search_logs

```sql
id UUID

query TEXT

retrieved_chunks INTEGER

response_time INTEGER
```

---

# 39. RAG APIs

## Search Knowledge

```http
POST /api/v1/rag/search
```

---

## Upload Document

```http
POST /api/v1/rag/documents
```

---

## Rebuild Embeddings

```http
POST /api/v1/rag/reindex
```

---

## Get Document

```http
GET /api/v1/rag/documents/{id}
```

---

# 40. BullMQ Jobs

```text
document-processing

embedding-generation

reindexing

chunk-cleanup
```

---

# 41. Redis Cache Strategy

Keys:

```text
embedding:{hash}

search:{query}

rag-response:{query}
```

---

TTL:

```text
15 Minutes

30 Minutes

1 Hour
```

---

# 42. Performance Targets

Search Latency:

```text
< 200ms
```

---

RAG Response:

```text
< 2 Seconds
```

---

Embedding Generation:

```text
< 5 Seconds
```

---

# 43. Cost Optimization

Strategies:

```text
Embedding Cache

Prompt Compression

Chunk Deduplication

Response Cache
```

---

# 44. Scalability Targets

Support:

```text
10M Documents

100M Chunks

1M Searches/Day
```

---

Architecture:

```text
Horizontal Scaling

Partitioning

Read Replicas
```

---

# 45. Security

Access Control:

```text
RBAC
```

---

Protect:

```text
Private Documents

User Data

Assessment History
```

---

# 46. Monitoring Metrics

Track:

```text
Retrieval Accuracy

Latency

Hit Rate

Token Usage

Search Volume
```

---

# 47. Future Enhancements

Phase 2

```text
Graph RAG
```

---

Phase 3

```text
Agentic Retrieval
```

---

Phase 4

```text
Multimodal RAG
```

---

Phase 5

```text
Video Knowledge Retrieval
```

---

# 48. Integration Points

Connected To:

```text
AI Tutor

Speaking Engine

Course Module

Assessment Module

Analytics Module
```

---

# Conclusion

RAG Engine là nền tảng tri thức của AI English Speaking Platform.

Cung cấp:

- Knowledge Retrieval
- Hybrid Search
- Re-Ranking
- Personalized Context
- Hallucination Reduction
- Cost Optimization

Được thiết kế để:

- Chính xác
- Mở rộng cao
- Chi phí thấp
- Dễ vận hành

Tech Stack:

- NestJS
- PostgreSQL
- pgvector
- Redis
- BullMQ
- OpenAI Embeddings
- Claude
- Gemini
- Kubernetes