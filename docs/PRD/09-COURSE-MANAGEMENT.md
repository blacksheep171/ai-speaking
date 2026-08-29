# 09-COURSE-MANAGEMENT.md
# Course Management Module Design

## AI English Speaking Platform

**Version:** 1.0  
**Module:** Course Management  
**Framework:** NestJS  
**Database:** PostgreSQL + Prisma  
**Cache:** Redis

---

# 1. Overview

## Purpose

Course Management Module chịu trách nhiệm quản lý:

- Course Catalog
- Course Categories
- Course Enrollment
- Course Progress
- Course Recommendation
- Course Publishing
- Course Versioning

---

# 2. Business Objectives

Cho phép:

### Learners

- Khám phá khóa học
- Đăng ký khóa học
- Theo dõi tiến độ
- Nhận đề xuất khóa học

### Admins

- Tạo khóa học
- Chỉnh sửa khóa học
- Xuất bản khóa học
- Theo dõi hiệu suất khóa học

---

# 3. Course Hierarchy

```text
Course Category
      ↓
Course
      ↓
Module
      ↓
Lesson
      ↓
Content
```

---

# 4. Domain Architecture

```text
Course
├── Category
├── Module
├── Lesson
├── Enrollment
├── Progress
├── Review
└── Recommendation
```

---

# 5. Course Categories

## Supported Categories

```text
Daily Conversation

Grammar

Vocabulary

Pronunciation

Business English

IELTS

TOEIC

Travel English

Job Interview
```

---

## Category Structure

```text
Language Learning
│
├── Speaking
├── Grammar
├── Vocabulary
└── Exam Preparation
```

---

# 6. Course Levels

## CEFR Levels

```text
A1 Beginner

A2 Elementary

B1 Intermediate

B2 Upper Intermediate

C1 Advanced

C2 Proficient
```

---

# 7. Course Entity

## Core Fields

| Field | Type |
|---------|---------|
| title | String |
| slug | String |
| description | Text |
| thumbnailUrl | String |
| level | Enum |
| durationHours | Integer |
| status | Enum |

---

## Example

```json
{
  "title": "IELTS Speaking Mastery",
  "level": "B2",
  "durationHours": 24
}
```

---

# 8. Course Status

```text
Draft

Review

Published

Archived
```

---

## Lifecycle

```text
Draft
 ↓
Review
 ↓
Published
 ↓
Archived
```

---

# 9. Course Modules

Một khóa học có thể gồm nhiều module.

Example:

```text
IELTS Speaking

├── Introduction
├── Part 1
├── Part 2
├── Part 3
└── Mock Tests
```

---

# 10. Lesson Structure

Mỗi module gồm nhiều bài học.

```text
Module
 ├── Lesson 1
 ├── Lesson 2
 ├── Lesson 3
```

---

## Lesson Types

```text
Video

Reading

Quiz

Speaking Practice

Vocabulary Exercise

Assessment
```

---

# 11. Course Content Types

Supported:

```text
TEXT

MARKDOWN

VIDEO

AUDIO

QUIZ

PDF

IMAGE
```

---

# 12. Enrollment System

## Enrollment Flow

```text
User
 ↓
View Course
 ↓
Enroll
 ↓
Access Lessons
 ↓
Track Progress
```

---

## Rules

Free Courses

```text
Available For Everyone
```

Premium Courses

```text
Require Subscription
```

---

# 13. Enrollment Schema

## enrollments

```sql
id UUID

user_id UUID

course_id UUID

enrolled_at TIMESTAMP

status VARCHAR
```

---

## Status

```text
ACTIVE

COMPLETED

DROPPED
```

---

# 14. Course Progress Tracking

Track:

```text
Completed Lessons

Completed Modules

Completion %

Study Time
```

---

## Formula

```text
Completed Lessons
÷
Total Lessons
×
100
```

---

Example

```text
15 / 20

=
75%
```

---

# 15. Lesson Progress

## lesson_progress

```sql
id UUID

user_id UUID

lesson_id UUID

completed BOOLEAN

completed_at TIMESTAMP

progress_percentage INTEGER
```

---

# 16. Course Progress Schema

## course_progress

```sql
id UUID

user_id UUID

course_id UUID

completion_percentage INTEGER

completed_lessons INTEGER
```

---

# 17. Completion Certificates

## Eligibility

```text
100% Course Completion
```

---

## Generate

```text
PDF Certificate
```

---

Fields:

```text
Student Name

Course Name

Completion Date

Certificate ID
```

---

# 18. Course Reviews

Users có thể đánh giá khóa học.

---

## Rating

```text
1 - 5 Stars
```

---

## Review Schema

```sql
id UUID

user_id UUID

course_id UUID

rating INTEGER

review TEXT
```

---

# 19. Course Recommendation Engine

Inputs:

```text
English Level

Learning Goal

Completed Courses

Assessment Results
```

---

Outputs:

```text
Recommended Courses
```

---

## Example

User Goal:

```text
IELTS
```

↓

Recommend:

```text
IELTS Speaking

IELTS Vocabulary

IELTS Mock Test
```

---

# 20. AI-Powered Recommendations

Recommendation Sources:

```text
Course History

Assessment Scores

Weak Skills

User Goals
```

---

# 21. Course Search

Supported Filters:

```text
Category

Level

Duration

Instructor

Rating
```

---

## Example

```http
GET /courses?level=B1&category=speaking
```

---

# 22. Course Sorting

```text
Newest

Most Popular

Highest Rated

Recommended
```

---

# 23. Course Database Schema

## courses

```sql
id UUID

title VARCHAR

slug VARCHAR

description TEXT

thumbnail_url TEXT

level VARCHAR

status VARCHAR

duration_hours INTEGER

created_at TIMESTAMP
```

---

# 24. Course Categories Schema

## course_categories

```sql
id UUID

name VARCHAR

slug VARCHAR
```

---

# 25. Course Modules Schema

## course_modules

```sql
id UUID

course_id UUID

title VARCHAR

module_order INTEGER
```

---

# 26. Lessons Schema

## lessons

```sql
id UUID

module_id UUID

title VARCHAR

lesson_order INTEGER

duration_minutes INTEGER
```

---

# 27. Lesson Contents Schema

## lesson_contents

```sql
id UUID

lesson_id UUID

content_type VARCHAR

content JSONB
```

---

# 28. Course APIs

## List Courses

```http
GET /api/v1/courses
```

---

## Course Detail

```http
GET /api/v1/courses/{id}
```

---

## Enroll Course

```http
POST /api/v1/courses/{id}/enroll
```

---

## My Courses

```http
GET /api/v1/courses/my
```

---

## Recommended Courses

```http
GET /api/v1/courses/recommended
```

---

# 29. Admin APIs

## Create Course

```http
POST /api/v1/admin/courses
```

---

## Update Course

```http
PUT /api/v1/admin/courses/{id}
```

---

## Publish Course

```http
POST /api/v1/admin/courses/{id}/publish
```

---

## Archive Course

```http
POST /api/v1/admin/courses/{id}/archive
```

---

# 30. Course Versioning

## Purpose

Cho phép cập nhật khóa học mà không ảnh hưởng học viên hiện tại.

---

## Strategy

```text
Course v1

Course v2

Course v3
```

---

## User Assignment

Người dùng tiếp tục học version đã đăng ký.

---

# 31. Course Analytics

Track:

```text
Enrollments

Completion Rate

Drop Rate

Average Rating

Average Completion Time
```

---

# 32. Dashboard Metrics

Admin Dashboard

```text
Total Courses

Published Courses

Enrollments

Revenue Per Course
```

---

# 33. Redis Caching Strategy

## Keys

```text
course:{id}

course:list

course:recommended:{userId}
```

---

## TTL

| Data | TTL |
|---------|---------|
| Course Detail | 1 Hour |
| Course List | 30 Minutes |
| Recommendations | 15 Minutes |

---

# 34. Business Rules

## Rule 1

Course Title must be unique.

---

## Rule 2

Only Published courses are visible.

---

## Rule 3

Premium course requires active subscription.

---

## Rule 4

Archived course cannot accept new enrollments.

---

# 35. Search Optimization

Use:

```text
PostgreSQL Full Text Search
```

Future:

```text
Elasticsearch
```

---

# 36. Scalability Strategy

Expected:

```text
100,000 Courses

10,000,000 Lessons Viewed
```

---

Solutions:

```text
Redis Cache

Read Replicas

CDN

Background Processing
```

---

# 37. Audit Logs

Track:

```text
Course Created

Course Updated

Course Published

Course Archived

Course Deleted
```

---

# 38. Future Enhancements

Phase 2

```text
Course Bundles
```

---

Phase 3

```text
AI Generated Courses
```

---

Phase 4

```text
Adaptive Learning Paths
```

---

# 39. Integration Points

Connected To:

```text
User Module

Lesson Module

Assessment Module

AI Recommendation Engine

Analytics Module
```

---

# 40. Conclusion

Course Management Module cung cấp:

- Course Catalog
- Enrollment System
- Progress Tracking
- Reviews
- Recommendations
- Course Analytics
- Content Versioning

Được thiết kế để:

- Scale đến hàng triệu enrollment
- Hỗ trợ AI recommendation
- Tích hợp Learning Path Engine
- Sẵn sàng cho Enterprise LMS

Tech Stack:

- NestJS
- PostgreSQL
- Prisma
- Redis
- BullMQ