# INGEST SYNTHESIS
# AI English Speaking Platform — docs/PRD (25 docs)

**Synthesized:** 2026-08-29
**Mode:** new
**Source Docs:** 25 PRD documents
**Precedence Used:** PRD (homogeneous set)
**Conflicts:** 0 blockers, 0 warnings, 4 auto-resolved (minor naming variants)

---

## Project Identity

**Product Name:** AI English Speaking Platform
**Project Alias:** ai-speaking
**Target Users:** Students (18-25), Working Professionals (25-40), Beginners (18-50)
**MVP Target:** Production-ready v1.0
**Revenue Target Year 1:** $100,000 ARR
**Tech Stack Runtime:** Node.js + TypeScript (full-stack)

---

## Core Domain Summary

The platform is an AI-powered English speaking practice system with:

1. Speaking Engine — Voice recording → STT → Pronunciation/Fluency/Grammar/Vocabulary/Coherence assessment
2. AI Conversation — Multi-turn chat with configurable AI roles (Teacher, Interviewer, Friend, IELTS Examiner, etc.)
3. AI Tutor & RAG — Retrieval-augmented knowledge (Grammar DB, IELTS materials, Vocabulary library, internal courses)
4. Personalized Learning — Assessment-driven recommendations, adaptive roadmap generation
5. Gamification — Achievements, streaks, XP, leaderboard
6. Subscription & Billing — Free (10 sessions/day), Premium ($9.99/mo, $79/yr), Corporate (custom)
7. Admin Portal — Course management, user management, analytics, prompt management

---

## Technology Decisions (LOCKED)

- Monorepo Tool: Turborepo + pnpm
- Frontend Framework: NextJS 15 + React 19 + TypeScript + TailwindCSS
- State Management: Zustand (UI) + React Query (server)
- Backend Framework: NestJS + TypeScript
- Database: PostgreSQL 16 + Prisma ORM
- Primary Key Type: UUID
- DB Naming Convention: snake_case
- Timezone: UTC
- Cache: Redis
- Queue: BullMQ
- AI Providers: OpenAI (GPT-5/GPT-5 Mini), Claude (Sonnet/Opus), Gemini (Pro/Flash)
- Vector DB MVP: PostgreSQL + pgvector
- Vector DB Scale: Pinecone
- File Storage: AWS S3 / Cloudflare R2
- Auth Mechanism: JWT + Refresh Token
- Infrastructure: Docker + Kubernetes + ArgoCD
- Logging: Loki
- Metrics: Prometheus + Grafana
- CI/CD: GitHub Actions

---

## Non-Functional Requirements (LOCKED)

- Availability: 99.9% uptime
- Scalability: 100,000 concurrent users
- API Response Time: < 500ms
- AI Response Time: < 5 seconds
- Security: OWASP Top 10, AES-256, TLS 1.3, Rate Limiting 100 req/min
- Reliability: No single point of failure

---

## Assessment Scoring Formula (LOCKED)

Overall Score = 25% Pronunciation + 20% Fluency + 20% Grammar + 20% Vocabulary + 15% Coherence

---

## AI Routing Matrix (LOCKED)

- Grammar Correction → GPT
- Pronunciation Feedback → GPT
- Long Conversation / Study Plan → Claude
- IELTS Feedback → Claude
- Quick Chat / Daily Practice → Gemini
- Knowledge Search → GPT

---

## Out of Scope (MVP)

- Live Teacher Classes, Marketplace, Offline Courses
- Native Mobile Apps (iOS/Android)
- Video Calling, Corporate LMS
