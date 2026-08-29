# 05-AI-ARCHITECTURE.md
# AI Architecture Design

## AI English Speaking Platform

**Version:** 1.0  
**Architecture Type:** Multi-LLM AI Platform  
**Purpose:** English Speaking Training & Assessment  
**Target Scale:** 100,000+ Concurrent Users

---

# 1. Overview

## Objective

Xây dựng nền tảng AI hỗ trợ học tiếng Anh giao tiếp thông qua:

- AI Conversation
- AI Speaking Assessment
- AI Tutor
- AI Feedback
- AI Learning Roadmap
- AI Recommendation Engine

Hệ thống phải:

- Chính xác
- Mở rộng được
- Tối ưu chi phí
- Hỗ trợ nhiều AI Provider
- Không phụ thuộc vào một nhà cung cấp AI duy nhất

---

# 2. AI Architecture Overview

```text
User
 ↓
Frontend
 ↓
API Gateway
 ↓
AI Gateway
 ↓
AI Router
 ↓
------------------------------------------------
|              |               |               |
v              v               v               v

OpenAI       Claude         Gemini       Internal AI
 ↓              ↓               ↓               ↓

------------------------------------------------
 ↓

Response Aggregator
 ↓

Memory Engine
 ↓

RAG Engine
 ↓

Final Response
 ↓

User
```

---

# 3. AI Core Components

## AI Gateway

Trung tâm điều phối tất cả AI requests.

Responsibilities:

- Authentication
- Quota Check
- Model Selection
- Cost Tracking
- Logging
- Monitoring

---

## AI Router

Lựa chọn model phù hợp cho từng tác vụ.

Responsibilities:

- Route Request
- Load Balance
- Fallback Provider
- Cost Optimization

---

## Memory Engine

Quản lý lịch sử hội thoại.

Responsibilities:

- Short-Term Memory
- Long-Term Memory
- Context Compression

---

## RAG Engine

Cung cấp kiến thức học tập.

Responsibilities:

- Embedding
- Retrieval
- Context Building

---

## Assessment Engine

Chấm điểm kỹ năng nói.

Responsibilities:

- Pronunciation
- Fluency
- Grammar
- Vocabulary
- Coherence

---

# 4. Multi-LLM Strategy

## Supported Providers

### OpenAI

Primary:

```text
GPT-5
GPT-5 Mini
```

Best For:

- Speaking Feedback
- Grammar
- Pronunciation Analysis

---

### Claude

Primary:

```text
Claude Sonnet
Claude Opus
```

Best For:

- Long Context
- Learning Plan
- Deep Analysis

---

### Gemini

Primary:

```text
Gemini Pro
Gemini Flash
```

Best For:

- Fast Responses
- Low Cost Tasks

---

# 5. AI Routing Strategy

## Routing Matrix

| Task | Model |
|--------|--------|
| Grammar Correction | GPT |
| Pronunciation Feedback | GPT |
| Long Conversation | Claude |
| Study Plan | Claude |
| Quick Chat | Gemini |
| Knowledge Search | GPT |
| IELTS Feedback | Claude |
| Daily Practice | Gemini |

---

## Fallback Strategy

```text
GPT
 ↓ fail
Claude
 ↓ fail
Gemini
 ↓ fail
Internal Fallback Message
```

---

# 6. AI Request Lifecycle

```text
User Request
 ↓
Validation
 ↓
Quota Check
 ↓
Load Context
 ↓
Build Prompt
 ↓
Select Model
 ↓
Call AI Provider
 ↓
Validate Response
 ↓
Store Logs
 ↓
Return Response
```

---

# 7. Prompt Engineering Framework

## Prompt Structure

```text
SYSTEM PROMPT

CONTEXT

USER PROFILE

CONVERSATION HISTORY

CURRENT TASK

USER INPUT
```

---

## Example

```text
You are an English teacher.

Student Level:
Intermediate

Learning Goal:
IELTS 7.0

Task:
Evaluate speaking performance.

Conversation:
...

Student Input:
...
```

---

# 8. Prompt Template System

Database Table:

```text
ai_prompt_templates
```

---

Example:

```text
conversation_teacher

conversation_interviewer

grammar_checker

pronunciation_feedback

ielts_examiner
```

---

## Versioning

```text
v1
v2
v3
```

Rollback supported.

---

# 9. Memory Architecture

## Short-Term Memory

Store:

```text
Current Conversation
```

Duration:

```text
Session Lifetime
```

Storage:

```text
Redis
```

---

## Long-Term Memory

Store:

```text
User Learning History
```

Storage:

```text
PostgreSQL
```

---

## Context Compression

Purpose:

Reduce token usage.

Method:

```text
Conversation
 ↓
Summarization
 ↓
Compressed Context
```

---

# 10. Context Builder

Input Sources

```text
User Profile

Learning Goal

Current Lesson

Conversation History

Recent Assessments
```

---

Output

```text
Optimized Prompt
```

---

# 11. RAG Architecture

## Purpose

Cho phép AI truy xuất kiến thức học tập.

---

Knowledge Sources

```text
Grammar

Vocabulary

IELTS Materials

Speaking Templates

Business English

Internal Lessons
```

---

# 12. RAG Pipeline

```text
User Query
 ↓
Embedding
 ↓
Vector Search
 ↓
Top K Results
 ↓
Context Builder
 ↓
Prompt
 ↓
LLM
 ↓
Response
```

---

# 13. Embedding Strategy

Recommended Models

```text
text-embedding-3-large

text-embedding-3-small
```

---

Chunk Size

```text
500 Tokens
```

---

Chunk Overlap

```text
50 Tokens
```

---

# 14. Vector Database

Options

### PostgreSQL + pgvector

Recommended MVP

---

### Pinecone

Recommended Scale

---

### Weaviate

Enterprise Scale

---

# 15. Conversation AI

## AI Roles

Supported:

```text
Teacher

Friend

Interviewer

Customer

Examiner

Business Partner
```

---

## Dynamic Persona

Example:

```text
You are a friendly English teacher.

Speak naturally.

Correct mistakes politely.

Encourage the student.
```

---

# 16. Speaking Assessment Engine

Input:

```text
Audio
```

Output:

```text
Score

Feedback

Recommendations
```

---

# 17. Assessment Pipeline

```text
Audio
 ↓
Speech To Text
 ↓
Transcript
 ↓
Grammar Analysis
 ↓
Pronunciation Analysis
 ↓
Fluency Analysis
 ↓
Vocabulary Analysis
 ↓
Final Score
```

---

# 18. Pronunciation Engine

Evaluation Metrics

### Accuracy

Word correctness

---

### Stress

Word stress

---

### Intonation

Sentence rhythm

---

### Clarity

Speech clarity

---

Scoring:

```text
0 - 100
```

---

# 19. Fluency Engine

Metrics

```text
Pause Frequency

Speech Rate

Filler Words

Sentence Completion
```

---

# 20. Grammar Engine

Checks:

```text
Tenses

Articles

Prepositions

Sentence Structure

Agreement
```

---

# 21. Vocabulary Engine

Metrics

```text
Lexical Diversity

Vocabulary Range

Topic Relevance

Word Complexity
```

---

# 22. Coherence Engine

Checks

```text
Logical Flow

Topic Consistency

Sentence Linking
```

---

# 23. Final Scoring Formula

```text
Pronunciation = 25%

Fluency = 20%

Grammar = 20%

Vocabulary = 20%

Coherence = 15%
```

---

Overall Score

```text
Weighted Average
```

---

# 24. Feedback Generation

Output Example

```json
{
  "strengths": [
    "Good vocabulary usage"
  ],
  "weaknesses": [
    "Incorrect pronunciation of TH sounds"
  ],
  "recommendations": [
    "Practice minimal pairs"
  ]
}
```

---

# 25. Personalized Learning Engine

Inputs

```text
Assessment History

Weak Skills

Learning Goals

Conversation Performance
```

---

Outputs

```text
Lessons

Topics

Exercises

Vocabulary Lists
```

---

# 26. AI Recommendation System

Recommend:

```text
Next Lesson

Daily Practice

Weekly Goals

Review Materials
```

---

# 27. AI Cost Optimization

## Problem

AI Cost Increases Rapidly

---

Solutions

### Cache Responses

Redis

---

### Context Compression

Reduce Tokens

---

### Model Routing

Simple Tasks → Gemini

Complex Tasks → Claude

Speaking Feedback → GPT

---

### Batch Processing

BullMQ Workers

---

# 28. AI Quota System

Free User

```text
10 Conversations / Day
```

---

Premium User

```text
Unlimited
```

---

Enterprise

```text
Custom Limits
```

---

# 29. AI Guardrails

Purpose:

Prevent Unsafe Responses

---

Rules

```text
No Hate Speech

No Harassment

No Violence

No Illegal Instructions

Educational Focus
```

---

# 30. Prompt Injection Protection

Detect:

```text
Ignore Previous Instructions

Reveal System Prompt

Execute Hidden Commands
```

---

Action:

```text
Reject Request
```

---

# 31. AI Moderation Layer

Pipeline

```text
User Input
 ↓
Moderation
 ↓
AI
 ↓
Moderation
 ↓
Response
```

---

# 32. AI Observability

Track

```text
Response Time

Token Usage

Error Rate

Cost

Provider Usage
```

---

Storage

```text
PostgreSQL
```

Dashboard

```text
Grafana
```

---

# 33. AI Analytics

Metrics

```text
Daily Requests

Average Cost

Average Latency

Top Prompts

Top Lessons
```

---

# 34. AI Evaluation Framework

Measure

```text
Accuracy

Helpfulness

Safety

Grammar Quality

Learning Effectiveness
```

---

Testing

```text
Automated Evaluation

Human Review

A/B Testing
```

---

# 35. AI Failure Handling

Cases

```text
Provider Down

Timeout

Rate Limit

Invalid Response
```

---

Recovery

```text
Retry

Fallback Model

Cached Response
```

---

# 36. Future AI Roadmap

Phase 2

```text
IELTS Examiner AI

TOEIC Coach AI

Business English Coach
```

---

Phase 3

```text
AI Avatar

Video Conversations

Emotion Detection
```

---

Phase 4

```text
Real-time Speaking Coach

Live Pronunciation Correction

Multilingual Tutor
```

---

# 37. AI Scalability Targets

Requests Per Day

```text
10,000,000+
```

---

Concurrent Users

```text
100,000+
```

---

AI Availability

```text
99.9%
```

---

# Conclusion

AI Architecture được thiết kế theo mô hình:

```text
Multi-LLM
+
RAG
+
Memory Engine
+
Assessment Engine
+
Recommendation Engine
```

Hỗ trợ:

- OpenAI
- Claude
- Gemini
- PostgreSQL
- Redis
- BullMQ
- Kubernetes

Đảm bảo:

- Chi phí tối ưu
- Hiệu suất cao
- Khả năng mở rộng lớn
- Trải nghiệm học tập cá nhân hóa
- Enterprise Ready