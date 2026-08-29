# 12-AI-TUTOR.md
# AI Tutor System Design

## AI English Speaking Platform

**Version:** 1.0  
**Module:** AI Tutor  
**Architecture:** Multi-Agent AI System  
**Framework:** NestJS  
**AI Providers:** OpenAI, Claude, Gemini  
**Memory:** Redis + PostgreSQL  
**Knowledge Base:** RAG Engine

---

# 1. Overview

## Purpose

AI Tutor là "giáo viên AI" trung tâm của nền tảng.

Nhiệm vụ:

- Dạy tiếng Anh
- Luyện giao tiếp
- Chấm điểm
- Sửa lỗi
- Xây dựng lộ trình học
- Động viên học viên
- Theo dõi tiến bộ

---

# 2. Objectives

AI Tutor phải:

```text
Natural

Helpful

Personalized

Context-Aware

Goal-Oriented
```

---

## Learning Outcomes

Giúp người dùng:

```text
Improve Speaking

Improve Pronunciation

Improve Grammar

Improve Vocabulary

Improve Fluency
```

---

# 3. High-Level Architecture

```text
User
 ↓
AI Gateway
 ↓
AI Router
 ↓
Conversation Engine
 ↓
Context Builder
 ↓
Memory Engine
 ↓
RAG Engine
 ↓
AI Provider
 ↓
Response
```

---

# 4. AI Tutor Components

```text
Conversation Engine

Prompt Engine

Memory Engine

Context Builder

Recommendation Engine

Feedback Engine

Assessment Engine
```

---

# 5. Multi-Agent Architecture

AI Tutor được chia thành nhiều AI Agents.

```text
Teacher Agent

Conversation Agent

Pronunciation Coach

Grammar Coach

Vocabulary Coach

IELTS Examiner

Business Coach

Learning Planner
```

---

# 6. Teacher Agent

Role:

```text
English Teacher
```

Responsibilities:

```text
Explain Concepts

Guide Lessons

Correct Mistakes

Provide Examples
```

---

# 7. Conversation Agent

Role:

```text
Speaking Partner
```

Responsibilities:

```text
Maintain Conversation

Ask Questions

Create Scenarios

Encourage Speaking
```

---

# 8. Pronunciation Coach

Responsibilities:

```text
Pronunciation Feedback

Accent Guidance

Stress Correction

Intonation Suggestions
```

---

# 9. Grammar Coach

Responsibilities:

```text
Detect Errors

Correct Sentences

Explain Rules

Generate Exercises
```

---

# 10. Vocabulary Coach

Responsibilities:

```text
Suggest Better Words

Teach Synonyms

Expand Vocabulary

Track Learned Words
```

---

# 11. IELTS Examiner Agent

Role:

```text
Official IELTS Examiner
```

---

Capabilities:

```text
Part 1

Part 2

Part 3

Band Estimation

Feedback
```

---

# 12. Business English Coach

Role:

```text
Business Mentor
```

---

Topics:

```text
Meetings

Emails

Negotiations

Presentations

Interviews
```

---

# 13. Learning Planner Agent

Purpose:

```text
Create Personalized Roadmaps
```

---

Inputs:

```text
Assessment Scores

Goals

Weaknesses

Progress
```

---

Outputs:

```text
Weekly Plan

Recommended Lessons

Practice Schedule
```

---

# 14. Conversation Engine

## Responsibilities

```text
Manage Dialogues

Track Context

Maintain Persona

Generate Responses
```

---

## Flow

```text
User Message
 ↓
Context Builder
 ↓
Prompt Builder
 ↓
LLM
 ↓
Response
```

---

# 15. AI Personas

Supported Personas

```text
Friendly Teacher

Strict Teacher

Native Speaker

Business Mentor

IELTS Examiner

Travel Partner

Interviewer
```

---

# 16. Persona Example

```text
You are a friendly English teacher.

Speak naturally.

Correct mistakes politely.

Encourage the student.

Keep responses concise.
```

---

# 17. Conversation Modes

```text
Free Conversation

Role Play

Interview Practice

IELTS Simulation

Business English

Travel English
```

---

# 18. Free Conversation Mode

Topics:

```text
Daily Life

Hobbies

Technology

Food

Travel

Movies
```

---

# 19. Role Play Mode

Examples:

```text
Customer ↔ Waiter

Passenger ↔ Airport Staff

Manager ↔ Employee

Doctor ↔ Patient
```

---

# 20. Interview Mode

Scenarios:

```text
Junior Developer

Senior Developer

Project Manager

Marketing Specialist
```

---

# 21. IELTS Simulation Mode

Structure:

```text
Part 1

Part 2

Part 3
```

---

Evaluation:

```text
Fluency

Grammar

Vocabulary

Pronunciation
```

---

# 22. Memory Architecture

## Short-Term Memory

Storage:

```text
Redis
```

---

Contains:

```text
Recent Messages

Current Session

Conversation Context
```

---

# 23. Long-Term Memory

Storage:

```text
PostgreSQL
```

---

Contains:

```text
Learning History

Assessments

Goals

Preferences
```

---

# 24. Memory Lifecycle

```text
Conversation
 ↓
Summary
 ↓
Memory Store
 ↓
Future Context
```

---

# 25. Context Builder

Inputs:

```text
Current Message

Conversation History

Learning Goal

English Level

Assessment History
```

---

Output:

```text
Optimized Prompt
```

---

# 26. Context Compression

Purpose:

```text
Reduce Token Usage
```

---

Method:

```text
Summarization

Conversation Compression
```

---

# 27. Prompt Engineering

Structure:

```text
System Prompt

User Context

Memory

Task

User Input
```

---

# 28. Example Prompt

```text
You are an IELTS Examiner.

Student Level:
B1

Goal:
IELTS 7.0

Task:
Conduct Part 2 Speaking Test.
```

---

# 29. AI Routing Strategy

## OpenAI

Best For:

```text
Grammar

Pronunciation Feedback

Speaking Analysis
```

---

## Claude

Best For:

```text
Long Context

Roadmaps

Deep Explanations
```

---

## Gemini

Best For:

```text
Quick Chat

Low Cost Tasks
```

---

# 30. Conversation Scoring

Metrics:

```text
Response Length

Vocabulary Variety

Grammar Accuracy

Speaking Confidence
```

---

# 31. AI Feedback Engine

Generate:

```text
Strengths

Weaknesses

Suggestions

Exercises
```

---

# 32. Example Feedback

```json
{
  "strengths": [
    "Good fluency"
  ],
  "weaknesses": [
    "Incorrect article usage"
  ],
  "recommendations": [
    "Practice countable nouns"
  ]
}
```

---

# 33. Personalized Coaching

Inputs:

```text
Weak Skills

Assessment Results

Learning Goals
```

---

Outputs:

```text
Practice Plans

Lessons

Exercises
```

---

# 34. Recommendation Engine

Recommend:

```text
Courses

Lessons

Vocabulary

Practice Sessions
```

---

# 35. Real-Time Chat Architecture

```text
Frontend
 ↓
WebSocket
 ↓
AI Gateway
 ↓
LLM
 ↓
Streaming Response
```

---

# 36. WebSocket Events

```text
chat.started

message.received

response.streaming

response.completed
```

---

# 37. AI Cost Optimization

Strategies:

```text
Caching

Context Compression

Model Routing

Response Reuse
```

---

# 38. Token Management

Track:

```text
Prompt Tokens

Completion Tokens

Total Cost
```

---

Store:

```text
AI Usage Logs
```

---

# 39. Safety Layer

Moderation:

```text
Toxic Content

Harassment

Violence

Prompt Injection
```

---

# 40. Prompt Injection Protection

Detect:

```text
Ignore Instructions

Reveal Prompt

Bypass Rules
```

---

Action:

```text
Reject Request
```

---

# 41. Database Schema

## ai_conversations

```sql
id UUID

user_id UUID

persona VARCHAR

conversation_mode VARCHAR

created_at TIMESTAMP
```

---

# 42. Messages Schema

## ai_messages

```sql
id UUID

conversation_id UUID

sender VARCHAR

content TEXT

tokens_used INTEGER

created_at TIMESTAMP
```

---

# 43. AI Feedback Schema

## ai_feedback

```sql
id UUID

conversation_id UUID

feedback JSONB

created_at TIMESTAMP
```

---

# 44. APIs

## Start Conversation

```http
POST /api/v1/ai/conversations
```

---

## Send Message

```http
POST /api/v1/ai/messages
```

---

## Get Conversation

```http
GET /api/v1/ai/conversations/{id}
```

---

## Generate Learning Plan

```http
POST /api/v1/ai/learning-plan
```

---

# 45. Analytics

Track:

```text
Conversation Count

Average Session Duration

Average Messages

Token Usage

AI Cost
```

---

# 46. Scalability Targets

Support:

```text
100,000 Concurrent Users

1,000,000 Conversations/Day
```

---

Strategies:

```text
Redis

BullMQ

Horizontal Scaling

Multi-LLM Routing
```

---

# 47. Future Enhancements

Phase 2

```text
AI Voice Tutor
```

---

Phase 3

```text
AI Avatar Teacher
```

---

Phase 4

```text
Video Conversation Tutor
```

---

Phase 5

```text
Emotion-Aware Tutor
```

---

# 48. Integration Points

Connected To:

```text
Speaking Engine

Assessment Engine

RAG Engine

Course Module

Lesson Module

Recommendation Engine
```

---

# Conclusion

AI Tutor là trung tâm của AI English Speaking Platform.

Bao gồm:

- Multi-Agent Architecture
- AI Conversation Engine
- Personalized Coaching
- Memory System
- Context Builder
- Multi-LLM Routing
- Real-Time Chat

Đảm bảo:

- Cá nhân hóa trải nghiệm học tập
- Tối ưu chi phí AI
- Mở rộng linh hoạt
- Hỗ trợ hàng triệu cuộc hội thoại

Tech Stack:

- NestJS
- PostgreSQL
- Redis
- BullMQ
- OpenAI
- Claude
- Gemini
- Kubernetes