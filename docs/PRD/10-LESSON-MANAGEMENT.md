# 10-LESSON-MANAGEMENT.md
# Lesson Management Module Design

## AI English Speaking Platform

**Version:** 1.0  
**Module:** Lesson Management  
**Framework:** NestJS  
**Database:** PostgreSQL + Prisma  
**Cache:** Redis

---

# 1. Overview

## Purpose

Lesson Management Module chịu trách nhiệm quản lý:

- Lesson Content
- Speaking Exercises
- Vocabulary Exercises
- Grammar Exercises
- Quizzes
- Lesson Progress
- Lesson Completion
- Adaptive Learning Paths

---

# 2. Objectives

Cho phép học viên:

- Học từng bài học
- Thực hành giao tiếp
- Làm bài kiểm tra
- Nhận phản hồi từ AI
- Theo dõi tiến độ học tập

---

# 3. Lesson Architecture

```text
Course
 ↓
Module
 ↓
Lesson
 ↓
Activities
 ↓
Exercises
 ↓
Assessments
```

---

# 4. Lesson Types

Supported:

```text
Reading Lesson

Listening Lesson

Speaking Lesson

Grammar Lesson

Vocabulary Lesson

Quiz Lesson

Assessment Lesson
```

---

# 5. Lesson Structure

```text
Lesson
├── Introduction
├── Learning Content
├── Practice Activities
├── Quiz
├── Speaking Exercise
├── Assessment
└── Summary
```

---

# 6. Lesson Entity

## Fields

| Field | Type |
|---------|---------|
| title | String |
| slug | String |
| description | Text |
| lessonType | Enum |
| durationMinutes | Integer |
| difficultyLevel | Enum |

---

## Example

```json
{
  "title": "Introducing Yourself",
  "lessonType": "SPEAKING",
  "durationMinutes": 20
}
```

---

# 7. Lesson Difficulty Levels

```text
A1

A2

B1

B2

C1

C2
```

---

# 8. Activity System

A lesson bao gồm nhiều activity.

---

Examples:

```text
Read Text

Watch Video

Answer Quiz

Record Voice

AI Conversation
```

---

# 9. Activity Types

```text
READING

VIDEO

AUDIO

QUIZ

FLASHCARD

SPEAKING

AI_CHAT

ASSESSMENT
```

---

# 10. Learning Content Types

Supported:

```text
Markdown

Rich Text

Video

Audio

Image

PDF

Interactive Content
```

---

# 11. Content Rendering Flow

```text
Lesson
 ↓
Activities
 ↓
Content Renderer
 ↓
Frontend Components
```

---

# 12. Speaking Exercise

## Purpose

Luyện phát âm và phản xạ giao tiếp.

---

Flow:

```text
Prompt
 ↓
User Speaks
 ↓
Speech-to-Text
 ↓
AI Analysis
 ↓
Feedback
```

---

# 13. Speaking Exercise Types

```text
Read Aloud

Repeat Sentence

Role Play

Describe Picture

Free Speaking
```

---

# 14. Read Aloud Exercise

Student đọc đoạn văn.

AI đánh giá:

```text
Pronunciation

Fluency

Accuracy
```

---

# 15. Repeat Sentence Exercise

Flow:

```text
Play Audio
 ↓
Student Repeats
 ↓
Compare Results
 ↓
Generate Feedback
```

---

# 16. Role Play Exercise

Example:

```text
Customer ↔ Shop Assistant

Interviewer ↔ Candidate

Teacher ↔ Student
```

---

AI đóng vai nhân vật tương tác.

---

# 17. Free Speaking Exercise

Prompt Example:

```text
Tell me about your hometown.
```

---

Evaluation:

```text
Grammar

Vocabulary

Fluency

Coherence
```

---

# 18. Vocabulary Exercise

Types:

```text
Flashcards

Match Words

Fill In The Blank

Word Selection

Image Matching
```

---

# 19. Vocabulary Exercise Schema

```sql
id UUID

lesson_id UUID

question TEXT

correct_answer TEXT

options JSONB
```

---

# 20. Grammar Exercise

Supported:

```text
Multiple Choice

Sentence Correction

Fill In The Blank

Sentence Ordering
```

---

# 21. Quiz System

## Purpose

Đánh giá khả năng tiếp thu bài học.

---

Quiz Types:

```text
Single Choice

Multiple Choice

True/False

Short Answer
```

---

# 22. Quiz Scoring

Formula:

```text
Correct Answers
÷
Total Questions
×
100
```

---

Example:

```text
8 / 10

=
80%
```

---

# 23. Assessment Activities

Types:

```text
Pronunciation Assessment

Grammar Assessment

Vocabulary Assessment

Speaking Assessment
```

---

# 24. Lesson Completion Rules

## Completion Criteria

Option 1

```text
100% Activities Completed
```

---

Option 2

```text
Minimum Score Achieved
```

---

# 25. Completion Example

```text
10 Activities

Completed 10

Lesson Completed
```

---

# 26. Lesson Progress Tracking

Track:

```text
Activity Progress

Exercise Progress

Quiz Progress

Assessment Progress
```

---

# 27. Progress Schema

## lesson_progress

```sql
id UUID

user_id UUID

lesson_id UUID

progress_percentage INTEGER

completed BOOLEAN

completed_at TIMESTAMP
```

---

# 28. Activity Progress Schema

## activity_progress

```sql
id UUID

user_id UUID

activity_id UUID

status VARCHAR

score INTEGER
```

---

# 29. Learning Path Engine

Purpose:

Tạo lộ trình học phù hợp.

---

Inputs:

```text
English Level

Learning Goal

Assessment Scores

Completed Lessons
```

---

Output:

```text
Recommended Lessons
```

---

# 30. Adaptive Learning

## Example

Nếu người dùng yếu:

```text
Pronunciation
```

↓

Recommend:

```text
Pronunciation Lessons
```

---

Nếu yếu:

```text
Vocabulary
```

↓

Recommend:

```text
Vocabulary Lessons
```

---

# 31. AI Integration

Connected Systems:

```text
AI Tutor

Speaking Engine

Assessment Engine

Recommendation Engine
```

---

# 32. Lesson APIs

## Get Lesson

```http
GET /api/v1/lessons/{id}
```

---

## Start Lesson

```http
POST /api/v1/lessons/{id}/start
```

---

## Complete Lesson

```http
POST /api/v1/lessons/{id}/complete
```

---

## Lesson Progress

```http
GET /api/v1/lessons/{id}/progress
```

---

# 33. Activity APIs

## Submit Activity

```http
POST /api/v1/activities/{id}/submit
```

---

## Submit Quiz

```http
POST /api/v1/activities/{id}/quiz
```

---

## Submit Speaking

```http
POST /api/v1/activities/{id}/speaking
```

---

# 34. Lesson Database Schema

## lessons

```sql
id UUID

module_id UUID

title VARCHAR

lesson_type VARCHAR

duration_minutes INTEGER

difficulty_level VARCHAR
```

---

# 35. Activities Schema

## activities

```sql
id UUID

lesson_id UUID

activity_type VARCHAR

title VARCHAR

content JSONB

activity_order INTEGER
```

---

# 36. Quiz Questions Schema

## quiz_questions

```sql
id UUID

activity_id UUID

question TEXT

options JSONB

correct_answer TEXT
```

---

# 37. Speaking Exercises Schema

## speaking_exercises

```sql
id UUID

activity_id UUID

prompt TEXT

exercise_type VARCHAR

expected_answer TEXT
```

---

# 38. Content Versioning

Purpose:

Cho phép cập nhật nội dung mà không ảnh hưởng dữ liệu cũ.

---

Example:

```text
Lesson v1

Lesson v2

Lesson v3
```

---

# 39. Analytics Metrics

Track:

```text
Lesson Starts

Lesson Completions

Drop-Off Rate

Average Score

Average Completion Time
```

---

# 40. Lesson Dashboard

Admin Metrics:

```text
Most Popular Lessons

Lowest Completion Lessons

Highest Rated Lessons

Average Speaking Score
```

---

# 41. Redis Cache Strategy

Keys:

```text
lesson:{id}

lesson-content:{id}

lesson-progress:{userId}:{lessonId}
```

---

TTL:

| Data | TTL |
|---------|---------|
| Lesson Detail | 1 Hour |
| Lesson Content | 2 Hours |
| Progress | 15 Minutes |

---

# 42. Business Rules

## Rule 1

Lesson phải thuộc một Module.

---

## Rule 2

Lesson Order phải duy nhất trong Module.

---

## Rule 3

Completed Lesson không thể reset nếu Course khóa.

---

## Rule 4

Assessment Lesson yêu cầu hoàn thành các bài trước.

---

# 43. Event System

Events:

```text
LESSON_STARTED

LESSON_COMPLETED

QUIZ_COMPLETED

SPEAKING_COMPLETED

ASSESSMENT_COMPLETED
```

---

# 44. Queue Processing

BullMQ Jobs:

```text
Speaking Analysis

Assessment Processing

AI Feedback Generation

Analytics Aggregation
```

---

# 45. Scalability Targets

Support:

```text
1,000,000 Lessons

10,000,000 Activity Attempts

100,000 Concurrent Learners
```

---

# 46. Future Enhancements

Phase 2

```text
Interactive Simulations
```

---

Phase 3

```text
AI Generated Lessons
```

---

Phase 4

```text
Real-Time Speaking Coach
```

---

Phase 5

```text
Virtual Classroom
```

---

# 47. Integration Points

Connected To:

```text
Course Module

AI Tutor

Speaking Engine

Assessment Engine

Analytics Module

Gamification Module
```

---

# Conclusion

Lesson Management Module cung cấp:

- Lesson Delivery System
- Activity Engine
- Quiz System
- Speaking Practice
- Adaptive Learning
- Progress Tracking
- AI Integration

Được thiết kế để:

- Cá nhân hóa trải nghiệm học tập
- Tối ưu việc luyện nói tiếng Anh
- Hỗ trợ AI Feedback thời gian thực
- Scale đến hàng triệu lesson attempts

Tech Stack:

- NestJS
- PostgreSQL
- Prisma
- Redis
- BullMQ
- OpenAI
- Claude
- Gemini