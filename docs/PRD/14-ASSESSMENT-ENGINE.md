# 14-ASSESSMENT-ENGINE.md
# Assessment Engine Architecture & Design

## AI English Speaking Platform

**Version:** 1.0  
**Module:** Assessment Engine  
**Framework:** NestJS  
**Database:** PostgreSQL + Prisma  
**Cache:** Redis  
**Queue:** BullMQ  
**AI Providers:** OpenAI, Claude, Gemini

---

# 1. Overview

## Purpose

Assessment Engine chịu trách nhiệm:

- English Placement Test
- Speaking Assessment
- Grammar Assessment
- Vocabulary Assessment
- Listening Assessment
- Reading Assessment
- CEFR Classification
- IELTS Band Estimation
- Progress Evaluation

---

# 2. Business Objectives

Cho phép hệ thống:

```text
Determine User Level

Identify Weak Skills

Track Progress

Recommend Learning Paths

Measure Learning Outcomes
```

---

# 3. Assessment Architecture

```text
User
 ↓
Assessment Module
 ↓
Question Engine
 ↓
Answer Collection
 ↓
Scoring Engine
 ↓
AI Evaluation
 ↓
Result Generator
 ↓
Recommendation Engine
```

---

# 4. Assessment Types

Supported:

```text
Placement Test

Speaking Test

Grammar Test

Vocabulary Test

Reading Test

Listening Test

Mock IELTS Test

Progress Test
```

---

# 5. Placement Test

## Purpose

Xác định trình độ ban đầu.

---

Output:

```text
A1

A2

B1

B2

C1

C2
```

---

# 6. Placement Test Structure

```text
Grammar Questions

Vocabulary Questions

Reading Questions

Listening Questions

Speaking Questions
```

---

# 7. Assessment Lifecycle

```text
Created
 ↓
Started
 ↓
In Progress
 ↓
Submitted
 ↓
Evaluated
 ↓
Completed
```

---

# 8. Assessment Domain Model

```text
Assessment
├── Sections
├── Questions
├── Attempts
├── Answers
├── Scores
└── Feedback
```

---

# 9. Assessment Sections

Example:

```text
Grammar

Vocabulary

Speaking

Listening

Reading
```

---

# 10. Question Types

Supported:

```text
Single Choice

Multiple Choice

True/False

Fill In The Blank

Short Answer

Essay

Speaking Response
```

---

# 11. Question Difficulty

Levels:

```text
Easy

Medium

Hard

Expert
```

---

Mapped To:

```text
A1-C2
```

---

# 12. Adaptive Assessment

Purpose:

```text
Dynamic Difficulty Adjustment
```

---

Flow:

```text
Correct Answer
 ↓
Increase Difficulty

Wrong Answer
 ↓
Decrease Difficulty
```

---

# 13. Grammar Assessment

Evaluate:

```text
Tenses

Articles

Prepositions

Sentence Structure

Subject-Verb Agreement
```

---

# 14. Vocabulary Assessment

Evaluate:

```text
Vocabulary Size

Word Usage

Synonyms

Collocations
```

---

# 15. Reading Assessment

Evaluate:

```text
Comprehension

Inference

Main Ideas

Details
```

---

# 16. Listening Assessment

Evaluate:

```text
Comprehension

Accent Understanding

Information Extraction
```

---

# 17. Speaking Assessment

Integrated With:

```text
Speaking Engine
```

---

Metrics:

```text
Pronunciation

Fluency

Grammar

Vocabulary

Coherence
```

---

# 18. Writing Assessment (Future)

Evaluate:

```text
Grammar

Vocabulary

Structure

Coherence

Task Achievement
```

---

# 19. Scoring Engine

## Standard Scale

```text
0 - 100
```

---

# 20. Section Score Formula

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
18 / 20

=
90
```

---

# 21. Weighted Assessment Score

Example:

```text
Grammar      25%

Vocabulary   25%

Listening    20%

Reading      15%

Speaking     15%
```

---

# 22. CEFR Mapping

| Score | CEFR |
|---------|---------|
| 0-30 | A1 |
| 31-45 | A2 |
| 46-60 | B1 |
| 61-75 | B2 |
| 76-90 | C1 |
| 91-100 | C2 |

---

# 23. IELTS Band Mapping

| Score | IELTS |
|---------|---------|
| 0-40 | 3.0 |
| 41-50 | 4.0 |
| 51-60 | 5.0 |
| 61-70 | 6.0 |
| 71-80 | 7.0 |
| 81-90 | 8.0 |
| 91-100 | 9.0 |

---

# 24. Skill Analysis

Track:

```text
Grammar Score

Vocabulary Score

Reading Score

Listening Score

Speaking Score
```

---

# 25. Weakness Detection

Example:

```text
Grammar: 85

Vocabulary: 80

Speaking: 45
```

↓

Weak Skill:

```text
Speaking
```

---

# 26. Recommendation Generation

Input:

```text
Assessment Scores

Goals

Learning History
```

---

Output:

```text
Recommended Courses

Recommended Lessons

Practice Tasks
```

---

# 27. AI Evaluation Layer

Used For:

```text
Essay Evaluation

Speaking Feedback

Personalized Analysis
```

---

# 28. AI Feedback Example

```json
{
  "strengths": [
    "Strong grammar knowledge"
  ],
  "weaknesses": [
    "Limited vocabulary range"
  ],
  "recommendations": [
    "Practice advanced vocabulary exercises"
  ]
}
```

---

# 29. Progress Assessment

Frequency:

```text
Weekly

Monthly

Quarterly
```

---

Purpose:

```text
Track Learning Improvement
```

---

# 30. Benchmark Comparisons

Compare:

```text
Current Score

Previous Score

Target Score
```

---

# 31. Assessment Result Report

Contains:

```text
Overall Score

CEFR Level

IELTS Estimate

Skill Breakdown

Recommendations
```

---

# 32. Assessment Entity

## assessments

```sql
id UUID

title VARCHAR

assessment_type VARCHAR

status VARCHAR

created_at TIMESTAMP
```

---

# 33. Assessment Sections Schema

## assessment_sections

```sql
id UUID

assessment_id UUID

title VARCHAR

section_order INTEGER
```

---

# 34. Questions Schema

## assessment_questions

```sql
id UUID

section_id UUID

question_type VARCHAR

difficulty VARCHAR

question TEXT
```

---

# 35. Assessment Attempts

## assessment_attempts

```sql
id UUID

user_id UUID

assessment_id UUID

started_at TIMESTAMP

completed_at TIMESTAMP

status VARCHAR
```

---

# 36. Assessment Answers

## assessment_answers

```sql
id UUID

attempt_id UUID

question_id UUID

answer JSONB

score NUMERIC
```

---

# 37. Assessment Results

## assessment_results

```sql
id UUID

attempt_id UUID

overall_score NUMERIC

cefr_level VARCHAR

ielts_band NUMERIC
```

---

# 38. Skill Scores

## assessment_skill_scores

```sql
id UUID

result_id UUID

grammar_score NUMERIC

vocabulary_score NUMERIC

reading_score NUMERIC

listening_score NUMERIC

speaking_score NUMERIC
```

---

# 39. Assessment APIs

## List Assessments

```http
GET /api/v1/assessments
```

---

## Start Assessment

```http
POST /api/v1/assessments/{id}/start
```

---

## Submit Assessment

```http
POST /api/v1/assessments/{id}/submit
```

---

## Get Result

```http
GET /api/v1/assessments/results/{id}
```

---

# 40. Analytics Metrics

Track:

```text
Assessment Attempts

Average Scores

Pass Rate

Skill Distribution

Level Distribution
```

---

# 41. Redis Cache Strategy

Keys:

```text
assessment:{id}

assessment-result:{id}

placement-test
```

---

TTL:

```text
30 Minutes

1 Hour
```

---

# 42. BullMQ Jobs

Queues:

```text
assessment-scoring

speaking-evaluation

feedback-generation

report-generation
```

---

# 43. Performance Targets

Assessment Submission:

```text
< 2 Seconds
```

---

Result Generation:

```text
< 5 Seconds
```

---

Placement Test:

```text
< 10 Seconds
```

---

# 44. Security

Protect:

```text
Question Bank

Assessment Answers

User Scores
```

---

Prevent:

```text
Cheating

Answer Leakage

Replay Attacks
```

---

# 45. Scalability Targets

Support:

```text
1,000,000 Assessments

100,000 Concurrent Users

10,000 Submissions/Minute
```

---

Architecture:

```text
Horizontal Scaling

Redis

BullMQ

Kubernetes
```

---

# 46. Future Enhancements

Phase 2

```text
Adaptive Testing
```

---

Phase 3

```text
Computerized Adaptive Testing (CAT)
```

---

Phase 4

```text
Writing Assessment Engine
```

---

Phase 5

```text
AI Proctoring
```

---

# 47. Integration Points

Connected To:

```text
User Module

Course Module

Lesson Module

Speaking Engine

AI Tutor

Recommendation Engine
```

---

# Conclusion

Assessment Engine cung cấp:

- Placement Tests
- Speaking Evaluation
- Grammar Assessment
- Vocabulary Assessment
- CEFR Classification
- IELTS Band Estimation
- Personalized Recommendations

Được thiết kế để:

- Đánh giá chính xác trình độ người học
- Cá nhân hóa lộ trình học tập
- Theo dõi tiến bộ dài hạn
- Hỗ trợ hàng triệu bài kiểm tra

Tech Stack:

- NestJS
- PostgreSQL
- Prisma
- Redis
- BullMQ
- OpenAI
- Claude
- Gemini
- Kubernetes