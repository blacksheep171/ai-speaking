# 24-DEPLOYMENT-RUNBOOK.md
# Deployment Runbook & Operations Guide

## AI English Speaking Platform

**Version:** 1.0  
**Frameworks:** NestJS, NextJS  
**Infrastructure:** Kubernetes (EKS)  
**CI/CD:** GitHub Actions  
**GitOps:** ArgoCD  
**Database:** PostgreSQL  
**Cache:** Redis  
**Queue:** BullMQ

---

# 1. Overview

## Purpose

Deployment Runbook cung cấp:

- Local Development Setup
- Deployment Procedures
- Release Management
- Rollback Strategies
- Incident Response
- Backup & Restore Operations

---

## Objectives

```text
Consistent Deployments

Minimal Downtime

Fast Recovery

Operational Excellence
```

---

# 2. Environment Overview

Supported Environments:

```text
Local

Development

Staging

Production
```

---

# 3. Environment URLs

Example:

| Environment | URL |
|------------|------------|
| Local | localhost |
| Development | dev.platform.com |
| Staging | staging.platform.com |
| Production | app.platform.com |

---

# 4. Local Development Setup

Requirements:

```text
NodeJS 22+

Docker

Docker Compose

Git
```

---

# 5. Clone Repository

```bash
git clone git@github.com:company/ai-english-platform.git

cd ai-english-platform
```

---

# 6. Install Dependencies

```bash
pnpm install
```

---

# 7. Environment Variables

Create:

```bash
cp .env.example .env
```

---

# 8. Required Variables

```env
DATABASE_URL=

REDIS_URL=

JWT_SECRET=

OPENAI_API_KEY=

CLAUDE_API_KEY=

GEMINI_API_KEY=
```

---

# 9. Docker Compose Services

```text
PostgreSQL

Redis

MinIO

Mailhog
```

---

# 10. Start Local Stack

```bash
docker compose up -d
```

---

# 11. Database Migration

```bash
pnpm prisma migrate dev
```

---

# 12. Seed Database

```bash
pnpm prisma db seed
```

---

# 13. Start Backend

```bash
pnpm start:dev
```

---

# 14. Start Frontend

```bash
pnpm dev
```

---

# 15. Verify Environment

Check:

```text
API

Frontend

Database

Redis
```

---

# 16. CI/CD Pipeline

Pipeline Stages:

```text
Lint

Test

Build

Dockerize

Deploy
```

---

# 17. GitHub Actions Workflow

Triggered By:

```text
Push

Pull Request

Release Tag
```

---

# 18. Build Process

```text
Install Dependencies

Run Tests

Generate Prisma Client

Build Applications
```

---

# 19. Docker Build

Generate:

```text
Frontend Image

Backend Image

Worker Image
```

---

# 20. Container Registry

Recommended:

```text
Amazon ECR
```

---

# 21. Deployment Strategy

Recommended:

```text
Rolling Update
```

---

Alternative:

```text
Blue-Green
```

---

# 22. GitOps Workflow

```text
Code Merge
 ↓
Build Image
 ↓
Update Manifest
 ↓
ArgoCD Sync
 ↓
Deployment
```

---

# 23. ArgoCD Applications

```text
frontend

backend

workers

monitoring
```

---

# 24. Kubernetes Namespaces

```text
platform-dev

platform-staging

platform-prod
```

---

# 25. Manual Deployment

Sync Application:

```bash
argocd app sync backend
```

---

# 26. Verify Deployment

Check:

```bash
kubectl get pods
```

---

Expected:

```text
Running
```

---

# 27. Health Checks

Endpoints:

```http
GET /health

GET /ready

GET /live
```

---

# 28. Deployment Validation

Verify:

```text
Pods Healthy

No Errors

Traffic Stable
```

---

# 29. Database Migration Strategy

Rules:

```text
Backward Compatible

No Breaking Changes
```

---

# 30. Production Migration

Run:

```bash
pnpm prisma migrate deploy
```

---

# 31. Migration Checklist

Validate:

```text
Backup Completed

Migration Tested

Rollback Plan Ready
```

---

# 32. Rollback Strategy

Triggers:

```text
High Errors

Failed Deployment

Performance Degradation
```

---

# 33. Application Rollback

ArgoCD:

```bash
argocd app rollback backend
```

---

# 34. Kubernetes Rollback

```bash
kubectl rollout undo deployment/backend
```

---

# 35. Database Rollback

Preferred:

```text
Forward Fix
```

---

Avoid:

```text
Destructive Rollback
```

---

# 36. Incident Response Levels

```text
P1 Critical

P2 High

P3 Medium

P4 Low
```

---

# 37. P1 Examples

```text
Production Down

Database Outage

Payment Failure
```

---

# 38. Incident Response Flow

```text
Detect
 ↓
Acknowledge
 ↓
Investigate
 ↓
Mitigate
 ↓
Resolve
 ↓
Postmortem
```

---

# 39. Monitoring Runbook

Dashboards:

```text
Infrastructure

Application

Database

AI Services
```

---

# 40. Key Metrics

Track:

```text
CPU

Memory

Latency

Error Rate

Queue Length
```

---

# 41. Alerting Channels

```text
Slack

Email

PagerDuty
```

---

# 42. Log Investigation

Tools:

```text
Grafana

Loki

OpenTelemetry
```

---

# 43. Backup Strategy

Backup:

```text
Database

Storage

Configuration
```

---

# 44. PostgreSQL Backup

Frequency:

```text
Daily Full Backup

Hourly WAL Archive
```

---

Retention:

```text
30 Days
```

---

# 45. Redis Backup

Method:

```text
Snapshots

AOF
```

---

# 46. Restore Procedure

Steps:

```text
Stop Writes

Restore Backup

Validate Data

Resume Traffic
```

---

# 47. Disaster Recovery

Targets:

```text
RPO < 15 Minutes

RTO < 1 Hour
```

---

# 48. Production Release Checklist

Before Release:

```text
Tests Passed

Security Scan Passed

Migration Verified

Monitoring Ready

Rollback Plan Ready
```

---

# 49. Post Release Checklist

Verify:

```text
No Error Spikes

Stable Traffic

Healthy Pods

Successful Transactions
```

---

# 50. AI Services Runbook

Validate:

```text
OpenAI Connectivity

Claude Connectivity

Gemini Connectivity
```

---

Fallback:

```text
Provider Switching Enabled
```

---

# 51. Queue Operations

BullMQ Queues:

```text
email-queue

ai-queue

assessment-queue

analytics-queue
```

---

Monitor:

```text
Failed Jobs

Queue Lag

Retries
```

---

# 52. Redis Operations

Check:

```bash
redis-cli info
```

---

Monitor:

```text
Memory

Evictions

Connections
```

---

# 53. PostgreSQL Operations

Check:

```sql
SELECT NOW();
```

---

Monitor:

```text
Connections

Slow Queries

Replication Lag
```

---

# 54. Security Operations

Verify:

```text
TLS Certificates

Secrets Rotation

Access Reviews
```

---

Frequency:

```text
Monthly
```

---

# 55. Operational KPIs

Track:

```text
Deployment Frequency

MTTR

Incident Count

Change Failure Rate
```

---

# 56. Escalation Matrix

Level 1:

```text
On-Call Engineer
```

---

Level 2:

```text
Senior Engineer
```

---

Level 3:

```text
Engineering Manager
```

---

# 57. Future Enhancements

Phase 2

```text
Automated Rollbacks
```

---

Phase 3

```text
Chaos Engineering
```

---

Phase 4

```text
Self-Healing Infrastructure
```

---

Phase 5

```text
Multi-Region Disaster Recovery
```

---

# Conclusion

Deployment Runbook cung cấp:

- Deployment Procedures
- Rollback Strategies
- Incident Response
- Monitoring Operations
- Backup & Restore
- Production Checklists

Được thiết kế để:

- Giảm downtime
- Tăng độ ổn định hệ thống
- Chuẩn hóa vận hành
- Hỗ trợ scale enterprise-grade

Tech Stack:

- NestJS
- NextJS
- PostgreSQL
- Redis
- BullMQ
- Kubernetes
- ArgoCD
- GitHub Actions
- AWS