# 03-ERD.md
# Entity Relationship Diagram (ERD)

## AI English Speaking Platform

**Database:** PostgreSQL 16  
**ORM:** Prisma ORM  
**Naming Convention:** snake_case  
**Primary Key:** UUID  
**Timezone:** UTC

---

# 1. Database Overview

Hệ thống được chia thành các domain chính:

```text
Authentication
Users
Courses
Lessons
Conversations
AI Assessment
Learning Progress
Subscriptions
Payments
Notifications
Analytics
Administration
```

---

# 2. High Level ERD

```text
Users
 |
 +---- UserProfiles
 |
 +---- UserSettings
 |
 +---- UserSubscriptions
 |
 +---- UserAchievements
 |
 +---- Conversations
 |          |
 |          +---- Messages
 |
 +---- Assessments
 |
 +---- LearningProgress
 |
 +---- Enrollments
            |
            +---- Courses
                        |
                        +---- Lessons

Payments
 |
 +---- Invoices

Notifications

AnalyticsEvents
```

---

# 3. Core Tables

---

# users

Lưu thông tin tài khoản.

| Column | Type |
|----------|----------|
| id | UUID |
| email | VARCHAR(255) |
| password_hash | TEXT |
| role | VARCHAR(50) |
| status | VARCHAR(50) |
| email_verified | BOOLEAN |
| last_login_at | TIMESTAMP |
| created_at | TIMESTAMP |
| updated_at | TIMESTAMP |

---

## Constraints

```sql
UNIQUE(email)
```

---

## Indexes

```sql
idx_users_email
idx_users_role
idx_users_status
```

---

# user_profiles

Thông tin cá nhân.

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| first_name | VARCHAR |
| last_name | VARCHAR |
| avatar_url | TEXT |
| birth_date | DATE |
| country | VARCHAR |
| native_language | VARCHAR |
| english_level | VARCHAR |
| learning_goal | VARCHAR |
| created_at | TIMESTAMP |

---

## Relationship

```text
User 1 → 1 UserProfile
```

---

# user_settings

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| preferred_language | VARCHAR |
| notification_enabled | BOOLEAN |
| dark_mode | BOOLEAN |
| timezone | VARCHAR |

---

# user_devices

Theo dõi thiết bị đăng nhập.

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| device_name | VARCHAR |
| device_type | VARCHAR |
| ip_address | VARCHAR |
| last_active_at | TIMESTAMP |

---

# refresh_tokens

Lưu refresh token.

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| token | TEXT |
| expires_at | TIMESTAMP |
| revoked_at | TIMESTAMP |

---

# 4. Course Domain

---

# courses

| Column | Type |
|----------|----------|
| id | UUID |
| title | VARCHAR |
| slug | VARCHAR |
| description | TEXT |
| thumbnail_url | TEXT |
| level | VARCHAR |
| status | VARCHAR |
| created_at | TIMESTAMP |

---

## Example Levels

```text
Beginner
Elementary
Intermediate
Upper Intermediate
Advanced
```

---

# course_categories

| Column | Type |
|----------|----------|
| id | UUID |
| name | VARCHAR |
| slug | VARCHAR |

---

# course_category_relations

N-N Relationship

| Column | Type |
|----------|----------|
| course_id | UUID |
| category_id | UUID |

---

# lessons

| Column | Type |
|----------|----------|
| id | UUID |
| course_id | UUID |
| title | VARCHAR |
| description | TEXT |
| lesson_order | INTEGER |
| duration_minutes | INTEGER |
| created_at | TIMESTAMP |

---

## Relationship

```text
Course 1 → N Lessons
```

---

# lesson_contents

| Column | Type |
|----------|----------|
| id | UUID |
| lesson_id | UUID |
| content_type | VARCHAR |
| content | JSONB |

---

## Types

```text
TEXT
VIDEO
QUIZ
SPEAKING
VOCABULARY
```

---

# 5. Enrollment Domain

---

# enrollments

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| course_id | UUID |
| enrolled_at | TIMESTAMP |

---

# lesson_progress

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| lesson_id | UUID |
| completed | BOOLEAN |
| completed_at | TIMESTAMP |
| progress_percentage | INTEGER |

---

# course_progress

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| course_id | UUID |
| completed_lessons | INTEGER |
| completion_percentage | INTEGER |

---

# 6. Conversation Domain

---

# conversations

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| title | VARCHAR |
| topic | VARCHAR |
| ai_role | VARCHAR |
| status | VARCHAR |
| created_at | TIMESTAMP |

---

## Example AI Roles

```text
Teacher
Friend
Interviewer
Examiner
Customer
```

---

# conversation_messages

| Column | Type |
|----------|----------|
| id | UUID |
| conversation_id | UUID |
| sender_type | VARCHAR |
| message | TEXT |
| token_usage | INTEGER |
| model_name | VARCHAR |
| created_at | TIMESTAMP |

---

## Sender Types

```text
USER
AI
SYSTEM
```

---

# conversation_contexts

Lưu context phục vụ AI.

| Column | Type |
|----------|----------|
| id | UUID |
| conversation_id | UUID |
| context_json | JSONB |
| created_at | TIMESTAMP |

---

# 7. Audio Domain

---

# audio_files

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| conversation_id | UUID |
| file_url | TEXT |
| file_size | BIGINT |
| duration_seconds | INTEGER |
| mime_type | VARCHAR |
| created_at | TIMESTAMP |

---

# transcripts

| Column | Type |
|----------|----------|
| id | UUID |
| audio_file_id | UUID |
| transcript_text | TEXT |
| language | VARCHAR |
| created_at | TIMESTAMP |

---

# 8. Assessment Domain

---

# assessments

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| conversation_id | UUID |
| overall_score | NUMERIC(5,2) |
| created_at | TIMESTAMP |

---

# assessment_details

| Column | Type |
|----------|----------|
| id | UUID |
| assessment_id | UUID |
| pronunciation_score | NUMERIC |
| fluency_score | NUMERIC |
| grammar_score | NUMERIC |
| vocabulary_score | NUMERIC |
| coherence_score | NUMERIC |

---

# assessment_feedbacks

| Column | Type |
|----------|----------|
| id | UUID |
| assessment_id | UUID |
| strengths | TEXT |
| weaknesses | TEXT |
| recommendations | TEXT |

---

# pronunciation_errors

| Column | Type |
|----------|----------|
| id | UUID |
| assessment_id | UUID |
| word | VARCHAR |
| expected_pronunciation | VARCHAR |
| actual_pronunciation | VARCHAR |
| severity | VARCHAR |

---

# 9. AI Domain

---

# ai_models

| Column | Type |
|----------|----------|
| id | UUID |
| provider | VARCHAR |
| model_name | VARCHAR |
| active | BOOLEAN |

---

## Providers

```text
OPENAI
CLAUDE
GEMINI
```

---

# ai_requests

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| model_id | UUID |
| prompt_tokens | INTEGER |
| completion_tokens | INTEGER |
| total_tokens | INTEGER |
| cost | NUMERIC |
| latency_ms | INTEGER |
| created_at | TIMESTAMP |

---

# ai_prompt_templates

| Column | Type |
|----------|----------|
| id | UUID |
| name | VARCHAR |
| template_content | TEXT |
| version | INTEGER |

---

# 10. RAG Domain

---

# knowledge_documents

| Column | Type |
|----------|----------|
| id | UUID |
| title | VARCHAR |
| content | TEXT |
| category | VARCHAR |
| source | VARCHAR |

---

# document_chunks

| Column | Type |
|----------|----------|
| id | UUID |
| document_id | UUID |
| chunk_content | TEXT |
| chunk_order | INTEGER |

---

# document_embeddings

| Column | Type |
|----------|----------|
| id | UUID |
| chunk_id | UUID |
| embedding_vector | VECTOR |
| created_at | TIMESTAMP |

---

# retrieval_logs

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| query_text | TEXT |
| retrieved_documents | JSONB |
| created_at | TIMESTAMP |

---

# 11. Subscription Domain

---

# subscription_plans

| Column | Type |
|----------|----------|
| id | UUID |
| name | VARCHAR |
| price | NUMERIC |
| duration_days | INTEGER |
| ai_usage_limit | INTEGER |

---

# user_subscriptions

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| plan_id | UUID |
| start_date | TIMESTAMP |
| end_date | TIMESTAMP |
| status | VARCHAR |

---

# 12. Payment Domain

---

# payments

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| subscription_id | UUID |
| amount | NUMERIC |
| currency | VARCHAR |
| payment_provider | VARCHAR |
| payment_status | VARCHAR |

---

# invoices

| Column | Type |
|----------|----------|
| id | UUID |
| payment_id | UUID |
| invoice_number | VARCHAR |
| invoice_url | TEXT |

---

# 13. Gamification Domain

---

# achievements

| Column | Type |
|----------|----------|
| id | UUID |
| title | VARCHAR |
| description | TEXT |
| icon_url | TEXT |

---

# user_achievements

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| achievement_id | UUID |
| unlocked_at | TIMESTAMP |

---

# user_streaks

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| current_streak | INTEGER |
| longest_streak | INTEGER |

---

# 14. Notification Domain

---

# notifications

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| title | VARCHAR |
| message | TEXT |
| is_read | BOOLEAN |

---

# notification_templates

| Column | Type |
|----------|----------|
| id | UUID |
| name | VARCHAR |
| channel | VARCHAR |
| content | TEXT |

---

# 15. Analytics Domain

---

# analytics_events

| Column | Type |
|----------|----------|
| id | UUID |
| user_id | UUID |
| event_name | VARCHAR |
| event_data | JSONB |
| created_at | TIMESTAMP |

---

## Examples

```text
LOGIN
START_LESSON
COMPLETE_LESSON
START_CONVERSATION
FINISH_CONVERSATION
UPGRADE_PLAN
```

---

# daily_statistics

| Column | Type |
|----------|----------|
| id | UUID |
| date | DATE |
| active_users | INTEGER |
| new_users | INTEGER |
| total_conversations | INTEGER |

---

# 16. Admin Domain

---

# admin_audit_logs

| Column | Type |
|----------|----------|
| id | UUID |
| admin_id | UUID |
| action | VARCHAR |
| target_type | VARCHAR |
| target_id | UUID |
| metadata | JSONB |
| created_at | TIMESTAMP |

---

# 17. Recommended PostgreSQL Extensions

```sql
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS vector;
CREATE EXTENSION IF NOT EXISTS pg_trgm;
```

---

# 18. Database Relationships Summary

```text
User
 ├── UserProfile
 ├── UserSettings
 ├── UserDevices
 ├── RefreshTokens
 ├── Conversations
 ├── Assessments
 ├── Enrollments
 ├── UserSubscriptions
 ├── UserAchievements
 ├── Notifications
 └── AnalyticsEvents

Course
 ├── Lessons
 ├── Categories
 └── Enrollments

Conversation
 ├── Messages
 ├── AudioFiles
 ├── Assessments
 └── Contexts

Assessment
 ├── AssessmentDetails
 ├── AssessmentFeedbacks
 └── PronunciationErrors
```

---

# 19. Estimated Database Size

100,000 Users

```text
Users                    ~100K
Messages                 ~50M
Audio Files              ~20M
Assessments              ~30M
Analytics Events         ~100M
```

Expected PostgreSQL Size:

```text
500GB - 2TB
```

---

# 20. Scalability Recommendations

- PostgreSQL Read Replicas
- Redis Caching Layer
- Table Partitioning
- Connection Pooling (PgBouncer)
- Async Processing (BullMQ)
- Vector Search Optimization
- Daily Backup
- PITR Recovery

---

# Conclusion

Database được thiết kế để:

- Hỗ trợ 100,000+ người dùng đồng thời
- Multi-tenant ready
- AI-native architecture
- RAG integration
- Subscription business model
- Enterprise scalability

Sẵn sàng cho:
- NestJS
- Prisma
- PostgreSQL
- Redis
- BullMQ
- OpenAI
- Claude
- Gemini
- Kubernetes