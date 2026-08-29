# ROADMAP.md
# AI English Speaking Platform — Development Roadmap

**Version:** 1.0
**Generated:** 2026-08-29
**Total Phases:** 7
**Target:** MVP v1.0 Production-Ready

---

## Overview

```
Phase 1 → Monorepo Foundation & Database Schema
Phase 2 → Authentication & User Management
Phase 3 → Core API & Course/Lesson Management
Phase 4 → Speaking Engine & Assessment
Phase 5 → AI Conversation & RAG
Phase 6 → Subscription, Payment & Gamification
Phase 7 → Admin Portal, Analytics & Production Hardening
```

---

# Phase 1: Monorepo Foundation & Infrastructure

**Goal:** Establish the Turborepo monorepo, shared packages, Docker environment, and complete database schema.

**Status:** NOT STARTED

## Deliverables

### 1.1 Monorepo Setup
- Initialize Turborepo + pnpm workspaces
- Root: turbo.json, pnpm-workspace.yaml, package.json, .env.example
- Apps: web, api, workers, admin, ai-gateway (scaffolded, not implemented)
- Packages: ui, types, config, prisma, logger, ai-sdk, auth, utils

### 1.2 Shared Packages
- `packages/types`: DTOs, Interfaces, Enums (UserDto, CourseDto, AssessmentDto)
- `packages/config`: ESLint, Prettier, TypeScript, Jest shared configs
- `packages/logger`: Centralized logging (console + OpenTelemetry)
- `packages/utils`: Date helpers, string helpers, validation helpers

### 1.3 Database Schema (Prisma)
- All 30+ tables from ERD (03-ERD.md)
- PostgreSQL extensions: uuid-ossp, pgvector, pg_trgm
- Prisma schema, migrations, seed script
- Connection pooling config (PgBouncer)

### 1.4 Docker Environment
- docker-compose.yml for local dev: postgres, redis, (apps later)
- .env.example with all required variables
- Health checks for all services

### 1.5 CI/CD Foundation
- GitHub Actions: ci.yml (lint, typecheck, test), cd.yml stub
- Pre-commit hooks: lint-staged, prettier

## Requirements Addressed
- REQ-INFRA-001 (partial)

## Exit Criteria
- `pnpm install` succeeds workspace-wide
- `pnpm db:migrate` creates all tables
- Docker environment starts cleanly
- TypeScript compiles across all packages

---

# Phase 2: Authentication & User Management

**Goal:** Complete JWT auth system, email verification, Google/Apple OAuth, user profiles.

**Status:** NOT STARTED

## Deliverables

### 2.1 Auth Module (apps/api)
- POST /api/v1/auth/register — email + password
- POST /api/v1/auth/login — returns JWT + refresh token
- POST /api/v1/auth/refresh — refresh token rotation
- POST /api/v1/auth/logout — revoke refresh token
- POST /api/v1/auth/forgot-password
- POST /api/v1/auth/reset-password
- POST /api/v1/auth/verify-email
- POST /api/v1/auth/google — Google OAuth callback
- POST /api/v1/auth/apple — Apple OAuth callback

### 2.2 Auth Infrastructure
- JwtAuthGuard, RolesGuard, PremiumGuard
- JWT payload: { sub, email, role }
- JWT TTL: 15 min; Refresh Token TTL: 30 days
- refresh_tokens table with revocation
- user_devices tracking

### 2.3 Email Service
- BullMQ email_queue worker
- Transactional emails: verification, password reset, welcome
- Template: SendGrid or AWS SES

### 2.4 User Profile Module
- GET/PUT /api/v1/users/profile
- Avatar upload to S3/R2
- User settings (notification, dark_mode, timezone)
- English level + learning goal onboarding flow

### 2.5 Auth Frontend (apps/web)
- Register page
- Login page
- Email verification page
- Forgot/Reset password pages
- Google/Apple OAuth buttons
- NextAuth.js or custom JWT client-side handler

## Requirements Addressed
- REQ-AUTH-001, REQ-AUTH-002, REQ-AUTH-003, REQ-USER-001

## Exit Criteria
- User can register, verify email, login, logout
- JWT + refresh token cycle works
- Google OAuth flow works end-to-end
- Profile CRUD works

---

# Phase 3: Course & Lesson Management

**Goal:** Build course catalog, lesson content system, enrollment, and progress tracking.

**Status:** NOT STARTED

## Deliverables

### 3.1 Course Module (apps/api)
- GET /api/v1/courses — list all courses
- GET /api/v1/courses/:id — course detail
- POST /api/v1/courses/:id/enroll
- Course categories and relations

### 3.2 Lesson Module (apps/api)
- GET /api/v1/lessons/:id — lesson with content
- Content types: TEXT, VIDEO, QUIZ, SPEAKING, VOCABULARY
- JSONB content storage via lesson_contents

### 3.3 Progress Tracking
- POST /api/v1/lessons/:id/progress — mark lesson done
- GET /api/v1/courses/:id/progress — course completion %
- lesson_progress, course_progress tables

### 3.4 Course Frontend (apps/web)
- Course listing page with filters (level, category)
- Course detail page with lesson tree
- Lesson viewer: renders TEXT, VIDEO, QUIZ content
- Progress indicators

## Requirements Addressed
- REQ-COURSE-001

## Exit Criteria
- User can browse, enroll, and complete lessons
- Progress persists correctly
- Admin can create/edit courses and lessons (via API)

---

# Phase 4: Speaking Engine & Assessment

**Goal:** Voice recording pipeline, STT, 5-dimension assessment, feedback generation.

**Status:** NOT STARTED

## Deliverables

### 4.1 Voice Recording (Frontend)
- Browser MediaRecorder API → audio capture (webm/mp3)
- Upload to presigned S3/R2 URL
- Display recording timer and waveform visualizer

### 4.2 Speech Processing Queue
- BullMQ speech_queue worker
- Job: receive audio_file_id → STT → transcript
- STT provider: OpenAI Whisper API

### 4.3 Assessment Engine (apps/api + apps/workers)
- POST /api/v1/assessment — trigger assessment from conversation_id/audio_file_id
- BullMQ assessment_queue worker
- 5-dimension scoring via GPT-5:
  - Pronunciation (accuracy, stress, intonation, clarity) — 0–100
  - Fluency (pauses, rate, fillers, completion) — 0–100
  - Grammar (tenses, articles, prepositions, structure) — 0–100
  - Vocabulary (diversity, range, relevance, complexity) — 0–100
  - Coherence (logical flow, consistency, linking) — 0–100
  - Overall = 25% × P + 20% × F + 20% × G + 20% × V + 15% × C
- Store: assessments, assessment_details, assessment_feedbacks, pronunciation_errors

### 4.4 Feedback Frontend (apps/web)
- Pronunciation score radar chart
- Strengths/weaknesses/recommendations display
- Per-word pronunciation error highlights
- Speaking history timeline on dashboard

## Requirements Addressed
- REQ-SPEAKING-001, REQ-SPEAKING-002, REQ-ASSESS-001, REQ-DASHBOARD-001 (partial)

## Exit Criteria
- User records voice → upload → STT → assessment stored in DB
- All 5 dimension scores computed
- Feedback rendered on frontend
- Overall score formula correct per TDD

---

# Phase 5: AI Conversation & RAG Engine

**Goal:** Multi-turn AI conversation, AI roles, prompt templates, RAG knowledge retrieval.

**Status:** NOT STARTED

## Deliverables

### 5.1 AI Gateway (apps/ai-gateway)
- Unified provider interface: openAIProvider, claudeProvider, geminiProvider
- AI Router: route by task type per routing matrix
- Fallback chain: GPT → Claude → Gemini → static message
- Cost tracking per request → ai_requests table
- Quota check → integration with subscription module

### 5.2 Conversation Module (apps/api)
- POST /api/v1/conversations — start new conversation
- POST /api/v1/conversations/:id/messages — send message
- GET /api/v1/conversations/:id — get conversation with messages
- PATCH /api/v1/conversations/:id — update status (end conversation)
- Context Memory: Redis (short-term, session) + PostgreSQL (long-term history)
- Context Compression: auto-summarize old messages to reduce token usage

### 5.3 Prompt Template System
- ai_prompt_templates table with versioning (v1, v2, v3)
- Templates: conversation_teacher, conversation_interviewer, grammar_checker, pronunciation_feedback, ielts_examiner
- Rollback support

### 5.4 RAG Engine (apps/api + packages/ai-sdk)
- Document ingestion pipeline: parse → chunk (500 tokens, 50 overlap) → embed → store pgvector
- Knowledge sources: Grammar, Vocabulary, IELTS, Speaking Templates, Business English, Lessons
- Retrieval: embedding query → vector search → top-K results → context building
- Retrieval logging: retrieval_logs table

### 5.5 Conversation Frontend (apps/web)
- Chat interface: text + voice input, streaming response
- AI role selector (Teacher/Friend/Interviewer/etc.)
- Scenario selector (Job Interview, IELTS, Daily, etc.)
- Conversation history sidebar
- Real-time typing indicator

## Requirements Addressed
- REQ-CONV-001, REQ-AI-001, REQ-RAG-001

## Exit Criteria
- User can start a conversation, send text/voice, receive AI response
- AI routing works per matrix (GPT/Claude/Gemini)
- RAG retrieves relevant documents and includes in prompt
- Context persists across multi-turn conversation
- Token costs tracked

---

# Phase 6: Subscription, Payment, Gamification & Notifications

**Goal:** Monetization, quota enforcement, gamification loops, notification system.

**Status:** NOT STARTED

## Deliverables

### 6.1 Subscription Module (apps/api)
- GET /api/v1/subscriptions/plans — list plans
- POST /api/v1/subscriptions — create subscription
- Quota enforcement middleware (Free: 10 conv/day)
- user_subscriptions table management

### 6.2 Payment Module (apps/api)
- POST /api/v1/payments/checkout — create Stripe checkout session
- POST /api/v1/payments/webhook — handle Stripe events
- payments + invoices tables
- Auto-upgrade user role on successful payment

### 6.3 Gamification Module (apps/api)
- Streak tracking: update user_streaks on daily conversation completion
- Achievement unlocking: check conditions after key events
  - "First Conversation", "7-Day Streak", "30-Day Streak", "100 Conversations", "1000 Min Speaking"
- GET /api/v1/gamification/achievements — user achievements
- GET /api/v1/gamification/streak — current streak

### 6.4 Notification Module (apps/api + apps/workers)
- BullMQ notification_queue worker
- Email: daily reminder, streak alert, achievement unlock, payment receipt
- In-app: notifications table + real-time push via WebSocket or SSE
- Templates in notification_templates table

### 6.5 Recommendation Engine (apps/api)
- POST-assessment trigger → generate recommendations
- GET /api/v1/recommendations — personalized lesson list
- Based on: assessment history, weak skills, learning goal

### 6.6 Subscription Frontend (apps/web)
- Pricing page: Free vs Premium comparison
- Stripe checkout redirect
- Subscription status display in settings
- Usage quota indicator (sessions remaining)

## Requirements Addressed
- REQ-SUB-001, REQ-PAY-001, REQ-GAMIF-001, REQ-NOTIF-001, REQ-REC-001

## Exit Criteria
- Free user blocked after 10 conversations/day
- Stripe payment completes and upgrades plan
- Achievements unlock on correct events
- Daily reminder emails sent via BullMQ

---

# Phase 7: Admin Portal, Analytics, Testing & Production

**Goal:** Admin dashboard, analytics system, full test suite, Kubernetes deployment.

**Status:** NOT STARTED

## Deliverables

### 7.1 Admin Module (apps/admin — NextJS)
- User management: list, view, suspend, change role
- Course management: create/edit courses and lessons
- Prompt template management: create/update with version control
- Analytics dashboard: DAU, MAU, total conversations, revenue
- Subscription plan management
- Audit log viewer

### 7.2 Analytics Module (apps/api)
- Event tracking: analytics_events table (all key actions)
- Daily stats aggregation: daily_statistics table (via BullMQ analytics_queue)
- GET /api/v1/analytics/dashboard — admin KPIs
- GET /api/v1/users/dashboard — user-facing stats

### 7.3 Test Suite
- Unit tests: 70%+ coverage for all modules (Jest/Vitest)
- Integration tests: all REST API endpoints (Supertest)
- E2E tests: Playwright for register→speak→assess→subscribe flow
- Load test: k6 — 10,000 concurrent users target

### 7.4 Kubernetes Production Deployment
- Namespaces: frontend, backend, redis, postgres, monitoring
- Deployments: frontend-deployment, api-deployment, worker-deployment
- Services: ClusterIP (internal), LoadBalancer (public), Ingress
- HPA: auto-scale on CPU/memory
- ConfigMaps + Secrets management
- ArgoCD app definitions in infra/argocd/applications/

### 7.5 Observability
- Prometheus: request count, error rate, AI latency metrics
- Grafana dashboards: API health, AI cost, user activity
- Loki: centralized log aggregation from all pods
- Alerting: PagerDuty/Slack integration on error rate thresholds

### 7.6 Security Hardening
- OWASP Top 10 audit
- Secrets scanning (GitHub Actions)
- Rate limiting verification (100 req/min)
- TLS cert management (cert-manager in K8s)
- API security headers (Helmet.js)

## Requirements Addressed
- REQ-ADMIN-001, REQ-ANALYTICS-001, REQ-DASHBOARD-001, REQ-INFRA-001, REQ-SEC-001, REQ-TEST-001

## Exit Criteria
- Admin portal fully functional
- All 25 PRD requirements have corresponding tests
- Load test passes at 10,000 concurrent users
- System deployed to Kubernetes with ArgoCD sync
- Prometheus/Grafana dashboards live
- 99.9% uptime SLA met in staging

---

## Phase Completion Summary

| Phase | Description | Key Requirements |
|---|---|---|
| 1 | Monorepo Foundation | REQ-INFRA-001 |
| 2 | Auth & User Management | REQ-AUTH-*, REQ-USER-001 |
| 3 | Course & Lesson | REQ-COURSE-001 |
| 4 | Speaking Engine & Assessment | REQ-SPEAKING-*, REQ-ASSESS-001 |
| 5 | AI Conversation & RAG | REQ-CONV-001, REQ-AI-001, REQ-RAG-001 |
| 6 | Subscription, Payment, Gamification | REQ-SUB-001, REQ-PAY-001, REQ-GAMIF-001, REQ-NOTIF-001, REQ-REC-001 |
| 7 | Admin, Analytics, Production | REQ-ADMIN-001, REQ-ANALYTICS-001, REQ-INFRA-001, REQ-SEC-001, REQ-TEST-001 |

---

## Next Step

Run `/gsd:plan-phase 1` to begin detailed task breakdown for Phase 1.
