# 16-GAMIFICATION-SYSTEM.md
# Gamification System Architecture & Design

## AI English Speaking Platform

**Version:** 1.0  
**Module:** Gamification System  
**Framework:** NestJS  
**Database:** PostgreSQL + Prisma  
**Cache:** Redis  
**Queue:** BullMQ

---

# 1. Overview

## Purpose

Gamification System giúp:

- Tăng động lực học tập
- Tăng retention
- Tăng daily active users
- Tạo thói quen học tập
- Khuyến khích luyện nói thường xuyên

---

## Business Goals

```text
Increase Retention

Increase Session Time

Increase Daily Practice

Increase Course Completion
```

---

# 2. Core Components

```text
XP System

Level System

Achievements

Badges

Streak Engine

Challenges

Leaderboards

Reward Engine

Coins System
```

---

# 3. Gamification Architecture

```text
User Action
 ↓
Event Bus
 ↓
Gamification Engine
 ↓
Reward Calculation
 ↓
Database Update
 ↓
Notification
```

---

# 4. XP System

## Purpose

XP (Experience Points) phản ánh mức độ hoạt động của người dùng.

---

## Earn XP From

```text
Lesson Completion

Speaking Practice

Assessment Completion

Daily Login

Course Completion

Challenge Completion
```

---

# 5. XP Rules

| Action | XP |
|----------|----------|
| Daily Login | 5 |
| Lesson Complete | 20 |
| Speaking Session | 30 |
| Assessment Complete | 50 |
| Course Complete | 200 |
| Weekly Challenge | 300 |

---

# 6. XP Calculation

Formula:

```text
Total XP
=
Sum(All XP Events)
```

---

Example:

```text
Lesson = 20

Speaking = 30

Quiz = 10

Total = 60 XP
```

---

# 7. Level System

## Purpose

Hiển thị tiến trình dài hạn.

---

## Levels

```text
Level 1

Level 2

...

Level 100
```

---

# 8. XP Per Level

Example:

```text
Level 1 = 0 XP

Level 2 = 100 XP

Level 3 = 250 XP

Level 4 = 450 XP

Level 5 = 700 XP
```

---

# 9. Level Formula

```text
Level XP
=
100 × Level^1.5
```

---

# 10. User Rank Titles

```text
Beginner

Explorer

Learner

Speaker

Communicator

Advanced Speaker

English Master
```

---

# 11. Streak Engine

## Purpose

Khuyến khích học liên tục.

---

## Definition

Một ngày được tính streak nếu:

```text
Complete Lesson

OR

Speaking Practice

OR

Assessment
```

---

# 12. Streak Rules

```text
1 Day Activity
 ↓
+1 Streak
```

---

No Activity:

```text
24 Hours+
 ↓
Streak Reset
```

---

# 13. Streak Freeze

Premium Feature:

```text
1 Freeze / Month
```

---

Purpose:

```text
Protect Streak
```

---

# 14. Achievement System

## Purpose

Tạo cảm giác thành tựu.

---

# 15. Achievement Categories

```text
Learning

Speaking

Vocabulary

Assessment

Social

Premium
```

---

# 16. Example Achievements

```text
First Lesson

First Speaking Session

7 Day Streak

30 Day Streak

100 Lessons

1000 XP
```

---

# 17. Achievement Rarity

```text
Common

Rare

Epic

Legendary
```

---

# 18. Badges

Example:

```text
Grammar Master

Vocabulary Hero

Speaking Champion

IELTS Warrior

Consistency King
```

---

# 19. Daily Challenges

Examples:

```text
Complete 1 Lesson

Practice Speaking 10 Minutes

Learn 5 New Words
```

---

Reward:

```text
XP

Coins

Badge Progress
```

---

# 20. Weekly Challenges

Examples:

```text
Complete 5 Lessons

Practice 60 Minutes

Take 2 Assessments
```

---

# 21. Monthly Challenges

Examples:

```text
30 Day Streak

500 XP

Complete Course
```

---

# 22. Challenge Engine

```text
Challenge
 ↓
Progress Tracking
 ↓
Completion Detection
 ↓
Reward Distribution
```

---

# 23. Coins System

## Purpose

Virtual currency.

---

Earn From:

```text
Lessons

Challenges

Achievements

Events
```

---

# 24. Coins Usage

Future Features:

```text
Avatar Items

Themes

AI Personas

Premium Trials
```

---

# 25. Reward Engine

Rewards:

```text
XP

Coins

Badges

Achievements

Certificates
```

---

# 26. Reward Multipliers

Examples:

```text
Weekend Event

2x XP
```

---

```text
Special Campaign

3x Coins
```

---

# 27. Leaderboards

Types:

```text
Global

Country

Friends

Course

Weekly
```

---

# 28. Leaderboard Ranking

Based On:

```text
XP

Lessons Completed

Speaking Minutes

Achievements
```

---

# 29. Weekly Leaderboard

Reset:

```text
Every Monday
```

---

Rewards:

```text
Top 10 Badge

Coins

Special Rewards
```

---

# 30. Social Competition

Features:

```text
Friend Challenges

Group Challenges

Team Events
```

---

# 31. User Gamification Profile

Contains:

```text
Level

XP

Coins

Achievements

Badges

Streak
```

---

# 32. Database Schema

## user_gamification

```sql
id UUID

user_id UUID

level INTEGER

xp INTEGER

coins INTEGER

current_streak INTEGER

longest_streak INTEGER
```

---

# 33. XP Transactions

## xp_transactions

```sql
id UUID

user_id UUID

xp INTEGER

source VARCHAR

created_at TIMESTAMP
```

---

# 34. Achievements Schema

## achievements

```sql
id UUID

name VARCHAR

description TEXT

rarity VARCHAR
```

---

# 35. User Achievements

## user_achievements

```sql
id UUID

user_id UUID

achievement_id UUID

unlocked_at TIMESTAMP
```

---

# 36. Challenges Schema

## challenges

```sql
id UUID

name VARCHAR

challenge_type VARCHAR

reward JSONB
```

---

# 37. User Challenge Progress

## challenge_progress

```sql
id UUID

user_id UUID

challenge_id UUID

progress INTEGER

completed BOOLEAN
```

---

# 38. Leaderboards Schema

## leaderboard_entries

```sql
id UUID

user_id UUID

period VARCHAR

score INTEGER
```

---

# 39. Gamification Events

```text
LESSON_COMPLETED

COURSE_COMPLETED

SPEAKING_COMPLETED

ASSESSMENT_COMPLETED

LOGIN_COMPLETED
```

---

# 40. Event Processing

Using:

```text
BullMQ

Redis Streams
```

---

Flow:

```text
Event
 ↓
Queue
 ↓
Reward Calculation
 ↓
Update XP
```

---

# 41. Notification Integration

Notify:

```text
Level Up

Achievement Unlocked

Challenge Completed

Leaderboard Promotion
```

---

# 42. Analytics Metrics

Track:

```text
DAU

Retention

Average XP

Challenge Completion

Leaderboard Participation
```

---

# 43. Anti-Cheat System

Detect:

```text
Fake Activity

Automation

XP Farming

Replay Actions
```

---

Rules:

```text
Rate Limits

Duplicate Detection

Behavior Analysis
```

---

# 44. Redis Strategy

Keys:

```text
leaderboard:weekly

leaderboard:monthly

user-xp:{id}

user-streak:{id}
```

---

TTL:

```text
1 Day

7 Days

30 Days
```

---

# 45. Performance Targets

Leaderboard Query:

```text
< 100ms
```

---

Reward Distribution:

```text
< 500ms
```

---

# 46. Scalability Targets

Support:

```text
10 Million Users

100 Million XP Events

1 Million Challenges
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

# 47. Future Enhancements

Phase 2

```text
Guild System
```

---

Phase 3

```text
Team Competitions
```

---

Phase 4

```text
Season Pass
```

---

Phase 5

```text
NFT Certificates (Optional)
```

---

# 48. Integration Points

Connected To:

```text
User Module

Course Module

Lesson Module

Assessment Engine

AI Tutor

Notification Module
```

---

# 49. Success Metrics

Target Improvements:

```text
+25% Retention

+30% Daily Practice

+20% Lesson Completion

+15% Subscription Conversion
```

---

# Conclusion

Gamification System cung cấp:

- XP & Levels
- Streak Engine
- Achievements
- Badges
- Challenges
- Leaderboards
- Reward System

Được thiết kế để:

- Tăng động lực học tập
- Tăng retention dài hạn
- Khuyến khích luyện nói mỗi ngày
- Scale tới hàng chục triệu người dùng

Tech Stack:

- NestJS
- PostgreSQL
- Prisma
- Redis
- BullMQ
- Kubernetes