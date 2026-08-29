# REQUIREMENTS.md
# AI English Speaking Platform — Scoped Requirements

**Version:** 1.0
**Synthesized from:** 25 PRD docs
**Date:** 2026-08-29

---

## REQ-AUTH-001: User Registration

User can create account using Email, Google OAuth, or Apple OAuth.

**Acceptance Criteria:**
- Email verification required before first login
- Password policy enforced (min 8 chars, complexity)
- Google/Apple OAuth tokens validated
- Duplicate email rejected with clear error

**Source:** 01-PRD (FR-001), 07-AUTHENTICATION

---

## REQ-AUTH-002: User Login

Secure login with JWT + Refresh Token session management.

**Acceptance Criteria:**
- JWT issued on login (short-lived, 15min)
- Refresh Token issued (long-lived, 30 days), stored in refresh_tokens table
- Session tracked in user_devices table
- Logout revokes refresh token

**Source:** 01-PRD (FR-002), 07-AUTHENTICATION, 02-TDD

---

## REQ-AUTH-003: Password Recovery

Forgot password / reset password via email link.

**Acceptance Criteria:**
- Reset link expires in 1 hour
- Link is single-use

**Source:** 01-PRD, 07-AUTHENTICATION

---

## REQ-USER-001: User Profile Management

User maintains profile: name, avatar, birth_date, country, native language, English level, learning goal, timezone.

**Acceptance Criteria:**
- Profile created automatically on registration
- English level options: Beginner, Elementary, Intermediate, Upper Intermediate, Advanced
- Learning goal options: IELTS, TOEIC, Job Interview, Daily Conversation, Business English

**Source:** 01-PRD, 08-USER-MANAGEMENT, 03-ERD

---

## REQ-SPEAKING-001: Voice Recording & Upload

User records voice in browser; system uploads audio to storage.

**Acceptance Criteria:**
- Supported formats: mp3, wav, m4a, webm
- Max file size: configurable (default 50MB)
- Upload to AWS S3 / Cloudflare R2
- Async processing via BullMQ speech_queue

**Source:** 01-PRD (FR-003), 11-SPEAKING-ENGINE, 02-TDD

---

## REQ-SPEAKING-002: Speech-to-Text Transcription

Audio file → transcript via STT engine.

**Acceptance Criteria:**
- Transcript stored in transcripts table linked to audio_file_id
- Language: English (en-US)
- Powered by Whisper or OpenAI Audio API

**Source:** 11-SPEAKING-ENGINE, 02-TDD

---

## REQ-ASSESS-001: Multi-Dimensional Speaking Assessment

System evaluates speaking on 5 dimensions.

**Acceptance Criteria:**
- Pronunciation: 0–100 (accuracy, stress, intonation, clarity)
- Fluency: 0–100 (pause frequency, speech rate, filler words, sentence completion)
- Grammar: 0–100 (tenses, articles, prepositions, sentence structure, agreement)
- Vocabulary: 0–100 (lexical diversity, vocabulary range, topic relevance, word complexity)
- Coherence: 0–100 (logical flow, topic consistency, sentence linking)
- Overall = 25% Pronunciation + 20% Fluency + 20% Grammar + 20% Vocabulary + 15% Coherence
- Feedback stored in assessment_feedbacks (strengths, weaknesses, recommendations)
- Pronunciation errors stored in pronunciation_errors table

**Source:** 01-PRD (FR-005), 05-AI-ARCHITECTURE, 14-ASSESSMENT-ENGINE, 02-TDD

---

## REQ-CONV-001: AI Multi-Turn Conversation

User engages in text or voice conversation with AI.

**Acceptance Criteria:**
- AI roles available: Teacher, Friend, Interviewer, Customer, Examiner, Business Partner, IELTS Examiner
- Conversation contexts: Job Interview, Daily Conversation, Travel, Business Meeting, IELTS Speaking, TOEIC Speaking, Customer Service, Academic Discussion
- Context memory maintained within session (Redis short-term)
- Long-term history stored in conversation_messages table
- Multi-turn context compressed to manage token usage

**Source:** 01-PRD (FR-004), 05-AI-ARCHITECTURE, 11-SPEAKING-ENGINE

---

## REQ-AI-001: Multi-LLM Routing

AI Gateway routes requests to optimal provider.

**Acceptance Criteria:**
- Routing matrix respected:
  - GPT-5: grammar correction, pronunciation feedback, knowledge search
  - Claude Sonnet/Opus: long conversation, study plan, IELTS feedback
  - Gemini Pro/Flash: quick chat, daily practice, low-cost tasks
- Fallback chain: GPT → Claude → Gemini → Internal message
- Cost tracked per request in ai_requests table
- Prompt templates versioned in ai_prompt_templates table

**Source:** 05-AI-ARCHITECTURE, 12-AI-TUTOR

---

## REQ-RAG-001: Knowledge Retrieval (RAG)

AI retrieves relevant knowledge from indexed documents.

**Acceptance Criteria:**
- Knowledge sources: Grammar, Vocabulary, IELTS Materials, Speaking Templates, Business English, Internal Lessons
- Embedding model: text-embedding-3-large (primary), text-embedding-3-small (fallback)
- Chunk size: 500 tokens, overlap 50 tokens
- Vector DB: pgvector (MVP)
- Top-K retrieval integrated into prompt building
- Retrieval logged in retrieval_logs table

**Source:** 13-RAG-DESIGN, 05-AI-ARCHITECTURE, 12-AI-TUTOR

---

## REQ-COURSE-001: Course & Lesson Management

Admin manages course catalog; users enroll and track progress.

**Acceptance Criteria:**
- Courses have: title, slug, description, thumbnail, level, status
- Lesson types: TEXT, VIDEO, QUIZ, SPEAKING, VOCABULARY
- Enrollment tracked in enrollments table
- Per-lesson progress in lesson_progress table
- Overall course progress in course_progress table

**Source:** 09-COURSE-MANAGEMENT, 10-LESSON-MANAGEMENT, 03-ERD

---

## REQ-REC-001: Personalized Learning Recommendations

AI recommends next lessons, topics, and exercises based on assessment history.

**Acceptance Criteria:**
- Input: assessment history, weak skills, learning goals, conversation performance
- Output: ranked list of lessons, topics, vocabulary lists
- Recommendations updated after each assessment

**Source:** 01-PRD (FR-006), 15-RECOMMENDATION-ENGINE

---

## REQ-GAMIF-001: Gamification System

Achievements and streaks to drive retention.

**Acceptance Criteria:**
- Achievements unlocked: First Conversation, First Week, 7-Day Streak, 30-Day Streak, 100 Conversations, 1000 Minutes Speaking
- Streak tracked in user_streaks table (current_streak, longest_streak)
- Achievements stored in user_achievements table

**Source:** 01-PRD (FR-008), 16-GAMIFICATION-SYSTEM, 03-ERD

---

## REQ-SUB-001: Subscription Plans

Free and Premium plans with quota enforcement.

**Acceptance Criteria:**
- Free: 10 AI conversations/day limit
- Premium Monthly: $9.99/month — unlimited conversations, advanced analytics, personalized roadmap
- Premium Yearly: $79/year
- Corporate: custom pricing
- Quota tracked in user_subscriptions + subscription_plans (ai_usage_limit)
- Plan expiry enforced

**Source:** 01-PRD (FR-009), 19-SUBSCRIPTION-BILLING

---

## REQ-PAY-001: Payment Processing

Secure payment for subscription upgrades.

**Acceptance Criteria:**
- Payment stored in payments table (amount, currency, provider, status)
- Invoice generated and stored in invoices table
- Payment providers: Stripe (primary), configurable

**Source:** 19-SUBSCRIPTION-BILLING, 03-ERD

---

## REQ-NOTIF-001: Notification System

Multi-channel notifications (Email, Push, In-App).

**Acceptance Criteria:**
- Email via SendGrid or AWS SES
- Push notifications for daily reminders and streak alerts
- In-app notifications stored in notifications table
- Templates in notification_templates table
- Processed via BullMQ notification_queue

**Source:** 17-NOTIFICATION-SYSTEM, 06-SYSTEM-ARCHITECTURE

---

## REQ-ANALYTICS-001: Analytics & Reporting

Track user events and generate statistics.

**Acceptance Criteria:**
- Events tracked: LOGIN, START_LESSON, COMPLETE_LESSON, START_CONVERSATION, FINISH_CONVERSATION, UPGRADE_PLAN
- Daily statistics: active_users, new_users, total_conversations
- Admin dashboard with real-time metrics

**Source:** 18-ANALYTICS-SYSTEM, 03-ERD

---

## REQ-ADMIN-001: Admin Portal

Full admin capabilities.

**Acceptance Criteria:**
- Manage users (view, suspend, role change)
- Manage courses and lessons
- Manage AI prompt templates (versioned)
- View analytics dashboard
- Manage subscription plans
- Audit log stored in admin_audit_logs table

**Source:** 01-PRD, 08-USER-MANAGEMENT, 09-COURSE-MANAGEMENT

---

## REQ-DASHBOARD-001: User Dashboard

User sees learning progress at a glance.

**Acceptance Criteria:**
- Current speaking scores (radar/spider chart)
- Streak display
- Recent conversation history
- Weekly practice report
- Recommendations widget

**Source:** 01-PRD, 18-ANALYTICS-SYSTEM

---

## REQ-INFRA-001: Infrastructure & DevOps

Cloud-native deployment.

**Acceptance Criteria:**
- Docker containers for all apps
- Kubernetes namespaces: frontend, backend, redis, postgres, monitoring
- ArgoCD GitOps sync from infra/kubernetes
- GitHub Actions CI/CD: test → build → Docker image → ArgoCD sync
- Prometheus + Grafana + Loki for observability

**Source:** 20-DEVOPS-INFRASTRUCTURE, 02-TDD, 24-DEPLOYMENT-RUNBOOK

---

## REQ-SEC-001: Security Requirements

OWASP Top 10 compliance.

**Acceptance Criteria:**
- XSS protection headers
- CSRF protection
- SQL injection prevention via Prisma parameterized queries
- Input validation on all endpoints
- AES-256 data encryption at rest
- TLS 1.3 in transit
- Rate limiting: 100 req/min per IP

**Source:** 21-SECURITY-ARCHITECTURE, 02-TDD

---

## REQ-TEST-001: Testing Strategy

Comprehensive test coverage.

**Acceptance Criteria:**
- Unit tests: 70%+ coverage per module
- Integration tests: all API endpoints
- E2E tests: critical user flows (register, speak, assess, subscribe)
- Load testing: 10,000 concurrent users

**Source:** 22-TESTING-STRATEGY
