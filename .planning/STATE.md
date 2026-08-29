# STATE.md
# Project Memory — AI English Speaking Platform

**Last Updated:** 2026-08-29
**Current Phase:** 1 — Monorepo Foundation & Infrastructure
**Next Action:** /gsd:execute-phase 1

---

## Project Status

| Field | Value |
|---|---|
| Overall Status | PHASE 1 PLANNED — READY TO EXECUTE |
| Current Phase | 1 (Monorepo Foundation & Infrastructure) |
| Phase 1 Status | PLANNED |
| Active Phase | 1 |
| Last Completed Phase | None |
| Blocking Issues | None |

---

## Phases

| Phase | Name | Status |
|---|---|---|
| 1 | Monorepo Foundation & Infrastructure | **PLANNED** |
| 2 | Authentication & User Management | NOT STARTED |
| 3 | Course & Lesson Management | NOT STARTED |
| 4 | Speaking Engine & Assessment | NOT STARTED |
| 5 | AI Conversation & RAG Engine | NOT STARTED |
| 6 | Subscription, Payment, Gamification | NOT STARTED |
| 7 | Admin Portal, Analytics & Production | NOT STARTED |

---

## Phase 1 Plan Summary

**28 tasks across 5 waves:**

| Wave | Tasks | Focus |
|---|---|---|
| Wave 1 | 6 | Root scaffold (turbo, pnpm, tsconfig, .env, .gitignore) |
| Wave 2 | 7 | Shared packages (types, config, logger, utils, auth, ai-sdk, ui) |
| Wave 3 | 5 | App scaffolding (web, api, workers, admin, ai-gateway) |
| Wave 4 | 4 | Database (prisma schema: 30 tables, extensions, seed) |
| Wave 5 | 6 | Docker, Husky, commitlint, GitHub Actions CI/CD |

**Key files created by Phase 1:**
- `package.json`, `pnpm-workspace.yaml`, `turbo.json`, `tsconfig.base.json`
- `packages/types/`, `packages/config/`, `packages/logger/`, `packages/utils/`, `packages/auth/`, `packages/ai-sdk/`, `packages/ui/`, `packages/prisma/`
- `apps/web/`, `apps/api/`, `apps/workers/`, `apps/admin/`, `apps/ai-gateway/`
- `docker-compose.yml`, `.github/workflows/ci.yml`
- `packages/prisma/schema.prisma` (30 tables, pgvector ready)

**Exit criteria:** `pnpm install` + `pnpm typecheck` + `pnpm db:migrate` + `docker compose ps` all pass.

---

## Locked Decisions

See PROJECT.md `<decisions>` block.

Key locks:
- Turborepo + pnpm monorepo
- NestJS backend + NextJS 15 frontend
- PostgreSQL 16 + Prisma ORM + pgvector
- Multi-LLM (OpenAI + Claude + Gemini) with fallback chain
- Assessment formula: 25%P + 20%F + 20%G + 20%V + 15%C
- BullMQ for async jobs (5 queues)
- JWT + Refresh Token auth
- pgvector for MVP vector storage

---

## Open Decisions

| ID | Question | Resolve In |
|---|---|---|
| OD-001 | File storage: AWS S3 or Cloudflare R2? | Phase 1 (.env.example lists both) |
| OD-002 | Payment provider: Stripe confirmed? | Phase 6 setup |
| OD-003 | Kubernetes cloud provider: GKE / EKS / self-hosted? | Phase 7 setup |
| OD-004 | STT provider: OpenAI Whisper API or self-hosted? | Phase 4 setup |

---

## Session Log

| Date | Action |
|---|---|
| 2026-08-29 | Initialized via /gsd:ingest-docs docs/PRD |
| 2026-08-29 | Created PROJECT.md, REQUIREMENTS.md, ROADMAP.md, STATE.md |
| 2026-08-29 | Phase 1 planned via /gsd:plan-phase 1 — 28 tasks, 5 waves |
