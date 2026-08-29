# 25-MONOREPO-STRUCTURE.md
# Enterprise Monorepo Architecture & Folder Structure

## AI English Speaking Platform

**Version:** 1.0  
**Architecture:** Monorepo  
**Monorepo Tool:** Turborepo  
**Package Manager:** pnpm  
**Backend:** NestJS  
**Frontend:** NextJS  
**Database:** PostgreSQL + Prisma  
**Cache:** Redis  
**Queue:** BullMQ  
**Infrastructure:** Kubernetes + ArgoCD

---

# 1. Overview

## Purpose

Monorepo giúp:

- Centralized Codebase
- Shared Libraries
- Consistent Standards
- Faster Development
- Easier Deployment
- Better Scalability

---

## Goals

```text
Single Source Of Truth

Code Reuse

Independent Deployments

Enterprise Scalability
```

---

# 2. Monorepo Architecture

```text
ai-english-platform/
│
├── apps/
├── packages/
├── infrastructure/
├── docs/
├── scripts/
├── .github/
├── turbo.json
├── pnpm-workspace.yaml
└── package.json
```

---

# 3. Root Folder Structure

```text
ai-english-platform
│
├── apps
├── packages
├── infrastructure
├── docs
├── scripts
├── .github
│
├── turbo.json
├── package.json
├── pnpm-workspace.yaml
├── .env.example
└── README.md
```

---

# 4. Apps Directory

Purpose:

```text
Deployable Applications
```

---

Structure:

```text
apps/
├── web
├── api
├── workers
├── admin
└── ai-gateway
```

---

# 5. apps/web

Framework:

```text
NextJS 15
```

---

Responsibilities:

```text
Student Portal

Course Learning

Speaking Practice

Subscriptions
```

---

Structure:

```text
apps/web
│
├── src
├── public
├── tests
└── package.json
```

---

# 6. apps/admin

Framework:

```text
NextJS
```

---

Responsibilities:

```text
Admin Dashboard

Analytics

Course Management

User Management
```

---

# 7. apps/api

Framework:

```text
NestJS
```

---

Responsibilities:

```text
REST APIs

Business Logic

Authentication

Billing
```

---

# 8. apps/workers

Framework:

```text
NestJS Workers
```

---

Responsibilities:

```text
BullMQ Jobs

Email Processing

AI Processing

Analytics
```

---

# 9. apps/ai-gateway

Purpose:

```text
AI Provider Abstraction
```

---

Responsibilities:

```text
OpenAI

Claude

Gemini

Fallback Logic
```

---

# 10. Backend Architecture

```text
api
│
├── modules
├── common
├── config
├── infrastructure
└── main.ts
```

---

# 11. NestJS Modules

```text
auth

users

courses

lessons

speaking

assessment

subscription

analytics

notifications
```

---

# 12. Module Structure

```text
users
│
├── controllers
├── services
├── repositories
├── dto
├── entities
├── events
└── users.module.ts
```

---

# 13. Frontend Architecture

```text
web
│
├── app
├── components
├── hooks
├── services
├── stores
├── lib
└── styles
```

---

# 14. NextJS App Router Structure

```text
app
│
├── (auth)
├── dashboard
├── courses
├── speaking
├── settings
└── subscription
```

---

# 15. Shared Packages

Purpose:

```text
Reusable Libraries
```

---

Structure:

```text
packages/
```

---

# 16. Package Overview

```text
packages
├── ui
├── types
├── config
├── prisma
├── logger
├── ai-sdk
├── auth
└── utils
```

---

# 17. packages/ui

Contains:

```text
Buttons

Forms

Modals

Cards

Tables
```

---

Used By:

```text
web

admin
```

---

# 18. packages/types

Contains:

```text
DTOs

Interfaces

Enums

API Types
```

---

Example:

```typescript
UserDto

CourseDto

AssessmentDto
```

---

# 19. packages/config

Contains:

```text
ESLint

Prettier

TypeScript

Jest
```

---

# 20. packages/prisma

Purpose:

```text
Database Layer
```

---

Contains:

```text
schema.prisma

migrations

client
```

---

# 21. Prisma Structure

```text
packages/prisma
│
├── schema.prisma
├── migrations
├── seed.ts
└── generated
```

---

# 22. packages/logger

Purpose:

```text
Centralized Logging
```

---

Supports:

```text
Console

Grafana

OpenTelemetry
```

---

# 23. packages/ai-sdk

Purpose:

```text
Unified AI Interface
```

---

Providers:

```text
OpenAI

Claude

Gemini
```

---

# 24. AI SDK Structure

```text
providers
├── openai.provider.ts
├── claude.provider.ts
└── gemini.provider.ts
```

---

# 25. AI Gateway Pattern

```text
Application
 ↓
AI SDK
 ↓
Provider
 ↓
Response
```

---

# 26. packages/auth

Contains:

```text
JWT

RBAC

Permissions

Guards
```

---

# 27. packages/utils

Contains:

```text
Date Helpers

String Helpers

Validation Helpers
```

---

# 28. Infrastructure Folder

Purpose:

```text
Deployment Assets
```

---

Structure:

```text
infrastructure
├── terraform
├── kubernetes
├── argocd
└── monitoring
```

---

# 29. Terraform Structure

```text
terraform
├── modules
├── dev
├── staging
└── prod
```

---

# 30. Kubernetes Structure

```text
kubernetes
├── backend
├── frontend
├── workers
├── redis
└── monitoring
```

---

# 31. Kubernetes Resources

```text
Deployment

Service

Ingress

ConfigMap

Secret

HPA
```

---

# 32. ArgoCD Structure

```text
argocd
├── applications
└── projects
```

---

# 33. Monitoring Structure

```text
monitoring
├── prometheus
├── grafana
└── loki
```

---

# 34. Documentation Structure

```text
docs
├── PRD
├── ADR
├── API
├── Architecture
└── Runbooks
```

---

# 35. Architecture Decision Records

Folder:

```text
docs/ADR
```

---

Examples:

```text
ADR-001-use-prisma

ADR-002-use-bullmq

ADR-003-use-turborepo
```

---

# 36. Scripts Folder

Contains:

```text
Seed Scripts

Migration Scripts

Deployment Scripts

Utility Scripts
```

---

# 37. GitHub Structure

```text
.github
├── workflows
├── templates
└── actions
```

---

# 38. GitHub Actions

Workflows:

```text
ci.yml

cd.yml

security.yml

release.yml
```

---

# 39. Turborepo Configuration

Tasks:

```text
build

test

lint

typecheck
```

---

Example:

```json
{
  "tasks": {
    "build": {},
    "test": {}
  }
}
```

---

# 40. Package Dependency Rules

Allowed:

```text
Apps → Packages
```

---

Forbidden:

```text
Package → App
```

---

# 41. Layer Dependency Rules

```text
Presentation
 ↓
Application
 ↓
Domain
 ↓
Infrastructure
```

---

Never:

```text
Infrastructure → Presentation
```

---

# 42. CI/CD Flow

```text
Commit
 ↓
GitHub Actions
 ↓
Docker Build
 ↓
ECR
 ↓
ArgoCD
 ↓
Kubernetes
```

---

# 43. Scaling Strategy

Scale Independently:

```text
Frontend

Backend

Workers

AI Gateway
```

---

# 44. Team Ownership

Example:

```text
Frontend Team

Backend Team

AI Team

DevOps Team
```

---

# 45. Performance Guidelines

Avoid:

```text
Huge Shared Packages

Circular Dependencies
```

---

Use:

```text
Caching

Lazy Loading

Code Splitting
```

---

# 46. Security Guidelines

Never Store:

```text
Secrets In Repository
```

---

Use:

```text
AWS Secrets Manager
```

---

# 47. Enterprise Standards

Requirements:

```text
Code Reviews

CI Checks

Security Scans

Documentation
```

---

# 48. Scalability Targets

Support:

```text
10 Million Users

1 Million DAU

100,000 Concurrent Users
```

---

Architecture:

```text
Horizontal Scaling

Microservice-Ready

Cloud Native
```

---

# 49. Complete Folder Tree

```text
ai-english-platform
│
├── apps
│   ├── web
│   ├── admin
│   ├── api
│   ├── workers
│   └── ai-gateway
│
├── packages
│   ├── ui
│   ├── types
│   ├── prisma
│   ├── auth
│   ├── ai-sdk
│   ├── logger
│   ├── utils
│   └── config
│
├── infrastructure
│   ├── terraform
│   ├── kubernetes
│   ├── argocd
│   └── monitoring
│
├── docs
├── scripts
├── .github
│
├── turbo.json
├── pnpm-workspace.yaml
├── package.json
└── README.md
```

---

# 50. Final Architecture Summary

Technology Stack:

```text
Frontend:
- NextJS
- TailwindCSS
- React Query
- Zustand

Backend:
- NestJS
- Prisma
- PostgreSQL
- Redis
- BullMQ

AI:
- OpenAI
- Claude
- Gemini
- RAG

Infrastructure:
- Kubernetes
- ArgoCD
- GitHub Actions
- Terraform

Observability:
- Grafana
- Prometheus
- Loki
- OpenTelemetry
```

---

# Conclusion

Monorepo Structure được thiết kế theo tiêu chuẩn Enterprise SaaS nhằm:

- Dễ mở rộng
- Dễ bảo trì
- Tối ưu hiệu suất phát triển
- Hỗ trợ nhiều team làm việc song song
- Tích hợp AI-native architecture
- Scale tới hàng triệu người dùng

Kiến trúc này là nền tảng phù hợp để xây dựng một AI English Speaking Platform quy mô lớn với:

- AI Tutor
- Speaking Assessment
- Learning Paths
- Recommendation Engine
- Gamification
- Analytics
- Subscription Billing
- Cloud-Native Infrastructure