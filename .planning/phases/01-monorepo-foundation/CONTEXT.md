# Phase 1: Monorepo Foundation & Infrastructure — Context

**Gathered:** 2026-08-29
**Status:** Ready for planning
**Source:** Synthesized from docs/PRD (25 documents, ingest 2026-08-29)

<domain>
## Phase Boundary

Phase 1 establishes the complete structural and infrastructure foundation that ALL subsequent phases depend on. No application logic, no API endpoints, no UI screens. This phase delivers:

1. **Turborepo monorepo structure** — all apps and packages scaffolded, build pipeline working
2. **Shared packages** — types, config, logger, utils, prisma, ai-sdk, auth packages (minimal but usable)
3. **Complete Prisma schema** — all 30+ tables from ERD, PostgreSQL extensions, migrations, seed
4. **Docker local dev environment** — postgres + redis running, health checks passing
5. **CI/CD foundation** — GitHub Actions lint/typecheck/test pipeline, pre-commit hooks

**What is NOT in scope for Phase 1:**
- No business logic (auth, courses, AI)
- No API endpoints
- No UI pages
- No actual AI calls
- No payment integration

**Done state:** A developer can clone the repo, run `docker compose up -d && pnpm install && pnpm db:migrate && pnpm dev` and see all apps start (even if showing a placeholder "coming soon" page), and `pnpm typecheck` + `pnpm lint` pass across the workspace.
</domain>

<decisions>
## Implementation Decisions

### Monorepo Tool
- Turborepo + pnpm workspaces (LOCKED — 25-MONOREPO-STRUCTURE.md)
- Root turbo.json with tasks: build, test, lint, typecheck, dev
- pnpm-workspace.yaml: packages: ["apps/*", "packages/*"]
- Node.js >= 18 required

### Apps to Scaffold (5 total)
- `apps/web` — NextJS 15 + React 19 + TypeScript + TailwindCSS (Student portal)
- `apps/api` — NestJS 10+ + TypeScript (REST API)
- `apps/workers` — NestJS + BullMQ (Background job processors)
- `apps/admin` — NextJS + TypeScript (Admin dashboard)
- `apps/ai-gateway` — NestJS + TypeScript (AI provider abstraction layer)

### Shared Packages (8 total)
- `packages/types` — shared DTOs, Interfaces, Enums (TypeScript, ESM)
- `packages/config` — ESLint config, Prettier config, TypeScript base configs, Jest config
- `packages/prisma` — schema.prisma, migrations, generated client, seed script
- `packages/logger` — Pino-based centralized logger with OpenTelemetry support
- `packages/ai-sdk` — Unified AI interface (stub providers: openai, claude, gemini)
- `packages/auth` — JWT utilities, RBAC helpers, Guard types
- `packages/utils` — Date helpers, string helpers, validation helpers, pagination
- `packages/ui` — Minimal shared UI component stubs (Button, Card, Input)

### Database Schema
- PostgreSQL 16 (LOCKED)
- Prisma ORM (LOCKED)
- Naming: snake_case (LOCKED), UUID PKs (LOCKED), UTC timestamps (LOCKED)
- Extensions required:
  - `uuid-ossp` — UUID generation
  - `vector` (pgvector) — embedding storage for RAG
  - `pg_trgm` — trigram search for text similarity
- All 30+ tables from 03-ERD.md must be in schema.prisma
- Migration via `prisma migrate dev`
- Seed script: creates default admin user, subscription plans, sample course

### Docker Environment
- `docker-compose.yml` at root for local development
- Services: postgres:16, redis:7-alpine
- Volumes: postgres_data, redis_data
- Environment: loaded from .env
- Health checks on both services
- Ports: postgres:5432, redis:6379

### TypeScript Configuration
- Root `tsconfig.base.json` with strict mode, ESNext target
- Each app/package extends base config
- Path aliases: `@ai-platform/*` → packages

### Code Quality
- ESLint: @typescript-eslint/eslint-plugin, eslint-config-prettier
- Prettier: single quotes, 2-space indent, trailing commas
- Husky + lint-staged: run lint+format on staged files
- Commitlint: conventional commits enforced

### CI/CD (GitHub Actions)
- `.github/workflows/ci.yml`: on push/PR to main
  - Job: install → lint → typecheck → test → build
- `.github/workflows/cd.yml`: stub only (ArgoCD deploy — Phase 7)
- Cache: pnpm store cache between runs

### Claude's Discretion
- Specific ESLint rule strictness (recommended + import rules)
- Logger transport config (console for dev, JSON for prod)
- Exact Pino version vs Winston vs custom
- README.md content and structure
- Whether to use `@repo/*` or `@ai-platform/*` as package namespace
- apps/web initial page content (placeholder or minimal landing)
</decisions>

<canonical_refs>
## Canonical References

**All downstream agents MUST read these before implementing Phase 1.**

### ERD / Database Schema
- `docs/PRD/03-ERD.md` — Complete entity-relationship diagram, all 30+ tables, columns, types, constraints, indexes, relationships

### Monorepo Structure
- `docs/PRD/25-MONOREPO-STRUCTURE.md` — Authoritative folder structure: apps/*, packages/*, infrastructure/*, naming conventions, dependency rules

### Technical Design
- `docs/PRD/02-TDD.md` — Tech stack decisions, module structure, BullMQ queue names, Redis TTL strategy, security design

### AI Architecture (for ai-sdk package stub)
- `docs/PRD/05-AI-ARCHITECTURE.md` — Multi-LLM provider pattern, routing matrix, fallback chain

### Infrastructure
- `docs/PRD/20-DEVOPS-INFRASTRUCTURE.md` — Kubernetes structure, CI/CD pipeline, Docker patterns

### Coding Standards
- `docs/PRD/23-CODING-STANDARDS.md` — Code style, naming conventions, module organization rules

### Planning Context
- `.planning/PROJECT.md` — Locked tech stack decisions
- `.planning/REQUIREMENTS.md` — REQ-INFRA-001
</canonical_refs>

<specifics>
## Specific Implementation Details

### Key pnpm-workspace.yaml
```yaml
packages:
  - "apps/*"
  - "packages/*"
```

### Key turbo.json tasks
```json
{
  "$schema": "https://turbo.build/schema.json",
  "tasks": {
    "build": { "dependsOn": ["^build"], "outputs": [".next/**", "dist/**"] },
    "dev": { "persistent": true, "cache": false },
    "lint": { "outputs": [] },
    "typecheck": { "outputs": [] },
    "test": { "outputs": ["coverage/**"] },
    "db:generate": { "outputs": ["packages/prisma/generated/**"] },
    "db:migrate": { "cache": false }
  }
}
```

### BullMQ Queue Names (from 02-TDD.md — to define as enums in packages/types)
- email_queue
- speech_queue
- assessment_queue
- analytics_queue
- notification_queue

### Redis Cache Key Patterns (from 02-TDD.md)
- `user:{id}` — TTL 1 hour
- `lesson:{id}` — TTL 6 hours
- `conversation:{id}` — TTL session
- `ai:response:{hash}` — TTL 24 hours

### Assessment Score Formula (in packages/types as constants)
```typescript
export const ASSESSMENT_WEIGHTS = {
  pronunciation: 0.25,
  fluency: 0.20,
  grammar: 0.20,
  vocabulary: 0.20,
  coherence: 0.15,
} as const;
```

### User Roles Enum (packages/types)
```typescript
export enum UserRole { GUEST, USER, PREMIUM, ADMIN, SUPER_ADMIN }
```

### Prisma Tables to Include (from 03-ERD.md)
Auth: users, user_profiles, user_settings, user_devices, refresh_tokens
Course: courses, course_categories, course_category_relations, lessons, lesson_contents
Enrollment: enrollments, lesson_progress, course_progress
Conversation: conversations, conversation_messages, conversation_contexts
Audio: audio_files, transcripts
Assessment: assessments, assessment_details, assessment_feedbacks, pronunciation_errors
AI: ai_models, ai_requests, ai_prompt_templates
RAG: knowledge_documents, document_chunks, document_embeddings, retrieval_logs
Subscription: subscription_plans, user_subscriptions
Payment: payments, invoices
Gamification: achievements, user_achievements, user_streaks
Notification: notifications, notification_templates
Analytics: analytics_events, daily_statistics
Admin: admin_audit_logs
Total: 30 tables
</specifics>

<deferred>
## Deferred to Later Phases

- Auth logic (JWT issuance, Guards) → Phase 2
- Course/lesson CRUD → Phase 3
- Speaking/assessment logic → Phase 4
- AI provider calls → Phase 5
- Payment processing → Phase 6
- Admin dashboard pages → Phase 7
- Kubernetes manifests → Phase 7
- Monitoring/alerting → Phase 7
- PgBouncer production config → Phase 7
</deferred>

---
*Phase: 01-monorepo-foundation*
*Context gathered: 2026-08-29*
