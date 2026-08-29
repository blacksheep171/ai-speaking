# 06-SYSTEM-ARCHITECTURE.md
# System Architecture Design

## AI English Speaking Platform

**Version:** 1.0  
**Architecture Style:** Modular Monolith → Microservices Ready  
**Deployment Model:** Cloud Native  
**Infrastructure:** Kubernetes + ArgoCD

---

# 1. Overview

## Purpose

Tài liệu mô tả kiến trúc tổng thể của hệ thống AI English Speaking Platform.

Mục tiêu:

- High Availability
- High Scalability
- Fault Tolerance
- Cloud Native
- AI First
- Enterprise Ready

---

# 2. Architecture Principles

## Domain Driven Design (DDD)

Chia hệ thống theo domain business.

```text
Auth
User
Course
Lesson
Conversation
Assessment
AI
Payment
Analytics
Notification
```

---

## Modular Architecture

Mỗi domain độc lập.

```text
Module
Service
Repository
Controller
DTO
Entity
```

---

## API First

Toàn bộ giao tiếp thông qua API.

```text
REST API
Webhook
Internal Events
```

---

## Cloud Native

Designed for Kubernetes.

---

# 3. High Level Architecture

```text
+---------------------------------------------------+
|                     Frontend                      |
|                     NextJS                        |
+-------------------------+-------------------------+
                          |
                          |
                          v
+---------------------------------------------------+
|                    API Gateway                    |
|                      NestJS                       |
+---------------------------------------------------+
                          |
-----------------------------------------------------
|         |          |          |         |          |
v         v          v          v         v          v

Auth     User      Course    Lesson   Payment     AI
Module   Module    Module    Module   Module     Module

                          |
                          |
                          v

+---------------------------------------------------+
|                    PostgreSQL                     |
+---------------------------------------------------+

                          |
                          v

+---------------------------------------------------+
|                      Redis                        |
+---------------------------------------------------+

                          |
                          v

+---------------------------------------------------+
|                      BullMQ                       |
+---------------------------------------------------+

                          |
                          v

+---------------------------------------------------+
|                AI Provider Layer                  |
+---------------------------------------------------+

       OpenAI       Claude       Gemini
```

---

# 4. System Layers

```text
Presentation Layer

Application Layer

Domain Layer

Infrastructure Layer

External Services Layer
```

---

# 5. Frontend Architecture

Technology:

```text
NextJS
React
TypeScript
TailwindCSS
React Query
Zustand
```

---

## Responsibilities

### UI Rendering

Pages

Components

Layouts

---

### State Management

Client State

Server State

---

### Authentication

JWT Storage

Session Management

---

### AI Interaction

Voice Recording

Chat Interface

Speaking Practice

---

# 6. Frontend Folder Structure

```text
apps/web

src/

app/

components/

features/

hooks/

services/

store/

types/

utils/
```

---

# 7. Backend Architecture

Technology:

```text
NestJS
TypeScript
Prisma
```

---

## Design Pattern

```text
Controller

Service

Repository

DTO

Entity

Guard

Interceptor
```

---

# 8. Backend Module Structure

```text
src/

modules/

auth/

users/

courses/

lessons/

conversations/

assessments/

subscriptions/

payments/

analytics/

notifications/

admin/

ai/
```

---

# 9. API Gateway Layer

Responsibilities:

- Authentication
- Authorization
- Validation
- Rate Limiting
- Logging
- Monitoring

---

## Request Flow

```text
Client
 ↓
API Gateway
 ↓
Business Module
 ↓
Database
 ↓
Response
```

---

# 10. Domain Architecture

## Auth Domain

Responsibilities:

```text
Register

Login

JWT

Refresh Token

Email Verification
```

---

## User Domain

Responsibilities:

```text
Profiles

Settings

Learning Goals
```

---

## Course Domain

Responsibilities:

```text
Courses

Categories

Enrollments
```

---

## Lesson Domain

Responsibilities:

```text
Lessons

Content

Progress Tracking
```

---

## Conversation Domain

Responsibilities:

```text
AI Conversations

Messages

Context
```

---

## Assessment Domain

Responsibilities:

```text
Speaking Evaluation

Feedback

Scoring
```

---

## Subscription Domain

Responsibilities:

```text
Plans

Subscriptions

Quota Management
```

---

## Payment Domain

Responsibilities:

```text
Billing

Invoices

Payment Processing
```

---

## Analytics Domain

Responsibilities:

```text
Reports

Statistics

Dashboards
```

---

# 11. Database Architecture

Primary Database:

```text
PostgreSQL
```

ORM:

```text
Prisma
```

---

## Database Responsibilities

```text
Users

Courses

Lessons

Assessments

Payments

Analytics
```

---

# 12. PostgreSQL Design

## Read / Write Strategy

```text
Primary Database

Read Replicas
```

---

## Connection Pooling

```text
PgBouncer
```

---

## Backup

```text
Daily Backup

Point In Time Recovery
```

---

# 13. Cache Architecture

Technology:

```text
Redis
```

---

## Cached Data

```text
User Profile

Course Data

Lesson Data

AI Responses

Session Data
```

---

## Example

```text
user:123

lesson:456

conversation:789
```

---

# 14. Queue Architecture

Technology:

```text
BullMQ
```

---

## Queue Types

```text
speech-processing

assessment-processing

email-notification

analytics-processing

ai-background-jobs
```

---

## Processing Flow

```text
API
 ↓
Queue
 ↓
Worker
 ↓
Database
```

---

# 15. File Storage Architecture

Recommended:

```text
AWS S3

Cloudflare R2
```

---

## Stored Assets

```text
Audio Files

Reports

Exports

Avatars
```

---

# 16. AI Layer Architecture

Components:

```text
AI Gateway

AI Router

Prompt Builder

Memory Engine

RAG Engine

Assessment Engine
```

---

## AI Request Flow

```text
User Message
 ↓
Prompt Builder
 ↓
AI Router
 ↓
Provider
 ↓
Response
```

---

# 17. AI Provider Layer

Supported Providers:

```text
OpenAI

Claude

Gemini
```

---

## Provider Selection

Grammar

```text
GPT
```

---

Long Context

```text
Claude
```

---

Low Cost Tasks

```text
Gemini
```

---

# 18. RAG Architecture

Purpose:

Provide contextual knowledge.

---

## Knowledge Sources

```text
Grammar

Vocabulary

IELTS Materials

Business English

Internal Courses
```

---

## Flow

```text
Question
 ↓
Embedding
 ↓
Vector Search
 ↓
Relevant Documents
 ↓
Prompt
 ↓
LLM
```

---

# 19. Event Driven Architecture

Internal Events

```text
USER_REGISTERED

LESSON_COMPLETED

ASSESSMENT_CREATED

SUBSCRIPTION_ACTIVATED

PAYMENT_SUCCESSFUL
```

---

## Event Flow

```text
Service
 ↓
Event
 ↓
Subscribers
 ↓
Actions
```

---

# 20. Notification Architecture

Channels

```text
Email

Push Notification

In-App Notification
```

---

## Providers

```text
SendGrid

AWS SES
```

---

# 21. Security Architecture

Authentication

```text
JWT
```

---

Authorization

```text
RBAC
```

---

Encryption

```text
TLS 1.3

AES-256
```

---

## Security Layers

```text
WAF

Rate Limiter

Input Validation

Output Sanitization
```

---

# 22. Monitoring Architecture

Metrics

```text
Prometheus
```

---

Dashboards

```text
Grafana
```

---

Logs

```text
Loki
```

---

Tracing

```text
OpenTelemetry
```

---

# 23. Logging Architecture

Application Logs

```text
NestJS Logger
```

---

Structured Logs

```json
{
  "timestamp": "",
  "service": "",
  "level": "",
  "message": ""
}
```

---

# 24. Kubernetes Architecture

Namespaces

```text
frontend

backend

database

monitoring

argocd
```

---

Deployments

```text
frontend

api

workers
```

---

Services

```text
ClusterIP

LoadBalancer

Ingress
```

---

# 25. Horizontal Scaling

Frontend

```text
2 → 20 Pods
```

---

API

```text
2 → 50 Pods
```

---

Workers

```text
2 → 100 Pods
```

---

# 26. Disaster Recovery

Database

```text
PITR

Daily Backups
```

---

Storage

```text
Cross Region Replication
```

---

Redis

```text
Redis Sentinel
```

---

# 27. CI/CD Architecture

GitHub

↓  

GitHub Actions

↓  

Docker Build

↓  

Container Registry

↓  

ArgoCD

↓  

Kubernetes

---

# 28. Environment Strategy

Development

```text
Local
```

---

Testing

```text
QA
```

---

Staging

```text
Pre-Production
```

---

Production

```text
Production Cluster
```

---

# 29. Scalability Targets

Concurrent Users

```text
100,000+
```

---

Daily Requests

```text
10 Million+
```

---

AI Requests

```text
1 Million+/Day
```

---

# 30. Availability Targets

API Availability

```text
99.9%
```

---

Database Availability

```text
99.95%
```

---

AI Layer Availability

```text
99.9%
```

---

# 31. Future Architecture Evolution

Phase 1

```text
Modular Monolith
```

↓

Phase 2

```text
Domain Services
```

↓

Phase 3

```text
Microservices
```

↓

Phase 4

```text
Multi Region Deployment
```

↓

Phase 5

```text
Global AI Platform
```

---

# 32. System Architecture Diagram

```text
                     +----------------+
                     |     NextJS     |
                     +--------+-------+
                              |
                              v
                  +-----------+------------+
                  |       API Gateway      |
                  +-----------+------------+
                              |
    ----------------------------------------------------
    |          |          |          |          |       |
    v          v          v          v          v       v

  Auth       User      Course    Lesson    AI     Payment

                              |
                              v

                     +----------------+
                     | PostgreSQL     |
                     +----------------+

                              |
                              v

                     +----------------+
                     | Redis          |
                     +----------------+

                              |
                              v

                     +----------------+
                     | BullMQ         |
                     +----------------+

                              |
                              v

                 ----------------------------
                 |            |             |
                 v            v             v

              OpenAI      Claude       Gemini
```

---

# Conclusion

System Architecture được thiết kế dựa trên:

- Domain Driven Design
- Clean Architecture
- Cloud Native Principles
- AI First Design
- Kubernetes Native Deployment

Đảm bảo:

- High Scalability
- High Availability
- Fault Tolerance
- Security
- Maintainability
- Cost Optimization

Tech Stack:

- NextJS
- NestJS
- PostgreSQL
- Prisma
- Redis
- BullMQ
- OpenAI
- Claude
- Gemini
- Kubernetes
- ArgoCD