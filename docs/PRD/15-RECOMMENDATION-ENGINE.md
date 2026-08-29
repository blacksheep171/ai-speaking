# 15-RECOMMENDATION-ENGINE.md
# Recommendation Engine Architecture & Design

## AI English Speaking Platform

**Version:** 1.0  
**Module:** Recommendation Engine  
**Framework:** NestJS  
**Database:** PostgreSQL + Prisma  
**Cache:** Redis  
**Queue:** BullMQ  
**AI Providers:** OpenAI, Claude, Gemini

---

# 1. Overview

## Purpose

Recommendation Engine chịu trách nhiệm:

- Personalized Learning
- Course Recommendations
- Lesson Recommendations
- Learning Path Generation
- Next Best Action (NBA)
- Weakness-Based Suggestions
- AI Coaching Recommendations

---

## Goals

Mỗi người dùng sẽ nhận được:

```text
Đúng nội dung

Đúng thời điểm

Đúng độ khó

Đúng mục tiêu học tập
```

---

# 2. High-Level Architecture

```text
User Activity
        ↓
Analytics Engine
        ↓
Feature Builder
        ↓
Recommendation Engine
        ↓
Ranking Engine
        ↓
Personalized Results
```

---

# 3. Recommendation Types

```text
Course Recommendation

Lesson Recommendation

Vocabulary Recommendation

Practice Recommendation

Assessment Recommendation

Learning Path Recommendation
```

---

# 4. Data Sources

Recommendation Engine sử dụng:

```text
User Profile

Learning Goals

English Level

Assessment Results

Learning History

Course Progress

AI Tutor Conversations

Speaking Scores
```

---

# 5. Recommendation Architecture

```text
Recommendation Engine
├── Rule Engine
├── AI Engine
├── Ranking Engine
├── Similarity Engine
└── Learning Path Engine
```

---

# 6. Personalization Inputs

## User Profile

```text
Age

Country

Native Language

English Level
```

---

## Learning Goals

```text
IELTS

TOEIC

Business English

Travel English

Conversation
```

---

# 7. Skill-Based Recommendation

Input:

```text
Grammar Score

Vocabulary Score

Speaking Score

Reading Score

Listening Score
```

---

Example:

```text
Speaking = 40
```

↓

Recommend:

```text
Speaking Lessons

Pronunciation Exercises

Role Play Practice
```

---

# 8. Weakness Detection Engine

Purpose:

```text
Detect Lowest Skills
```

---

Example:

| Skill | Score |
|---------|---------|
| Grammar | 85 |
| Vocabulary | 78 |
| Speaking | 45 |

↓

Weak Skill:

```text
Speaking
```

---

# 9. Rule-Based Recommendations

Example Rules:

```text
IF Speaking < 50

THEN Recommend Speaking Course
```

---

```text
IF IELTS Goal

THEN Recommend IELTS Courses
```

---

# 10. AI-Powered Recommendations

LLM analyzes:

```text
Learning History

Assessment Results

Conversation History

Goals
```

---

Outputs:

```text
Learning Plan

Practice Tasks

Suggested Courses
```

---

# 11. Learning Path Generator

Purpose:

```text
Generate Personalized Roadmaps
```

---

Example:

```text
Week 1

Grammar Basics

Week 2

Speaking Practice

Week 3

Vocabulary Expansion

Week 4

Mock Assessment
```

---

# 12. Next Best Action (NBA)

Purpose:

```text
Determine Next Activity
```

---

Example:

```text
Complete Lesson

Take Assessment

Practice Speaking

Review Vocabulary
```

---

# 13. NBA Flow

```text
Current State
 ↓
Skill Analysis
 ↓
Goal Matching
 ↓
Recommendation Ranking
 ↓
Next Action
```

---

# 14. Course Recommendation

Inputs:

```text
Level

Goals

Course History

Progress
```

---

Output:

```text
Top Courses
```

---

# 15. Lesson Recommendation

Inputs:

```text
Completed Lessons

Weak Skills

Recent Activity
```

---

Output:

```text
Top Lessons
```

---

# 16. Vocabulary Recommendation

Recommend:

```text
New Words

Weak Words

Frequently Mistaken Words
```

---

# 17. Practice Recommendation

Recommend:

```text
Speaking Practice

Grammar Exercises

Listening Tasks

Mock Tests
```

---

# 18. Collaborative Filtering

Purpose:

```text
Users Similar To You
```

---

Example:

```text
Users with IELTS Goal

Completed Course A

↓

Recommend Course A
```

---

# 19. Content-Based Filtering

Based On:

```text
Course Tags

Topics

Difficulty

Learning Objectives
```

---

# 20. Hybrid Recommendation

Combine:

```text
Rule-Based

+

Collaborative Filtering

+

AI Ranking
```

---

# 21. Ranking Engine

Factors:

```text
Relevance

Difficulty Match

Popularity

Completion Rate

User Preferences
```

---

# 22. Ranking Formula

Example:

```text
40% Goal Match

25% Skill Match

20% Popularity

15% Recency
```

---

# 23. Recommendation Score

Scale:

```text
0-100
```

---

Higher Score:

```text
Higher Priority
```

---

# 24. Recommendation Lifecycle

```text
User Activity
 ↓
Data Collection
 ↓
Feature Generation
 ↓
Recommendation
 ↓
User Action
 ↓
Feedback Loop
```

---

# 25. Feedback Loop

Track:

```text
Clicks

Enrollments

Lesson Starts

Lesson Completions
```

---

Used To:

```text
Improve Recommendations
```

---

# 26. Real-Time Recommendations

Trigger Events:

```text
Assessment Completed

Lesson Completed

Course Completed

Speaking Session Finished
```

---

# 27. Recommendation Context

Inputs:

```text
Current Level

Current Goal

Current Session

Recent Activities
```

---

# 28. User Features

## user_features

```sql
id UUID

user_id UUID

grammar_score NUMERIC

vocabulary_score NUMERIC

speaking_score NUMERIC

reading_score NUMERIC

listening_score NUMERIC
```

---

# 29. Recommendation Records

## recommendations

```sql
id UUID

user_id UUID

recommendation_type VARCHAR

entity_type VARCHAR

entity_id UUID

score NUMERIC

created_at TIMESTAMP
```

---

# 30. Learning Paths Schema

## learning_paths

```sql
id UUID

user_id UUID

goal VARCHAR

generated_by VARCHAR

created_at TIMESTAMP
```

---

# 31. Learning Path Steps

## learning_path_steps

```sql
id UUID

learning_path_id UUID

step_order INTEGER

entity_type VARCHAR

entity_id UUID
```

---

# 32. Recommendation APIs

## Get Recommendations

```http
GET /api/v1/recommendations
```

---

## Get Recommended Courses

```http
GET /api/v1/recommendations/courses
```

---

## Get Recommended Lessons

```http
GET /api/v1/recommendations/lessons
```

---

## Get Learning Path

```http
GET /api/v1/recommendations/learning-path
```

---

# 33. Recommendation Response

```json
{
  "courses": [],
  "lessons": [],
  "nextAction": {},
  "learningPath": {}
}
```

---

# 34. BullMQ Queues

```text
recommendation-generation

ranking-engine

feature-builder

learning-path-generator
```

---

# 35. Redis Cache Strategy

Keys:

```text
recommendation:user:{id}

learning-path:{id}

nba:{userId}
```

---

TTL:

```text
15 Minutes

30 Minutes

1 Hour
```

---

# 36. Feature Engineering

Generated Features:

```text
Completion Rate

Average Score

Practice Frequency

Preferred Topics

Weak Skills
```

---

# 37. Analytics Metrics

Track:

```text
CTR

Recommendation Acceptance

Enrollment Rate

Completion Rate
```

---

# 38. Recommendation Quality Metrics

```text
Precision

Recall

NDCG

MAP
```

---

# 39. A/B Testing

Purpose:

```text
Evaluate Recommendation Models
```

---

Experiments:

```text
Ranking Algorithms

Learning Paths

AI Suggestions
```

---

# 40. AI Cost Optimization

Strategies:

```text
Cache Recommendations

Generate On Demand

Use Smaller Models First
```

---

# 41. Performance Targets

Recommendation API:

```text
< 200ms
```

---

Learning Path Generation:

```text
< 2 Seconds
```

---

# 42. Security

Protect:

```text
User Features

Assessment Data

Learning History
```

---

Access:

```text
RBAC
```

---

# 43. Scalability Targets

Support:

```text
10 Million Users

100 Million Recommendations

1 Million Requests/Day
```

---

Architecture:

```text
Redis

BullMQ

Horizontal Scaling

Kubernetes
```

---

# 44. Future Enhancements

Phase 2

```text
Machine Learning Ranking
```

---

Phase 3

```text
Deep Learning Recommendations
```

---

Phase 4

```text
Reinforcement Learning
```

---

Phase 5

```text
Generative Learning Paths
```

---

# 45. Integration Points

Connected To:

```text
User Module

Course Module

Lesson Module

Assessment Engine

AI Tutor

Analytics Module
```

---

# 46. Success Metrics

Target:

```text
+30% Lesson Completion

+25% Speaking Practice

+20% Retention

+15% Premium Conversion
```

---

# Conclusion

Recommendation Engine cung cấp:

- Personalized Learning
- AI-Powered Recommendations
- Learning Path Generation
- Next Best Action
- Hybrid Ranking
- Real-Time Personalization

Được thiết kế để:

- Cá nhân hóa tối đa
- Tăng hiệu quả học tập
- Tăng retention
- Hỗ trợ hàng triệu người dùng

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