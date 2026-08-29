# 21-SECURITY-ARCHITECTURE.md
# Security Architecture & Compliance

## AI English Speaking Platform

**Version:** 1.0  
**Module:** Security Architecture  
**Framework:** NestJS + NextJS  
**Authentication:** JWT + OAuth2  
**Infrastructure:** Kubernetes (EKS)  
**Compliance:** GDPR, SOC2, ISO27001

---

# 1. Overview

## Purpose

Security Architecture chịu trách nhiệm:

- Authentication
- Authorization
- API Security
- Data Protection
- AI Security
- Infrastructure Security
- Compliance
- Security Monitoring

---

## Security Principles

```text
Least Privilege

Defense In Depth

Zero Trust

Secure By Default
```

---

# 2. Security Architecture

```text
User
 ↓
Authentication
 ↓
Authorization
 ↓
API Gateway
 ↓
Application
 ↓
Database
 ↓
Audit Logs
```

---

# 3. Security Layers

```text
Identity Security

Application Security

API Security

Data Security

Infrastructure Security

AI Security
```

---

# 4. Authentication

Supported Methods:

```text
Email + Password

Google OAuth

Apple Login

Magic Link
```

---

# 5. JWT Authentication

Tokens:

```text
Access Token

Refresh Token
```

---

# 6. Access Token

Properties:

```text
Short Lived

Stateless

Signed JWT
```

---

Expiration:

```text
15 Minutes
```

---

# 7. Refresh Token

Properties:

```text
Long Lived

Stored Securely

Rotated
```

---

Expiration:

```text
30 Days
```

---

# 8. Authentication Flow

```text
Login
 ↓
Verify Credentials
 ↓
Generate Tokens
 ↓
Return Access + Refresh
```

---

# 9. Refresh Flow

```text
Refresh Token
 ↓
Validate
 ↓
Rotate
 ↓
Issue New Tokens
```

---

# 10. Password Security

Requirements:

```text
Minimum 8 Characters

Uppercase

Lowercase

Number

Special Character
```

---

Hashing:

```text
Argon2
```

---

# 11. OAuth2 Login

Providers:

```text
Google

Apple

Microsoft (Future)
```

---

# 12. Session Management

Track:

```text
Device

IP Address

Location

Last Activity
```

---

# 13. Authorization

Model:

```text
RBAC
```

---

Roles:

```text
Student

Instructor

Admin

Super Admin
```

---

# 14. Permissions

Examples:

```text
lesson.read

lesson.write

course.read

course.write

admin.manage
```

---

# 15. RBAC Architecture

```text
User
 ↓
Role
 ↓
Permissions
 ↓
Resource Access
```

---

# 16. API Security

Protect:

```text
Authentication APIs

AI APIs

Payment APIs
```

---

# 17. API Gateway Controls

Features:

```text
Rate Limiting

Authentication

Logging

Monitoring
```

---

# 18. Rate Limiting

Examples:

```text
Login

5 Requests / Minute
```

---

```text
AI Chat

60 Requests / Minute
```

---

# 19. Input Validation

Validate:

```text
Body

Query

Headers

Files
```

---

Tools:

```text
class-validator

Zod
```

---

# 20. OWASP Top 10 Protection

Protect Against:

```text
Injection

Broken Access Control

XSS

CSRF

SSRF

Insecure Deserialization
```

---

# 21. SQL Injection Protection

Use:

```text
Prisma ORM
```

---

Rules:

```text
Parameterized Queries Only
```

---

# 22. XSS Protection

Strategies:

```text
Input Sanitization

Output Encoding

CSP Headers
```

---

# 23. CSRF Protection

Methods:

```text
SameSite Cookies

CSRF Tokens
```

---

# 24. File Upload Security

Validate:

```text
File Type

File Size

Content Type
```

---

Scan:

```text
Malware
```

---

# 25. AI Security

Protect:

```text
AI Models

Prompts

Context Data
```

---

# 26. Prompt Injection Protection

Detect:

```text
Ignore Instructions

Reveal System Prompt

Bypass Safety Rules
```

---

Action:

```text
Block Request
```

---

# 27. AI Content Moderation

Detect:

```text
Toxic Content

Harassment

Violence

Illegal Content
```

---

# 28. AI Output Validation

Validate:

```text
Unsafe Responses

Prompt Leakage

Sensitive Data
```

---

# 29. Data Encryption

Encryption:

```text
At Rest

In Transit
```

---

# 30. Transport Security

Use:

```text
TLS 1.3
```

---

Rules:

```text
HTTPS Only
```

---

# 31. Database Encryption

Encrypt:

```text
Backups

Snapshots

Volumes
```

---

# 32. Secrets Management

Tool:

```text
AWS Secrets Manager
```

---

Store:

```text
API Keys

DB Passwords

JWT Secrets
```

---

# 33. Personal Data Protection

Sensitive Data:

```text
Email

Phone

Learning History

Payment Metadata
```

---

# 34. Data Retention

Policies:

```text
User Controlled

Configurable
```

---

Example:

```text
Delete After Account Closure
```

---

# 35. Audit Logging

Track:

```text
Logins

Permission Changes

Billing Changes

Admin Actions
```

---

# 36. Audit Log Schema

## audit_logs

```sql
id UUID

user_id UUID

action VARCHAR

resource VARCHAR

metadata JSONB

created_at TIMESTAMP
```

---

# 37. Security Monitoring

Monitor:

```text
Failed Logins

Rate Limit Violations

Permission Errors

Suspicious Activity
```

---

# 38. Threat Detection

Detect:

```text
Brute Force

Credential Stuffing

Abnormal Usage
```

---

# 39. Security Alerts

Notify:

```text
Security Team

Admins

Affected Users
```

---

# 40. GDPR Compliance

Support:

```text
Right To Access

Right To Delete

Right To Export
```

---

# 41. Data Export

Formats:

```text
JSON

CSV
```

---

# 42. Data Deletion

Flow:

```text
Request
 ↓
Verification
 ↓
Deletion
 ↓
Audit Log
```

---

# 43. SOC2 Controls

Requirements:

```text
Access Control

Monitoring

Incident Response

Backups
```

---

# 44. Security Incident Response

Lifecycle:

```text
Detection
 ↓
Investigation
 ↓
Containment
 ↓
Recovery
 ↓
Postmortem
```

---

# 45. Penetration Testing

Frequency:

```text
Quarterly
```

---

Scope:

```text
Web

API

Infrastructure
```

---

# 46. Security Metrics

Track:

```text
Failed Logins

Blocked Requests

Security Incidents

Vulnerability Count
```

---

# 47. Security Targets

```text
Zero Critical Vulnerabilities

99.9% Availability

< 15 Min Incident Detection
```

---

# 48. Integration Points

Connected To:

```text
User Module

Billing Module

AI Tutor

Analytics Module

DevOps Infrastructure
```

---

# 49. Future Enhancements

Phase 2

```text
MFA Authentication
```

---

Phase 3

```text
Risk-Based Authentication
```

---

Phase 4

```text
Behavioral Biometrics
```

---

Phase 5

```text
AI Threat Detection
```

---

# Conclusion

Security Architecture cung cấp:

- Authentication & Authorization
- API Security
- AI Security
- Data Protection
- Compliance Controls
- Audit Logging

Được thiết kế để:

- Bảo vệ dữ liệu người dùng
- Đảm bảo tuân thủ pháp lý
- Giảm thiểu rủi ro bảo mật
- Hỗ trợ mở rộng quy mô toàn cầu

Tech Stack:

- NestJS
- NextJS
- JWT
- OAuth2
- Prisma
- PostgreSQL
- Redis
- AWS Secrets Manager
- Kubernetes
- TLS 1.3