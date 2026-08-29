# Intel: Locked Decisions

Source: docs/PRD (25 docs), synthesized 2026-08-29

## Architecture
- Modular Monolith → Microservices Ready (NestJS modules)
- Domain Driven Design: Auth, User, Course, Lesson, Conversation, Assessment, AI, Payment, Analytics, Notification
- API First: all communication via REST API + internal events

## Stack (all LOCKED)
- Turborepo + pnpm workspaces
- NextJS 15 + React 19 + TailwindCSS + Zustand + React Query
- NestJS + TypeScript + Prisma
- PostgreSQL 16 (UUID PKs, snake_case, UTC)
- Redis (cache + rate limiting + queue backend)
- BullMQ (all async job processing)
- Multi-LLM: OpenAI + Claude + Gemini with fallback
- pgvector for MVP vector storage
- GitHub Actions CI/CD
- Kubernetes + ArgoCD GitOps

## AI Decisions (LOCKED)
- Routing matrix: GPT for grammar/pronunciation, Claude for long context/IELTS, Gemini for speed/cost
- Fallback chain: GPT → Claude → Gemini → static message
- Embedding: text-embedding-3-large primary
- Chunk size: 500 tokens, 50 overlap
- Context compression: summarize old messages to manage tokens

## Assessment (LOCKED)
- 5 dimensions: Pronunciation, Fluency, Grammar, Vocabulary, Coherence
- Formula: 25%/20%/20%/20%/15%
- Scale: 0–100 per dimension
