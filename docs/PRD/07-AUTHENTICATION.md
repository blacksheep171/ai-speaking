# 07-AUTHENTICATION.md
# Authentication & Authorization Design

## AI English Speaking Platform

**Version:** 1.0  
**Module:** Authentication & Authorization  
**Framework:** NestJS  
**Database:** PostgreSQL + Prisma  
**Cache:** Redis

---

# 1. Overview

## Purpose

Authentication Module chịu trách nhiệm:

- User Registration
- User Login
- User Logout
- JWT Authentication
- Refresh Token Rotation
- OAuth Login
- Email Verification
- Password Reset
- Session Management
- Authorization (RBAC)

---

# 2. Authentication Architecture

```text
Client
 ↓
API Gateway
 ↓
Auth Module
 ↓
PostgreSQL
 ↓
Redis
```

---

## Components

```text
Auth Controller

Auth Service

Jwt Service

Token Service

Session Service

OAuth Service

Email Service
```

---

# 3. Authentication Flow

## Login Flow

```text
User
 ↓
Email + Password
 ↓
Auth Service
 ↓
Validate Credentials
 ↓
Generate JWT
 ↓
Generate Refresh Token
 ↓
Store Refresh Token
 ↓
Return Tokens
```

---

# 4. Registration Flow

```text
User
 ↓
Register
 ↓
Validate Email
 ↓
Create User
 ↓
Create Profile
 ↓
Send Verification Email
 ↓
Success
```

---

# 5. JWT Architecture

## Access Token

Purpose:

```text
API Authentication
```

Lifetime:

```text
15 Minutes
```

---

## Refresh Token

Purpose:

```text
Generate New Access Token
```

Lifetime:

```text
30 Days
```

---

# 6. JWT Payload

```json
{
  "sub": "user_uuid",
  "email": "user@example.com",
  "role": "premium",
  "type": "access",
  "iat": 1700000000,
  "exp": 1700000900
}
```

---

# 7. Token Strategy

## Access Token

Storage:

```text
HttpOnly Cookie
```

or

```text
Memory Storage
```

---

## Refresh Token

Storage:

```text
HttpOnly Cookie
```

Never:

```text
localStorage
```

---

# 8. Refresh Token Rotation

## Problem

Nếu refresh token bị đánh cắp:

```text
Account Takeover
```

---

## Solution

Rotation Strategy

```text
Old Refresh Token
 ↓
Refresh Request
 ↓
Generate New Token
 ↓
Revoke Old Token
 ↓
Store New Token
```

---

# 9. Refresh Flow

```text
Access Token Expired
 ↓
Refresh Endpoint
 ↓
Validate Refresh Token
 ↓
Generate New Access Token
 ↓
Generate New Refresh Token
 ↓
Revoke Old Refresh Token
 ↓
Return New Tokens
```

---

# 10. User Roles

```text
guest

user

premium

admin

super_admin
```

---

# 11. RBAC Design

Role-Based Access Control

---

## Example

Guest

```text
View Landing Page
```

---

User

```text
Access Lessons
Access Conversations
```

---

Premium

```text
Unlimited AI Usage
Advanced Reports
```

---

Admin

```text
Manage Users
Manage Courses
```

---

Super Admin

```text
Full Access
```

---

# 12. Permission Matrix

| Resource | User | Premium | Admin |
|-----------|--------|--------|--------|
| Lessons | ✔ | ✔ | ✔ |
| AI Chat | Limited | Unlimited | ✔ |
| Dashboard | ✔ | ✔ | ✔ |
| Users | ✘ | ✘ | ✔ |
| Analytics | ✘ | ✘ | ✔ |

---

# 13. NestJS Guards

## JwtAuthGuard

Purpose:

```text
Verify JWT
```

---

## RolesGuard

Purpose:

```text
Verify User Role
```

---

## PremiumGuard

Purpose:

```text
Verify Subscription
```

---

# 14. Authentication Decorators

## Example

```typescript
@UseGuards(JwtAuthGuard)
```

---

```typescript
@Roles('admin')
```

---

```typescript
@CurrentUser()
```

---

# 15. Password Policy

Minimum Length

```text
8 Characters
```

---

Requirements

```text
Uppercase

Lowercase

Number

Special Character
```

---

Example

```text
StrongPass123!
```

---

# 16. Password Hashing

Algorithm

```text
Argon2
```

Recommended:

```text
argon2id
```

---

Never Use

```text
MD5

SHA1
```

---

# 17. Email Verification

## Flow

```text
Register
 ↓
Generate Verification Token
 ↓
Send Email
 ↓
User Clicks Link
 ↓
Verify Token
 ↓
Activate Account
```

---

## Verification Token

Lifetime:

```text
24 Hours
```

---

# 18. Password Reset

## Flow

```text
Forgot Password
 ↓
Generate Reset Token
 ↓
Email User
 ↓
User Opens Link
 ↓
Reset Password
```

---

## Reset Token

Lifetime

```text
1 Hour
```

---

# 19. Session Management

Storage

```text
Redis
```

---

Session Data

```json
{
  "userId": "uuid",
  "deviceId": "uuid",
  "ipAddress": "x.x.x.x",
  "createdAt": ""
}
```

---

# 20. Device Tracking

Purpose

```text
Security Monitoring
```

---

Tracked Data

```text
Device Name

Browser

Operating System

IP Address

Location
```

---

# 21. Concurrent Sessions

Free User

```text
2 Devices
```

---

Premium User

```text
10 Devices
```

---

Admin

```text
Unlimited
```

---

# 22. OAuth Architecture

Supported Providers

```text
Google

Apple
```

---

Future

```text
Facebook

Microsoft

GitHub
```

---

# 23. Google Login Flow

```text
Frontend
 ↓
Google OAuth
 ↓
Authorization Code
 ↓
Backend
 ↓
Google Verify
 ↓
Create/Login User
 ↓
Issue JWT
```

---

# 24. Apple Login Flow

```text
Frontend
 ↓
Apple Sign-In
 ↓
Identity Token
 ↓
Backend Verify
 ↓
Create/Login User
 ↓
Issue JWT
```

---

# 25. Database Tables

```text
users

user_profiles

user_devices

refresh_tokens

verification_tokens

password_reset_tokens
```

---

# 26. users Table

```sql
id UUID PK

email VARCHAR(255)

password_hash TEXT

role VARCHAR(50)

status VARCHAR(50)

email_verified BOOLEAN

created_at TIMESTAMP
```

---

# 27. refresh_tokens Table

```sql
id UUID PK

user_id UUID

token_hash TEXT

expires_at TIMESTAMP

revoked_at TIMESTAMP
```

---

## Security

Store:

```text
Hashed Token
```

Never:

```text
Raw Token
```

---

# 28. verification_tokens Table

```sql
id UUID

user_id UUID

token_hash TEXT

expires_at TIMESTAMP
```

---

# 29. password_reset_tokens Table

```sql
id UUID

user_id UUID

token_hash TEXT

expires_at TIMESTAMP
```

---

# 30. Redis Strategy

## Session Keys

```text
session:{userId}

session:{userId}:{deviceId}
```

---

## Rate Limit Keys

```text
login:{ip}

register:{ip}

password-reset:{email}
```

---

# 31. Rate Limiting

Login

```text
5 Attempts / 15 Minutes
```

---

Register

```text
10 Requests / Hour
```

---

Password Reset

```text
3 Requests / Hour
```

---

# 32. Brute Force Protection

If Exceeded:

```text
Temporary Block
```

---

Example

```text
15 Minutes Lock
```

---

# 33. Security Headers

```text
Content-Security-Policy

X-Frame-Options

X-Content-Type-Options

Strict-Transport-Security
```

---

# 34. JWT Secret Management

Never Store

```text
Source Code
```

---

Store In

```text
Kubernetes Secret

Vault
```

---

# 35. Audit Logging

Log Events

```text
LOGIN_SUCCESS

LOGIN_FAILED

LOGOUT

PASSWORD_CHANGED

EMAIL_VERIFIED

TOKEN_REFRESHED
```

---

# 36. Authentication Metrics

Track

```text
Login Success Rate

Failed Logins

Active Sessions

OAuth Usage

Password Resets
```

---

# 37. Authentication Sequence Diagram

```text
User
 ↓
Login
 ↓
Auth Controller
 ↓
Auth Service
 ↓
Validate Password
 ↓
Generate JWT
 ↓
Store Refresh Token
 ↓
Return Tokens
 ↓
User
```

---

# 38. Error Codes

```text
INVALID_CREDENTIALS

EMAIL_NOT_VERIFIED

ACCOUNT_LOCKED

TOKEN_EXPIRED

TOKEN_INVALID

SESSION_EXPIRED
```

---

# 39. High Availability Strategy

PostgreSQL

```text
Primary + Replica
```

---

Redis

```text
Redis Sentinel
```

---

Stateless API

```text
Horizontal Scaling
```

---

# 40. Future Improvements

Phase 2

```text
Multi-Factor Authentication (MFA)
```

---

Phase 3

```text
Passkeys
```

---

Phase 4

```text
Biometric Authentication
```

---

# Conclusion

Authentication Module cung cấp:

- JWT Authentication
- Refresh Token Rotation
- OAuth Login
- RBAC Authorization
- Session Management
- Redis Caching
- Security Monitoring

Đảm bảo:

- OWASP Compliance
- High Scalability
- Enterprise Security
- Kubernetes Ready

Tech Stack:

- NestJS
- PostgreSQL
- Prisma
- Redis
- JWT
- OAuth2