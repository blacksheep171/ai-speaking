# PROJECT.md
# AI English Speaking Platform

**Version:** 1.0
**Status:** Active — MVP Development
**Initialized:** 2026-08-29
**Source:** Ingested from docs/PRD (25 documents)

---

## Vision

Build an AI-powered English speaking practice platform that gives every learner access to a 1:1 tutor experience at a fraction of the cost — through real-time pronunciation analysis, adaptive AI conversation, and personalized learning roadmaps.

## Product Goals

- Help users practice speaking anytime, anywhere
- Provide instant AI feedback on pronunciation, fluency, grammar, vocabulary, coherence
- Simulate realistic conversation scenarios (job interviews, IELTS, daily chat, business)
- Personalize the learning journey based on performance data
- Scale to 100,000+ concurrent users

## Business Goals (Year 1)

- 50,000 registered users
- 10,000 MAU
- 2,000 paid subscribers
- $100,000 ARR

---

## Tech Stack (LOCKED)

### Monorepo
- Tool: Turborepo + pnpm workspaces
- Structure: apps/ + packages/ + infrastructure/

### Frontend (apps/web, apps/admin)
- NextJS 15 + React 19 + TypeScript
- TailwindCSS
- Zustand (UI state) + React Query (server state)

### Backend (apps/api)
- NestJS + TypeScript
- Design Pattern: Controller → Service → Repository → DTO → Entity
- Architecture: Modular Monolith → Microservices Ready

### Workers (apps/workers)
- NestJS + BullMQ
- Queues: speech-processing, assessment-processing, email-notification, analytics-processing, ai-background-jobs

### AI Gateway (apps/ai-gateway)
- Multi-LLM abstraction: OpenAI + Claude + Gemini
- Fallback: GPT → Claude → Gemini → Internal message

### Database
- PostgreSQL 16 + Prisma ORM
- Naming: snake_case, UUID primary keys, UTC timestamps
- Extensions: uuid-ossp, pgvector, pg_trgm

### Cache
- Redis (session, AI responses, rate limiting)
- TTL: Profile 1h | Lesson 6h | AI Response 24h

### Queue
- BullMQ on Redis

### File Storage
- AWS S3 or Cloudflare R2 (audio files, reports, avatars)

### AI Providers
- OpenAI: GPT-5, GPT-5 Mini (grammar, pronunciation, feedback)
- Claude: Sonnet, Opus (long context, study plans, IELTS)
- Gemini: Pro, Flash (quick chat, low-cost tasks, daily practice)
- Vector DB: pgvector (MVP) → Pinecone (scale)
- Embedding: text-embedding-3-large / text-embedding-3-small

### Infrastructure
- Docker + Kubernetes + ArgoCD (GitOps)
- CI/CD: GitHub Actions
- Monitoring: Prometheus + Grafana + Loki

### Security
- Auth: JWT + Refresh Token
- Encryption: AES-256 + TLS 1.3
- OWASP Top 10 compliance
- Rate Limiting: 100 req/min

---

## User Roles

| Role | Capabilities |
|---|---|
| Guest | Browse, view pricing/courses |
| Free User | 10 AI conversations/day, basic reports |
| Premium User | Unlimited conversations, advanced analytics, personalized roadmap |
| Admin | Full management: users, courses, analytics, prompts, subscriptions |

---

## Core Domains

1. **Auth** — Register, Login, JWT, Refresh Token, Email Verification, Google/Apple OAuth
2. **User** — Profiles, settings, learning goals, devices
3. **Course** — Course catalog, categories, enrollments
4. **Lesson** — Lesson content (TEXT, VIDEO, QUIZ, SPEAKING, VOCABULARY), progress tracking
5. **Conversation** — AI multi-turn conversations, context memory, audio recording
6. **Assessment** — Pronunciation/Fluency/Grammar/Vocabulary/Coherence scoring
7. **AI Gateway** — Multi-LLM routing, prompt templates, cost tracking
8. **RAG** — Knowledge documents, embeddings, retrieval logs
9. **Subscription** — Plans (Free/Premium/Corporate), quota management
10. **Payment** — Billing, invoices, payment processing
11. **Gamification** — Achievements, streaks, XP
12. **Notification** — Email, push, in-app (via SendGrid/AWS SES)
13. **Analytics** — Events, daily statistics, dashboards
14. **Admin** — Audit logs, admin actions

---

## Assessment Scoring (LOCKED)

```
Overall = 25% Pronunciation + 20% Fluency + 20% Grammar + 20% Vocabulary + 15% Coherence
Scale: 0–100 per dimension
```

---

## Non-Functional Requirements (LOCKED)

| NFR | Target |
|---|---|
| Availability | 99.9% |
| Concurrent Users | 100,000 |
| API Response | < 500ms |
| AI Response | < 5 seconds |
| Rate Limit | 100 req/min |

---

## Out of Scope (MVP)

- Live Teacher Classes
- Marketplace
- Offline Courses
- Native Mobile Apps (iOS/Android)
- Video Calling
- Corporate LMS

---

## Success Criteria (MVP Done When)

- User can register and verify email
- User can have a voice conversation with AI
- AI assesses pronunciation and gives feedback
- Dashboard displays progress
- Payment/subscription works
- System handles 10,000 concurrent users under load

<decisions>
- LOCKED: Turborepo + pnpm as monorepo toolchain
- LOCKED: NestJS backend + NextJS 15 frontend
- LOCKED: PostgreSQL 16 + Prisma ORM
- LOCKED: UUID primary keys, snake_case naming, UTC timestamps
- LOCKED: Multi-LLM strategy (OpenAI + Claude + Gemini) with fallback chain
- LOCKED: Assessment formula: 25% Pronunciation + 20% Fluency + 20% Grammar + 20% Vocabulary + 15% Coherence
- LOCKED: BullMQ for all async job processing
- LOCKED: Redis for caching and rate limiting
- LOCKED: JWT + Refresh Token authentication
- LOCKED: pgvector for MVP vector storage
</decisions>
