# 23-CODING-STANDARDS.md
# Coding Standards & Engineering Guidelines

## AI English Speaking Platform

**Version:** 1.0  
**Applies To:** Backend, Frontend, AI Services, DevOps  
**Languages:** TypeScript, SQL  
**Frameworks:** NestJS, NextJS, Prisma

---

# 1. Overview

## Purpose

Coding Standards đảm bảo:

- Consistency
- Maintainability
- Scalability
- Readability
- Testability

---

## Engineering Principles

```text
SOLID

DRY

KISS

YAGNI

Clean Architecture
```

---

# 2. Monorepo Standards

Structure:

```text
apps/
packages/
infrastructure/
docs/
```

---

Rules:

```text
No Cross-Layer Dependencies

Shared Code In Packages

Feature-Based Modules
```

---

# 3. TypeScript Standards

Required:

```text
strict = true
```

---

Forbidden:

```text
any
```

---

Preferred:

```typescript
unknown
```

---

# 4. Naming Conventions

| Item | Convention |
|--------|--------|
| Variable | camelCase |
| Function | camelCase |
| Class | PascalCase |
| Interface | PascalCase |
| Enum | PascalCase |
| Constant | UPPER_SNAKE_CASE |

---

Examples:

```typescript
const MAX_RETRY = 3;

class UserService {}

function createUser() {}
```

---

# 5. File Naming

Use:

```text
kebab-case
```

---

Examples:

```text
user.service.ts

course.controller.ts

ai-provider.factory.ts
```

---

# 6. Folder Naming

Use:

```text
kebab-case
```

---

Examples:

```text
user-management

course-engine

speaking-module
```

---

# 7. Import Rules

Order:

```text
Node Modules

Internal Packages

Relative Imports
```

---

Example:

```typescript
import fs from 'fs';

import { Logger } from '@platform/common';

import { User } from './user.entity';
```

---

# 8. NestJS Standards

Structure:

```text
module

controller

service

repository
```

---

Example:

```text
users
├── user.module.ts
├── user.controller.ts
├── user.service.ts
└── user.repository.ts
```

---

# 9. Controller Rules

Responsibilities:

```text
Validate Input

Call Service

Return Response
```

---

Forbidden:

```text
Business Logic
```

---

# 10. Service Rules

Responsibilities:

```text
Business Logic

Transactions

Domain Rules
```

---

Forbidden:

```text
Direct HTTP Logic
```

---

# 11. Repository Rules

Responsibilities:

```text
Database Access
```

---

Forbidden:

```text
Business Logic
```

---

# 12. Dependency Injection

Required:

```typescript
constructor(
  private readonly userService: UserService
) {}
```

---

Forbidden:

```typescript
new UserService()
```

---

# 13. DTO Standards

All Requests:

```text
Must Use DTOs
```

---

Example:

```typescript
export class CreateUserDto {
  email: string;
}
```

---

# 14. Validation Standards

Use:

```text
class-validator
```

---

Example:

```typescript
@IsEmail()
email: string;
```

---

# 15. Prisma Standards

Access Database Through:

```text
Repository Layer
```

---

Forbidden:

```text
Prisma Calls In Controllers
```

---

# 16. Prisma Transactions

Required For:

```text
Payments

Subscriptions

Critical Writes
```

---

Example:

```typescript
await prisma.$transaction(...)
```

---

# 17. API Design Standards

Style:

```text
RESTful
```

---

Versioning:

```text
/api/v1
```

---

# 18. Endpoint Naming

Good:

```http
GET /users

POST /users

GET /users/{id}
```

---

Bad:

```http
GET /getUsers
```

---

# 19. Response Format

Standard:

```json
{
  "success": true,
  "data": {},
  "meta": {}
}
```

---

# 20. Error Format

Standard:

```json
{
  "success": false,
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User not found"
  }
}
```

---

# 21. HTTP Status Codes

Use:

```text
200 OK

201 Created

400 Bad Request

401 Unauthorized

403 Forbidden

404 Not Found

500 Internal Server Error
```

---

# 22. Exception Handling

Use:

```text
Global Exception Filter
```

---

Avoid:

```typescript
try/catch Everywhere
```

---

# 23. Logging Standards

Use:

```text
Structured Logs
```

---

Example:

```json
{
  "event": "user_created",
  "userId": "123"
}
```

---

# 24. Log Levels

```text
DEBUG

INFO

WARN

ERROR
```

---

# 25. Forbidden Logging

Never Log:

```text
Passwords

Tokens

Credit Cards

Secrets
```

---

# 26. Clean Architecture

Layers:

```text
Presentation

Application

Domain

Infrastructure
```

---

# 27. Dependency Rule

```text
Outer → Inner

Never Reverse
```

---

# 28. Domain-Driven Design (DDD)

Use For:

```text
User

Course

Assessment

Subscription
```

---

Concepts:

```text
Entity

Value Object

Aggregate

Domain Service
```

---

# 29. SOLID Principles

Required:

```text
Single Responsibility

Open/Closed

Liskov

Interface Segregation

Dependency Inversion
```

---

# 30. Function Standards

Maximum:

```text
50 Lines
```

---

Preferred:

```text
20–30 Lines
```

---

# 31. Class Standards

Maximum:

```text
300 Lines
```

---

Preferred:

```text
< 200 Lines
```

---

# 32. Method Standards

Rules:

```text
Single Responsibility

Self-Descriptive Names
```

---

Example:

```typescript
calculateSpeakingScore()
```

---

# 33. Comment Standards

Comment:

```text
Why
```

---

Avoid:

```text
What
```

---

Bad:

```typescript
// Increment counter
counter++;
```

---

# 34. Constants Standards

Use:

```typescript
export const MAX_LOGIN_ATTEMPTS = 5;
```

---

Avoid:

```typescript
if (attempts > 5)
```

---

# 35. Environment Variables

Required:

```text
.env.example
```

---

Naming:

```text
DATABASE_URL

REDIS_URL

JWT_SECRET
```

---

# 36. Git Branch Strategy

```text
main

develop

feature/*

hotfix/*
```

---

# 37. Commit Convention

Use:

```text
Conventional Commits
```

---

Examples:

```text
feat(auth): add Google login

fix(ai): resolve token overflow

refactor(user): simplify service
```

---

# 38. Pull Request Standards

Must Include:

```text
Description

Screenshots

Test Results
```

---

# 39. Code Review Checklist

Review:

```text
Logic

Performance

Security

Tests

Readability
```

---

# 40. Security Standards

Validate:

```text
Input

Authorization

Rate Limits
```

---

Never Trust:

```text
Client Data
```

---

# 41. Performance Standards

Avoid:

```text
N+1 Queries

Large Payloads

Blocking Operations
```

---

Use:

```text
Pagination

Caching

Indexes
```

---

# 42. Frontend Standards

Framework:

```text
NextJS App Router
```

---

Rules:

```text
Server Components First

Client Components When Needed
```

---

# 43. React Standards

Avoid:

```text
Huge Components
```

---

Preferred:

```text
Reusable Components
```

---

# 44. State Management

Use:

```text
React Query

Zustand
```

---

Avoid:

```text
Global State Abuse
```

---

# 45. Testing Standards

Required:

```text
Unit Tests

Integration Tests
```

---

Coverage:

```text
80%+
```

---

# 46. Documentation Standards

Every Module Must Have:

```text
README

API Docs

Architecture Notes
```

---

# 47. Definition Of Done

Requirements:

```text
Code Complete

Tests Pass

Reviewed

Documented

Deployed
```

---

# 48. Engineering KPIs

Track:

```text
Coverage

Bugs

PR Lead Time

Deployment Frequency
```

---

# 49. Future Improvements

Phase 2

```text
Architecture Decision Records
```

---

Phase 3

```text
Automated Code Quality Gates
```

---

Phase 4

```text
AI-Assisted Code Reviews
```

---

# Conclusion

Coding Standards đảm bảo:

- Consistent Codebase
- High Maintainability
- Scalability
- Security
- Engineering Excellence

Áp dụng cho:

- NestJS
- NextJS
- Prisma
- PostgreSQL
- Redis
- BullMQ
- AI Services
- Kubernetes

Nhằm xây dựng một nền tảng AI English Speaking Platform có khả năng mở rộng, dễ bảo trì và đạt chuẩn enterprise-grade.