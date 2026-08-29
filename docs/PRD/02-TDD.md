# 02-TDD.md
# Technical Design Document (TDD)

## AI English Speaking Platform

**Version:** 1.0  
**Status:** Draft  
**Architecture Style:** Modular Monolith → Microservices Ready  
**Tech Stack:**

- NextJS
- NestJS
- PostgreSQL
- Prisma
- Redis
- BullMQ
- OpenAI
- Claude
- Gemini
- RAG
- Kubernetes
- ArgoCD

---

# 1. Technical Overview

## Purpose

Tài liệu này mô tả thiết kế kỹ thuật chi tiết của hệ thống AI English Speaking Platform.

Mục tiêu:

- Khả năng mở rộng cao
- Dễ bảo trì
- AI-first architecture
- Cloud-native deployment
- Multi-LLM support
- Production-ready

---

# 2. High Level Architecture

```text
                    +----------------+
                    |     NextJS     |
                    |   Frontend UI  |
                    +--------+-------+
                             |
                             |
                             v
                  +----------+-----------+
                  |      API Gateway     |
                  |       NestJS         |
                  +----------+-----------+
                             |
      --------------------------------------------------
      |          |           |           |             |
      v          v           v           v             v

+-----------+ +----------+ +----------+ +----------+ +----------+
| Auth      | | User     | | Lesson   | | AI       | | Payment  |
| Service   | | Service  | | Service  | | Service  | | Service  |
+-----------+ +----------+ +----------+ +----------+ +----------+

                             |
                             |
                             v

                   +--------------------+
                   |      PostgreSQL    |
                   +--------------------+

                             |
                             v

                   +--------------------+
                   |       Redis        |
                   +--------------------+

                             |
                             v

                   +--------------------+
                   |      BullMQ        |
                   +--------------------+

                             |
                             v

          ----------------------------------------
          |                 |                    |
          v                 v                    v

      OpenAI            Claude               Gemini

                             |
                             v

                    +----------------+
                    |      RAG       |
                    +----------------+
```

---

# 3. System Components

## Frontend

Technology:

- NextJS 15
- React 19
- TypeScript
- TailwindCSS
- Zustand
- React Query

Responsibilities:

- Authentication UI
- Dashboard
- Speaking Interface
- AI Chat
- Reports

---

## Backend

Technology:

- NestJS
- TypeScript

Responsibilities:

- API
- Business Logic
- Authentication
- AI Orchestration
- Data Processing

---

## Database

Technology:

- PostgreSQL

Responsibilities:

- Persistent Data
- Analytics Data
- User Data

---

## Cache Layer

Technology:

- Redis

Responsibilities:

- Session Cache
- AI Response Cache
- Rate Limiting
- Queue Backend

---

## Queue Layer

Technology:

- BullMQ

Responsibilities:

- Audio Processing
- AI Tasks
- Email Jobs
- Analytics Jobs

---

# 4. Monorepo Structure

```text
ai-english-platform/

apps/
│
├── web
├── api
├── admin
│
packages/
│
├── ui
├── types
├── config
├── eslint-config
│
services/
│
├── ai-service
├── speech-service
├── notification-service
│
infra/
│
├── docker
├── kubernetes
├── argocd
│
prisma/
│
docs/
```

---

# 5. Frontend Architecture

## App Router

```text
app/

(auth)
(dashboard)
(ai)
(admin)

layout.tsx
page.tsx
```

---

## Feature Modules

```text
features/

auth/
user/
dashboard/
lesson/
speaking/
conversation/
subscription/
analytics/
```

---

## State Management

### React Query

Used For:

- API Data
- Server Cache

### Zustand

Used For:

- UI State
- Audio State
- Chat State

---

# 6. Backend Architecture

## Module Structure

```text
src/

modules/

auth/
users/
lessons/
conversations/
assessment/
subscription/
admin/

shared/

common/
database/
queue/
cache/
```

---

# 7. API Gateway

Responsibilities:

- Authentication
- Authorization
- Rate Limiting
- Request Validation

---

## Example

```http
POST /api/v1/auth/login

POST /api/v1/conversations

POST /api/v1/assessment

GET /api/v1/dashboard
```

---

# 8. Authentication Design

## Login Flow

```text
User
 ↓
Login
 ↓
Validate Credentials
 ↓
Generate JWT
 ↓
Generate Refresh Token
 ↓
Store Session
 ↓
Return Tokens
```

---

## JWT Payload

```json
{
  "sub": "user_id",
  "email": "user@email.com",
  "role": "user"
}
```

---

# 9. Authorization Design

Roles:

```text
guest
user
premium
admin
super_admin
```

---

## Guards

```typescript
JwtAuthGuard
RolesGuard
PremiumGuard
```

---

# 10. AI Architecture

## Multi-LLM Strategy

Supported Models:

- GPT-5
- Claude
- Gemini

---

## Routing Logic

```text
User Prompt
      ↓
 AI Router
      ↓
 -------------------
 |       |         |
OpenAI Claude Gemini
```

---

## Decision Matrix

| Task | Model |
|--------|--------|
| Grammar | GPT |
| Speaking Feedback | GPT |
| Long Context | Claude |
| Reasoning | Claude |
| Fast Responses | Gemini |

---

# 11. Conversation Engine

Conversation Components:

```text
Conversation
Message
Context
Memory
Feedback
```

---

## Context Builder

Input:

```text
User Profile
Conversation History
Current Lesson
Learning Goal
```

Output:

```text
Final Prompt
```

---

# 12. RAG Architecture

## Goal

Provide contextual learning materials.

---

## Flow

```text
User Question
      ↓
Embedding
      ↓
Vector Search
      ↓
Relevant Documents
      ↓
Prompt Builder
      ↓
LLM
      ↓
Response
```

---

## Knowledge Sources

- Grammar Database
- Vocabulary Library
- IELTS Materials
- Internal Courses

---

# 13. Speech Processing Pipeline

```text
Audio Upload
      ↓
Storage
      ↓
Speech To Text
      ↓
Transcript
      ↓
Pronunciation Analysis
      ↓
Feedback
```

---

## Supported Formats

- mp3
- wav
- m4a
- webm

---

# 14. Assessment Engine

Scoring Categories:

### Pronunciation

0-100

### Fluency

0-100

### Grammar

0-100

### Vocabulary

0-100

### Coherence

0-100

---

## Overall Score Formula

```text
Overall =
25% Pronunciation +
20% Fluency +
20% Grammar +
20% Vocabulary +
15% Coherence
```

---

# 15. Database Design Strategy

Database:

PostgreSQL

ORM:

Prisma

---

## Naming Convention

```text
snake_case
```

Examples:

```text
user_profiles
conversation_messages
assessment_results
```

---

# 16. Redis Strategy

## Cache Keys

```text
user:{id}

conversation:{id}

lesson:{id}
```

---

## TTL

| Data | TTL |
|--------|--------|
| Profile | 1 hour |
| Lesson | 6 hours |
| AI Response | 24 hours |

---

# 17. BullMQ Design

Queues:

```text
email_queue

speech_queue

assessment_queue

analytics_queue

notification_queue
```

---

## Example Job

```typescript
{
  userId: "123",
  audioUrl: "...",
  conversationId: "..."
}
```

---

# 18. File Storage

Recommended:

- AWS S3
- Cloudflare R2

Store:

- Audio Files
- Reports
- Exports

---

# 19. Observability

## Logging

Technology:

- Loki

---

## Metrics

Technology:

- Prometheus

Metrics:

- Request Count
- Error Rate
- AI Latency

---

## Monitoring

Technology:

- Grafana

---

# 20. Security Design

Authentication:

- JWT

Encryption:

- AES-256

Transport:

- TLS 1.3

---

## Rate Limiting

```text
100 requests/minute
```

---

## OWASP Controls

- XSS Protection
- CSRF Protection
- SQL Injection Prevention
- Input Validation

---

# 21. Kubernetes Architecture

```text
Namespace

frontend
backend
redis
postgres
monitoring
```

---

## Deployments

```text
frontend-deployment

api-deployment

worker-deployment
```

---

## Services

```text
ClusterIP

LoadBalancer

Ingress
```

---

# 22. CI/CD Pipeline

GitHub Actions

```text
Push
 ↓
Test
 ↓
Build
 ↓
Docker Image
 ↓
Registry
 ↓
ArgoCD Sync
 ↓
Kubernetes Deploy
```

---

# 23. ArgoCD GitOps

Repository:

```text
infra/kubernetes
```

ArgoCD watches:

```text
deployment.yaml
service.yaml
ingress.yaml
```

---

# 24. Scalability Strategy

Horizontal Scaling:

```text
Frontend Pods

2 → 20
```

```text
API Pods

2 → 50
```

```text
Worker Pods

2 → 100
```

---

# 25. Disaster Recovery

Database:

- Daily Backup
- PITR

Storage:

- Multi-region replication

Redis:

- Redis Sentinel

---

# 26. Performance Targets

API Latency:

< 500ms

AI Response:

< 5s

Page Load:

< 2s

Availability:

99.9%

---

# 27. Future Evolution

Phase 1:

Modular Monolith

↓

Phase 2:

Domain Services

↓

Phase 3:

Microservices

↓

Phase 4:

Multi-region Global Platform

---

# Conclusion

Kiến trúc này được thiết kế để hỗ trợ:

- 100,000+ concurrent users
- Multi-LLM orchestration
- AI Speaking Assessment
- RAG Knowledge Base
- Kubernetes Native Deployment
- Enterprise-grade Scalability

với nền tảng:

- NestJS
- NextJS
- PostgreSQL
- Prisma
- Redis
- BullMQ
- OpenAI
- Claude
- Gemini
- Kubernetes
- ArgoCD