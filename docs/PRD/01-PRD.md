# 01-PRD.md
# Product Requirements Document (PRD)

## AI English Speaking Platform

**Version:** 1.0  
**Status:** Draft  
**Target Release:** MVP v1.0

---

# 1. Executive Summary

## Product Name

AI English Speaking Platform

## Product Vision

Xây dựng nền tảng học tiếng Anh giao tiếp sử dụng AI giúp người học:

- Luyện nói mọi lúc
- Nhận phản hồi tức thời
- Mô phỏng hội thoại thực tế
- Đánh giá trình độ tự động
- Cá nhân hóa lộ trình học

Mục tiêu cuối cùng:

> Giúp người dùng đạt khả năng giao tiếp tiếng Anh tự tin tương đương việc học cùng giáo viên 1:1 nhưng với chi phí thấp hơn rất nhiều.

---

# 2. Business Goals

## Year 1

### User Goals

- 50,000 registered users
- 10,000 monthly active users
- 2,000 paid subscribers

### Revenue Goals

- Monthly Subscription
- Premium AI Tutor
- Corporate Training Packages

Target Revenue:

- $100,000 ARR

---

# 3. Problem Statement

## Current Challenges

Người học tiếng Anh hiện nay gặp các vấn đề:

### Problem 1

Thiếu môi trường luyện nói.

### Problem 2

Ngại giao tiếp với người thật.

### Problem 3

Không biết phát âm sai ở đâu.

### Problem 4

Chi phí giáo viên cao.

### Problem 5

Không có lộ trình học cá nhân hóa.

---

# 4. Solution Overview

Hệ thống AI English Speaking Platform cung cấp:

## AI Conversation

Người dùng trò chuyện với AI.

Các ngữ cảnh:

- Job Interview
- Daily Conversation
- Travel
- Business Meeting
- IELTS Speaking
- TOEIC Speaking
- Customer Service
- Academic Discussion

## AI Pronunciation Evaluation

AI đánh giá:

- Pronunciation
- Fluency
- Grammar
- Vocabulary
- Coherence

## AI Tutor

AI đóng vai:

- Teacher
- Interviewer
- Customer
- Friend
- Business Partner
- IELTS Examiner

## Personalized Learning

AI phân tích:

- Điểm mạnh
- Điểm yếu
- Tiến độ học tập

Sau đó đề xuất:

- Bài học
- Từ vựng
- Chủ đề luyện tập
- Kế hoạch học tập

---

# 5. Product Scope

## MVP Scope

### Authentication

- Register
- Login
- Logout
- Forgot Password
- Reset Password
- Email Verification
- Google Login
- Apple Login

### User Profile

- Personal Information
- Learning Goal
- English Level
- Learning Preferences
- Timezone
- Avatar

### Speaking Practice

- Voice Recording
- Speech To Text
- Pronunciation Analysis
- AI Feedback

### AI Conversation

- Text Chat
- Voice Chat
- Multi-turn Conversation
- Context Memory

### Dashboard

- Learning Progress
- Daily Activity
- Speaking Score
- Weekly Report

### Subscription

- Free Plan
- Premium Plan

---

# 6. Out Of Scope

Không nằm trong MVP:

- Live Teacher Classes
- Marketplace
- Offline Courses
- Native Mobile App
- Video Calling
- Corporate LMS

---

# 7. User Personas

## Persona A - Student

### Demographic

- Age: 18-25
- University Student

### Goals

- IELTS
- Study Abroad
- Scholarship

### Pain Points

- Speaking Anxiety
- Poor Pronunciation
- Lack of Speaking Partners

---

## Persona B - Working Professional

### Demographic

- Age: 25-40

### Goals

- Job Interview
- International Communication
- Career Advancement

### Pain Points

- Lack of Practice
- Busy Schedule

---

## Persona C - Beginner

### Demographic

- Age: 18-50

### Goals

- Daily Communication

### Pain Points

- No Confidence
- Small Vocabulary
- Fear of Making Mistakes

---

# 8. User Roles

## Guest

Can:

- Browse Landing Page
- View Pricing
- View Courses

Cannot:

- Use AI Features

---

## Free User

Can:

- 10 AI Conversations / Day
- Basic Reports
- Limited Speaking Sessions

---

## Premium User

Can:

- Unlimited Conversations
- Advanced Analytics
- Personalized Roadmap
- Priority AI Models

---

## Admin

Can:

- Manage Users
- Manage Courses
- Manage Lessons
- Manage Prompts
- View Analytics
- Manage Subscription Plans

---

# 9. Functional Requirements

## FR-001 Registration

User can create account using:

- Email
- Google
- Apple

Acceptance Criteria:

- Email verification required
- Password policy enforced

---

## FR-002 Login

User can login securely.

Requirements:

- JWT
- Refresh Token
- Session Management

---

## FR-003 Voice Recording

User records voice.

System:

1. Upload audio
2. Process audio
3. Generate transcript
4. Store result

---

## FR-004 AI Conversation

User sends message.

System:

1. Receive input
2. Load conversation history
3. Build context
4. Query AI
5. Return answer

---

## FR-005 Speaking Assessment

System evaluates:

### Pronunciation

Score: 0-100

### Fluency

Score: 0-100

### Grammar

Score: 0-100

### Vocabulary

Score: 0-100

### Coherence

Score: 0-100

### Overall

Score: 0-100

---

## FR-006 Learning Roadmap

AI recommends:

- Lessons
- Topics
- Exercises

Based on:

- User Level
- Historical Performance
- Weak Skills

---

## FR-007 Progress Tracking

Track:

- Daily Practice
- Weekly Practice
- Monthly Practice
- Streak

---

## FR-008 Achievement System

Achievements:

- First Conversation
- First Week
- 7 Day Streak
- 30 Day Streak
- 100 Conversations
- 1000 Minutes Speaking

---

## FR-009 Subscription

Plans:

### Free

- 10 Sessions / Day

### Premium

- Unlimited Sessions

---

# 10. Non Functional Requirements

## Availability

- 99.9%

## Scalability

Support:

- 100,000 concurrent users

## Response Time

### API

- < 500ms

### AI Response

- < 5 seconds

## Security

- OWASP Top 10 Compliance
- Rate Limiting
- Data Encryption

## Reliability

- No Single Point Of Failure

---

# 11. User Journey

## New User Journey

Step 1

Register Account

↓

Step 2

Complete English Assessment

↓

Step 3

Receive Learning Roadmap

↓

Step 4

Start Speaking Practice

↓

Step 5

Receive AI Feedback

↓

Step 6

Track Progress

↓

Step 7

Upgrade Subscription

---

# 12. Speaking Practice Flow

```text
User
 ↓
Record Voice
 ↓
Upload Audio
 ↓
Speech To Text
 ↓
Transcript
 ↓
AI Analysis
 ↓
Feedback
 ↓
Score
 ↓
Dashboard
```

---

# 13. Success Metrics

## Product Metrics

### DAU

Target:

- 5,000+

### MAU

Target:

- 10,000+

### Retention

Day 30 Retention:

- > 35%

### Subscription Conversion

Target:

- > 5%

---

# 14. Monetization

## Free Plan

- Limited AI Usage

## Premium Monthly

- $9.99/month

## Premium Yearly

- $79/year

## Corporate Plan

- Custom Pricing

---

# 15. Risk Assessment

## Risk 1

AI Cost Too High

Mitigation:

- Caching
- Model Routing
- Prompt Optimization

---

## Risk 2

Low Retention

Mitigation:

- Gamification
- Daily Goals
- Push Notifications

---

## Risk 3

Scaling Issues

Mitigation:

- Kubernetes
- Redis
- BullMQ

---

# 16. Technology Stack

## Frontend

- NextJS
- TypeScript
- TailwindCSS
- Zustand
- React Query

## Backend

- NestJS
- TypeScript

## Database

- PostgreSQL

## ORM

- Prisma

## Cache

- Redis

## Queue

- BullMQ

## AI

- OpenAI
- Claude
- Gemini

## Retrieval

- RAG
- Vector Database

## Infrastructure

- Docker
- Kubernetes
- ArgoCD

---

# 17. MVP Deliverables

### Authentication

- Complete

### Speaking Engine

- Complete

### AI Chat

- Complete

### Dashboard

- Complete

### Subscription

- Complete

### Admin Portal

- Complete

---

# 18. Future Roadmap

## Phase 2

- IELTS Coach
- TOEIC Coach
- Business English

## Phase 3

- AI Video Avatar
- Virtual Classroom
- AI Teacher Marketplace

## Phase 4

- Mobile Apps
- Smart Wearable Integration
- Enterprise LMS

---

# 19. Acceptance Criteria

MVP được xem là hoàn thành khi:

- User đăng ký thành công
- User có thể nói chuyện với AI
- AI đánh giá phát âm
- Dashboard hiển thị tiến độ
- Thanh toán hoạt động
- Hệ thống chịu tải 10,000 users

---

# 20. Conclusion

AI English Speaking Platform là hệ thống học tiếng Anh thế hệ mới sử dụng:

- OpenAI
- Claude
- Gemini
- RAG

để tạo trải nghiệm học giao tiếp cá nhân hóa, mở rộng quy mô lớn và tối ưu chi phí vận hành.