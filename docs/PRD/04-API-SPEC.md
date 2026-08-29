# 04-API-SPEC.md
# API Specification

## AI English Speaking Platform

**Version:** 1.0  
**Protocol:** HTTPS  
**Architecture:** RESTful API  
**Authentication:** JWT Bearer Token  
**Documentation:** OpenAPI 3.1 / Swagger

Base URL:

```text
Production
https://api.ai-english.com

Staging
https://staging-api.ai-english.com

Local
http://localhost:3000
```

---

# 1. API Standards

## Request Headers

```http
Content-Type: application/json
Authorization: Bearer <JWT_TOKEN>
Accept: application/json
```

---

## Success Response

```json
{
  "success": true,
  "data": {},
  "message": "Success"
}
```

---

## Error Response

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request"
  }
}
```

---

# 2. Authentication APIs

Base Path

```text
/api/v1/auth
```

---

## Register

### Endpoint

```http
POST /api/v1/auth/register
```

### Request

```json
{
  "email": "john@example.com",
  "password": "StrongPassword123!",
  "firstName": "John",
  "lastName": "Doe"
}
```

### Response

```json
{
  "success": true,
  "message": "Registration successful"
}
```

---

## Login

### Endpoint

```http
POST /api/v1/auth/login
```

### Request

```json
{
  "email": "john@example.com",
  "password": "password"
}
```

### Response

```json
{
  "accessToken": "jwt_token",
  "refreshToken": "refresh_token",
  "expiresIn": 3600
}
```

---

## Refresh Token

```http
POST /api/v1/auth/refresh
```

### Request

```json
{
  "refreshToken": "token"
}
```

### Response

```json
{
  "accessToken": "new_token"
}
```

---

## Logout

```http
POST /api/v1/auth/logout
```

---

## Forgot Password

```http
POST /api/v1/auth/forgot-password
```

---

## Reset Password

```http
POST /api/v1/auth/reset-password
```

---

## Verify Email

```http
POST /api/v1/auth/verify-email
```

---

# 3. User APIs

Base Path

```text
/api/v1/users
```

---

## Get Profile

```http
GET /api/v1/users/me
```

### Response

```json
{
  "id": "uuid",
  "email": "john@example.com",
  "firstName": "John",
  "lastName": "Doe",
  "englishLevel": "Intermediate",
  "learningGoal": "IELTS"
}
```

---

## Update Profile

```http
PUT /api/v1/users/me
```

### Request

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "englishLevel": "Advanced"
}
```

---

## User Settings

```http
GET /api/v1/users/settings

PUT /api/v1/users/settings
```

---

## Upload Avatar

```http
POST /api/v1/users/avatar
```

Content-Type:

```text
multipart/form-data
```

---

# 4. Course APIs

Base Path

```text
/api/v1/courses
```

---

## List Courses

```http
GET /api/v1/courses
```

### Query Params

```text
?page=1
&limit=20
&level=beginner
```

---

## Course Detail

```http
GET /api/v1/courses/{courseId}
```

---

## Enroll Course

```http
POST /api/v1/courses/{courseId}/enroll
```

---

## My Courses

```http
GET /api/v1/courses/my-courses
```

---

# 5. Lesson APIs

Base Path

```text
/api/v1/lessons
```

---

## Lesson Detail

```http
GET /api/v1/lessons/{lessonId}
```

---

## Complete Lesson

```http
POST /api/v1/lessons/{lessonId}/complete
```

### Response

```json
{
  "completed": true
}
```

---

## Lesson Progress

```http
GET /api/v1/lessons/progress
```

---

# 6. Conversation APIs

Base Path

```text
/api/v1/conversations
```

---

## Create Conversation

```http
POST /api/v1/conversations
```

### Request

```json
{
  "topic": "Job Interview",
  "aiRole": "Interviewer"
}
```

### Response

```json
{
  "id": "conversation_uuid"
}
```

---

## Get Conversations

```http
GET /api/v1/conversations
```

---

## Conversation Detail

```http
GET /api/v1/conversations/{id}
```

---

## Delete Conversation

```http
DELETE /api/v1/conversations/{id}
```

---

# 7. Message APIs

Base Path

```text
/api/v1/messages
```

---

## Send Message

```http
POST /api/v1/messages
```

### Request

```json
{
  "conversationId": "uuid",
  "message": "Hello AI"
}
```

### Response

```json
{
  "reply": "Hello, how can I help you today?"
}
```

---

## Get Messages

```http
GET /api/v1/messages/{conversationId}
```

---

# 8. Voice APIs

Base Path

```text
/api/v1/voice
```

---

## Upload Audio

```http
POST /api/v1/voice/upload
```

Content-Type:

```text
multipart/form-data
```

### Response

```json
{
  "audioId": "uuid"
}
```

---

## Transcribe Audio

```http
POST /api/v1/voice/transcribe
```

### Request

```json
{
  "audioId": "uuid"
}
```

### Response

```json
{
  "transcript": "Hello everyone"
}
```

---

## Pronunciation Analysis

```http
POST /api/v1/voice/analyze
```

### Request

```json
{
  "audioId": "uuid"
}
```

### Response

```json
{
  "pronunciation": 85,
  "fluency": 82,
  "grammar": 88,
  "vocabulary": 80,
  "coherence": 84
}
```

---

# 9. Assessment APIs

Base Path

```text
/api/v1/assessments
```

---

## Create Assessment

```http
POST /api/v1/assessments
```

### Request

```json
{
  "conversationId": "uuid"
}
```

---

## Assessment Result

```http
GET /api/v1/assessments/{assessmentId}
```

### Response

```json
{
  "overallScore": 86,
  "pronunciation": 88,
  "fluency": 82,
  "grammar": 89,
  "vocabulary": 84,
  "feedback": {
    "strengths": [],
    "weaknesses": [],
    "recommendations": []
  }
}
```

---

## Assessment History

```http
GET /api/v1/assessments/history
```

---

# 10. AI APIs

Base Path

```text
/api/v1/ai
```

---

## AI Chat

```http
POST /api/v1/ai/chat
```

### Request

```json
{
  "message": "Let's practice English",
  "conversationId": "uuid"
}
```

---

### Response

```json
{
  "response": "Sure! Let's start."
}
```

---

## AI Feedback

```http
POST /api/v1/ai/feedback
```

---

## AI Recommendations

```http
GET /api/v1/ai/recommendations
```

### Response

```json
{
  "lessons": [],
  "topics": [],
  "vocabulary": []
}
```

---

# 11. RAG APIs

Base Path

```text
/api/v1/rag
```

---

## Search Knowledge

```http
POST /api/v1/rag/search
```

### Request

```json
{
  "query": "Past perfect tense"
}
```

### Response

```json
{
  "documents": []
}
```

---

# 12. Dashboard APIs

Base Path

```text
/api/v1/dashboard
```

---

## Dashboard Summary

```http
GET /api/v1/dashboard
```

### Response

```json
{
  "totalMinutes": 450,
  "totalLessons": 32,
  "currentStreak": 14,
  "averageScore": 84
}
```

---

## Weekly Statistics

```http
GET /api/v1/dashboard/weekly
```

---

## Monthly Statistics

```http
GET /api/v1/dashboard/monthly
```

---

# 13. Achievement APIs

Base Path

```text
/api/v1/achievements
```

---

## List Achievements

```http
GET /api/v1/achievements
```

---

## User Achievements

```http
GET /api/v1/achievements/my
```

---

# 14. Subscription APIs

Base Path

```text
/api/v1/subscriptions
```

---

## Plans

```http
GET /api/v1/subscriptions/plans
```

### Response

```json
[
  {
    "id": "premium",
    "price": 9.99,
    "currency": "USD"
  }
]
```

---

## Subscribe

```http
POST /api/v1/subscriptions/subscribe
```

---

## Cancel Subscription

```http
POST /api/v1/subscriptions/cancel
```

---

## Current Subscription

```http
GET /api/v1/subscriptions/current
```

---

# 15. Payment APIs

Base Path

```text
/api/v1/payments
```

---

## Create Payment Intent

```http
POST /api/v1/payments/create-intent
```

---

## Payment History

```http
GET /api/v1/payments/history
```

---

## Invoice Detail

```http
GET /api/v1/payments/invoices/{invoiceId}
```

---

# 16. Notification APIs

Base Path

```text
/api/v1/notifications
```

---

## List Notifications

```http
GET /api/v1/notifications
```

---

## Mark As Read

```http
POST /api/v1/notifications/{id}/read
```

---

# 17. Admin APIs

Base Path

```text
/api/v1/admin
```

Role Required:

```text
ADMIN
SUPER_ADMIN
```

---

## Users

```http
GET /api/v1/admin/users
```

```http
GET /api/v1/admin/users/{id}
```

```http
PUT /api/v1/admin/users/{id}
```

```http
DELETE /api/v1/admin/users/{id}
```

---

## Courses

```http
POST /api/v1/admin/courses
```

```http
PUT /api/v1/admin/courses/{id}
```

```http
DELETE /api/v1/admin/courses/{id}
```

---

## Analytics

```http
GET /api/v1/admin/analytics
```

---

## AI Usage

```http
GET /api/v1/admin/ai-usage
```

---

# 18. Webhooks

Base Path

```text
/api/v1/webhooks
```

---

## Stripe Webhook

```http
POST /api/v1/webhooks/stripe
```

Events:

```text
payment_intent.succeeded

invoice.paid

customer.subscription.updated

customer.subscription.deleted
```

---

## OpenAI Usage Webhook

```http
POST /api/v1/webhooks/openai
```

---

# 19. Error Codes

| Code | Description |
|--------|--------|
| UNAUTHORIZED | Authentication Failed |
| FORBIDDEN | Access Denied |
| NOT_FOUND | Resource Not Found |
| VALIDATION_ERROR | Invalid Input |
| RATE_LIMIT_EXCEEDED | Too Many Requests |
| SUBSCRIPTION_REQUIRED | Premium Feature |
| AI_PROVIDER_ERROR | AI Service Failed |

---

# 20. Rate Limiting

Guest

```text
30 requests/minute
```

User

```text
100 requests/minute
```

Premium

```text
500 requests/minute
```

Admin

```text
1000 requests/minute
```

---

# 21. Pagination Standard

Request

```http
GET /api/v1/courses?page=1&limit=20
```

Response

```json
{
  "data": [],
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 100,
    "totalPages": 5
  }
}
```

---

# 22. Sorting Standard

```http
?sortBy=createdAt
&sortOrder=desc
```

---

# 23. Filtering Standard

```http
?level=beginner
&status=active
```

---

# 24. API Security

Authentication:

```text
JWT
```

Encryption:

```text
TLS 1.3
```

Protection:

```text
Rate Limiting
Input Validation
CSRF Protection
XSS Protection
SQL Injection Prevention
```

---

# 25. OpenAPI Documentation

Swagger Endpoint

```http
GET /docs
```

Swagger JSON

```http
GET /docs-json
```

---

# Conclusion

API Specification bao gồm:

- Authentication APIs
- User APIs
- Course APIs
- Lesson APIs
- Conversation APIs
- Voice APIs
- Assessment APIs
- AI APIs
- RAG APIs
- Dashboard APIs
- Subscription APIs
- Payment APIs
- Notification APIs
- Admin APIs
- Webhooks

Sẵn sàng triển khai với:

- NestJS
- PostgreSQL
- Prisma
- Redis
- BullMQ
- OpenAI
- Claude
- Gemini
- Kubernetes