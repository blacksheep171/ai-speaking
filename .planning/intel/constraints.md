# Intel: Constraints

## Hard Constraints (from PRD)
- MVP must NOT include: live teacher classes, marketplace, offline courses, native mobile apps, video calling, corporate LMS
- AI response SLA: < 5 seconds
- API response SLA: < 500ms
- Rate limiting: max 100 req/min per IP
- Free plan: max 10 AI conversations per day
- Security: OWASP Top 10 compliance mandatory

## Budget/Cost Constraints
- AI cost optimization required: caching (Redis, 24h TTL for AI responses), model routing (cheap Gemini for simple tasks), prompt optimization
- v1 cap: 50 docs per ingest run

## Technical Constraints
- Node.js >= 18 required for NestJS 10+
- PostgreSQL 16 required for pgvector extension
- pnpm required (not npm or yarn) for Turborepo workspace protocol
