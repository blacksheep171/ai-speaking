# 18-ANALYTICS-SYSTEM.md
# Analytics & Business Intelligence Architecture

## AI English Speaking Platform

**Version:** 1.0  
**Module:** Analytics System  
**Framework:** NestJS  
**OLTP Database:** PostgreSQL  
**Analytics Database:** ClickHouse  
**Cache:** Redis  
**Queue:** BullMQ  
**Visualization:** Grafana / Metabase

---

# 1. Overview

## Purpose

Analytics System chịu trách nhiệm:

- Product Analytics
- Learning Analytics
- Speaking Analytics
- AI Usage Analytics
- Revenue Analytics
- User Behavior Tracking
- KPI Dashboards
- Executive Reporting

---

## Business Goals

```text
Measure User Growth

Measure Learning Outcomes

Improve Retention

Reduce Churn

Optimize AI Cost

Increase Revenue
```

---

# 2. Analytics Architecture

```text
Frontend
      ↓
Event Tracking SDK
      ↓
Analytics API
      ↓
Event Queue
      ↓
Event Processor
      ↓
ClickHouse
      ↓
Dashboard
```

---

# 3. Analytics Layers

```text
Operational Analytics

Learning Analytics

Business Analytics

Executive Analytics
```

---

# 4. Event-Driven Analytics

Everything is an Event.

Examples:

```text
User Registered

Lesson Started

Lesson Completed

Speaking Completed

Assessment Completed

Subscription Purchased
```

---

# 5. Event Flow

```text
Client Event
 ↓
Analytics Gateway
 ↓
BullMQ Queue
 ↓
Processor
 ↓
ClickHouse
```

---

# 6. Core Analytics Domains

```text
User Analytics

Learning Analytics

Speaking Analytics

AI Analytics

Revenue Analytics
```

---

# 7. User Analytics

Track:

```text
Registrations

DAU

WAU

MAU

Retention

Churn
```

---

# 8. Learning Analytics

Track:

```text
Courses Started

Courses Completed

Lessons Started

Lessons Completed

Completion Rate
```

---

# 9. Speaking Analytics

Track:

```text
Speaking Sessions

Practice Minutes

Pronunciation Score

Fluency Score

Improvement Trends
```

---

# 10. AI Analytics

Track:

```text
Messages Sent

Tokens Consumed

AI Costs

Model Usage

Response Time
```

---

# 11. Revenue Analytics

Track:

```text
MRR

ARR

Subscriptions

Refunds

Revenue Growth
```

---

# 12. Product KPIs

Primary KPIs:

```text
DAU

MAU

Retention

Completion Rate

Revenue

AI Cost/User
```

---

# 13. North Star Metric

Recommended:

```text
Weekly Speaking Minutes
```

---

Reason:

```text
Reflects Learning Engagement
```

---

# 14. Event Taxonomy

Naming Convention:

```text
entity.action
```

---

Examples:

```text
user.registered

lesson.started

lesson.completed

speaking.completed

assessment.completed
```

---

# 15. User Lifecycle Analytics

```text
Visitor
 ↓
Registered
 ↓
Active
 ↓
Subscriber
 ↓
Advocate
```

---

# 16. Funnel Analysis

Example Funnel:

```text
Register
 ↓
Complete Onboarding
 ↓
Start Course
 ↓
Complete Lesson
 ↓
Subscribe
```

---

# 17. Funnel Metrics

Track:

```text
Conversion %

Drop-Off %

Completion %
```

---

# 18. Retention Analytics

Measure:

```text
Day 1

Day 7

Day 30

Day 90
```

---

# 19. Cohort Analysis

Group Users By:

```text
Signup Date

Country

Goal

Subscription Plan
```

---

# 20. Churn Analytics

Detect:

```text
Inactive Users

Subscription Cancellations

Declining Engagement
```

---

# 21. Learning Progress Analytics

Track:

```text
Average Lessons

Practice Time

Assessment Improvements

Learning Velocity
```

---

# 22. Speaking Improvement Analytics

Track:

```text
Pronunciation Trends

Fluency Trends

Vocabulary Growth

Confidence Growth
```

---

# 23. Assessment Analytics

Track:

```text
Average Scores

CEFR Distribution

IELTS Distribution

Pass Rates
```

---

# 24. Recommendation Analytics

Measure:

```text
Recommendation CTR

Acceptance Rate

Completion Rate
```

---

# 25. Gamification Analytics

Track:

```text
XP Earned

Level Distribution

Streak Distribution

Challenge Participation
```

---

# 26. AI Usage Analytics

Track:

```text
Requests

Tokens

Costs

Latency

Errors
```

---

# 27. AI Cost Dashboard

Metrics:

```text
Cost/User

Cost/Conversation

Cost/Day

Cost/Month
```

---

# 28. Revenue Analytics

Metrics:

```text
MRR

ARR

ARPU

LTV

CAC
```

---

# 29. Subscription Analytics

Track:

```text
Trials

Conversions

Renewals

Cancellations
```

---

# 30. Event Schema

## analytics_events

```sql
id UUID

event_name VARCHAR

user_id UUID

properties JSONB

created_at TIMESTAMP
```

---

# 31. Event Properties Example

```json
{
  "courseId": "uuid",
  "lessonId": "uuid",
  "duration": 120
}
```

---

# 32. ClickHouse Event Table

```sql
event_id UUID

event_name String

user_id UUID

properties String

created_at DateTime
```

---

# 33. Daily Aggregations

Generated Metrics:

```text
DAU

Lessons Completed

Practice Minutes

Revenue
```

---

# 34. Analytics APIs

## Dashboard Metrics

```http
GET /api/v1/analytics/dashboard
```

---

## Learning Metrics

```http
GET /api/v1/analytics/learning
```

---

## Revenue Metrics

```http
GET /api/v1/analytics/revenue
```

---

## AI Metrics

```http
GET /api/v1/analytics/ai
```

---

# 35. Executive Dashboard

Widgets:

```text
Users

Revenue

Retention

AI Costs

Learning Metrics
```

---

# 36. Learning Dashboard

Widgets:

```text
Completion Rate

Speaking Minutes

Assessment Scores

Learning Progress
```

---

# 37. Instructor Dashboard

Widgets:

```text
Student Progress

Course Popularity

Lesson Engagement
```

---

# 38. Real-Time Analytics

Powered By:

```text
Redis Streams

WebSockets
```

---

Track:

```text
Active Users

Current Sessions

Live Speaking Sessions
```

---

# 39. Redis Strategy

Keys:

```text
dau

active-users

realtime-metrics

top-courses
```

---

TTL:

```text
1 Minute

5 Minutes

15 Minutes
```

---

# 40. BullMQ Queues

```text
event-processing

daily-aggregation

report-generation

ai-cost-tracking
```

---

# 41. Reporting System

Reports:

```text
Daily

Weekly

Monthly

Quarterly
```

---

Formats:

```text
PDF

CSV

Excel
```

---

# 42. Data Warehouse Strategy

Storage:

```text
PostgreSQL
↓
ETL
↓
ClickHouse
```

---

Purpose:

```text
Fast Analytics Queries
```

---

# 43. Performance Targets

Dashboard Load:

```text
< 1 Second
```

---

Analytics Query:

```text
< 2 Seconds
```

---

Event Processing:

```text
< 500ms
```

---

# 44. Monitoring

Track:

```text
Queue Size

Event Throughput

Query Performance

Data Freshness
```

---

Tools:

```text
Prometheus

Grafana

OpenTelemetry
```

---

# 45. Security

Protect:

```text
PII

Payment Data

User Analytics
```

---

Compliance:

```text
GDPR

CCPA
```

---

# 46. Scalability Targets

Support:

```text
100 Million Events/Day

10 Million Users

1 Million DAU
```

---

Architecture:

```text
ClickHouse Cluster

Redis

BullMQ

Kubernetes
```

---

# 47. Future Enhancements

Phase 2

```text
Predictive Analytics
```

---

Phase 3

```text
AI-Powered Insights
```

---

Phase 4

```text
Learning Outcome Forecasting
```

---

Phase 5

```text
Anomaly Detection
```

---

# 48. Integration Points

Connected To:

```text
User Module

Course Module

Lesson Module

Speaking Engine

Assessment Engine

Subscription Module

AI Tutor
```

---

# 49. Success Metrics

Targets:

```text
+20% Retention

+30% Completion Rate

+15% Subscription Conversion

-25% AI Cost
```

---

# Conclusion

Analytics System cung cấp:

- Product Analytics
- Learning Analytics
- Speaking Analytics
- AI Cost Analytics
- Revenue Analytics
- Executive Dashboards

Được thiết kế để:

- Data-Driven Decisions
- Theo dõi hiệu quả học tập
- Tối ưu chi phí AI
- Scale tới hàng trăm triệu events

Tech Stack:

- NestJS
- PostgreSQL
- ClickHouse
- Redis
- BullMQ
- Grafana
- Metabase
- Kubernetes