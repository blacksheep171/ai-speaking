# INGEST-CONFLICTS.md
# Ingest Conflicts Report — docs/PRD (25 docs)

**Generated:** 2026-08-29
**Mode:** new
**Total Docs Ingested:** 25
**Conflict Engine:** auto

---

### BLOCKERS (0)

No blocking conflicts detected. All locked decisions are consistent across documents.

---

### WARNINGS (0)

No competing variants requiring manual resolution.

---

### INFO (4)

Auto-resolved minor naming/terminology variants:

1. **AI Providers naming** — 01-PRD says "OpenAI/Claude/Gemini"; 05-AI-ARCHITECTURE clarifies specific model names (GPT-5, Claude Sonnet/Opus, Gemini Pro/Flash). → Resolved: use model-specific names from 05-AI-ARCHITECTURE as authoritative.

2. **Vector DB choice** — 05-AI-ARCHITECTURE specifies pgvector for MVP, Pinecone for scale. 13-RAG-DESIGN confirms same. → Consistent, no conflict.

3. **Monorepo apps listing** — 02-TDD lists (web, api, admin); 25-MONOREPO-STRUCTURE lists (web, api, workers, admin, ai-gateway). → Resolved: use 25-MONOREPO-STRUCTURE as authoritative (more detailed).

4. **File storage** — referenced as "AWS S3" in some docs, "Cloudflare R2" in others. → Both listed as recommended options; no conflict (user chooses based on cost).
