# 11-SPEAKING-ENGINE.md
# Speaking Engine Architecture & Design

## AI English Speaking Platform

**Version:** 1.0  
**Module:** Speaking Engine  
**Framework:** NestJS  
**Storage:** PostgreSQL + S3/R2  
**Queue:** BullMQ  
**Cache:** Redis  
**AI Providers:** OpenAI, Claude, Gemini

---

# 1. Overview

## Purpose

Speaking Engine là trái tim của hệ thống AI English Speaking Platform.

Chức năng:

- Speech-to-Text
- Pronunciation Assessment
- Fluency Analysis
- Vocabulary Analysis
- Grammar Analysis
- Accent Detection
- Real-Time Feedback
- Speaking Score Generation
- IELTS Speaking Simulation

---

# 2. High Level Architecture

```text
User
 ↓
Microphone
 ↓
Frontend Recorder
 ↓
Audio Upload API
 ↓
Storage (S3/R2)
 ↓
BullMQ Queue
 ↓
Speech Processing Worker
 ↓
Transcript
 ↓
Assessment Engine
 ↓
AI Feedback
 ↓
Database
 ↓
Frontend
```

---

# 3. Speaking Pipeline

```text
Audio Input
 ↓
Audio Validation
 ↓
Audio Normalization
 ↓
Speech To Text
 ↓
Transcript Cleaning
 ↓
Pronunciation Analysis
 ↓
Fluency Analysis
 ↓
Grammar Analysis
 ↓
Vocabulary Analysis
 ↓
Coherence Analysis
 ↓
Final Scoring
 ↓
AI Feedback
```

---

# 4. Audio Requirements

## Supported Formats

```text
wav

mp3

m4a

aac

ogg

webm
```

---

## Recommended

```text
wav

16kHz

mono
```

---

## Maximum Size

```text
50MB
```

---

## Maximum Duration

```text
15 Minutes
```

---

# 5. Audio Upload Flow

```text
Frontend
 ↓
Generate Upload URL
 ↓
Upload To S3
 ↓
Notify Backend
 ↓
Create Processing Job
```

---

# 6. Speech-to-Text Layer

## Provider Priority

```text
1. OpenAI Whisper

2. Deepgram

3. Google Speech

4. Azure Speech
```

---

## Default

```text
OpenAI Whisper
```

---

# 7. Speech-to-Text Flow

```text
Audio File
 ↓
Whisper
 ↓
Transcript
 ↓
Timestamp Metadata
```

---

## Example Output

```json
{
  "text": "Hello everyone, my name is John.",
  "language": "en",
  "duration": 8.2
}
```

---

# 8. Transcript Processing

## Cleanup

Remove:

```text
Noise

Repeated Words

Invalid Characters
```

---

Normalize:

```text
Capitalization

Punctuation

Spacing
```

---

# 9. Speaking Session

## Entity

```text
Speaking Session
```

Represents:

```text
One Speaking Attempt
```

---

## Lifecycle

```text
Created
 ↓
Recording
 ↓
Uploaded
 ↓
Processing
 ↓
Completed
```

---

# 10. Pronunciation Engine

## Purpose

Đánh giá khả năng phát âm.

---

Metrics:

```text
Word Accuracy

Phoneme Accuracy

Stress

Intonation

Clarity
```

---

# 11. Pronunciation Score

Scale:

```text
0 - 100
```

---

Formula:

```text
Word Accuracy      40%

Stress             20%

Intonation         20%

Clarity            20%
```

---

# 12. Word Accuracy Analysis

Example

Expected:

```text
Think
```

Spoken:

```text
Tink
```

---

Result:

```text
TH pronunciation issue
```

---

# 13. Phoneme Analysis

Evaluate:

```text
TH

R

L

V

W

S
```

---

Detect:

```text
Missing Sounds

Wrong Sounds

Weak Sounds
```

---

# 14. Stress Analysis

Check:

```text
Word Stress

Sentence Stress
```

---

Example

```text
PHOtograph

phoTOGraphy
```

---

# 15. Intonation Analysis

Evaluate:

```text
Rising Tone

Falling Tone

Natural Rhythm
```

---

# 16. Fluency Engine

## Purpose

Đánh giá độ trôi chảy.

---

Metrics:

```text
Speech Rate

Pause Frequency

Pause Duration

Filler Words

Sentence Completion
```

---

# 17. Speech Rate

Formula:

```text
Words Per Minute
```

---

Recommended

```text
120-180 WPM
```

---

# 18. Filler Word Detection

Detect:

```text
Um

Uh

Like

You know

Actually
```

---

Output:

```json
{
  "fillerWords": 4
}
```

---

# 19. Fluency Score

Formula

```text
Speech Rate       30%

Pauses            30%

Filler Words      20%

Sentence Flow     20%
```

---

# 20. Grammar Analysis

Evaluate:

```text
Tenses

Articles

Prepositions

Sentence Structure

Subject-Verb Agreement
```

---

Example

Input:

```text
I go to school yesterday.
```

---

Issue:

```text
Past Tense Error
```

---

Correction:

```text
I went to school yesterday.
```

---

# 21. Vocabulary Analysis

Evaluate:

```text
Lexical Diversity

Word Complexity

Topic Relevance

Vocabulary Range
```

---

# 22. Vocabulary Score

Metrics

```text
Unique Words

Advanced Words

Topic Match
```

---

# 23. Coherence Analysis

Evaluate:

```text
Logical Flow

Topic Consistency

Transitions

Sentence Linking
```

---

# 24. CEFR Mapping

| Score | CEFR |
|---------|---------|
| 0-30 | A1 |
| 31-45 | A2 |
| 46-60 | B1 |
| 61-75 | B2 |
| 76-90 | C1 |
| 91-100 | C2 |

---

# 25. IELTS Mapping

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

# 26. Overall Speaking Score

Weights

```text
Pronunciation  25%

Fluency        20%

Grammar        20%

Vocabulary     20%

Coherence      15%
```

---

Formula

```text
Weighted Average
```

---

# 27. AI Feedback Engine

Input

```text
Transcript

Scores

Detected Errors
```

---

Output

```json
{
  "strengths": [],
  "weaknesses": [],
  "recommendations": []
}
```

---

# 28. Personalized Feedback

Example

```text
You have good vocabulary usage.

Focus on TH pronunciation.

Practice longer sentences to improve fluency.
```

---

# 29. Real-Time Speaking Coach

## Phase 2

Architecture

```text
Microphone
 ↓
WebSocket
 ↓
Streaming STT
 ↓
Live Analysis
 ↓
Instant Feedback
```

---

# 30. WebSocket Events

```text
recording.started

recording.chunk

recording.finished

transcript.updated

score.updated
```

---

# 31. Audio Processing Queue

BullMQ Queue

```text
audio-processing

transcription

assessment

feedback-generation
```

---

# 32. Queue Workflow

```text
Upload
 ↓
Queue
 ↓
Worker
 ↓
Store Results
```

---

# 33. Database Schema

## speaking_sessions

```sql
id UUID

user_id UUID

lesson_id UUID

audio_url TEXT

status VARCHAR

created_at TIMESTAMP
```

---

# 34. Transcripts Schema

## transcripts

```sql
id UUID

session_id UUID

transcript TEXT

language VARCHAR
```

---

# 35. Speaking Scores Schema

## speaking_scores

```sql
id UUID

session_id UUID

pronunciation_score NUMERIC

fluency_score NUMERIC

grammar_score NUMERIC

vocabulary_score NUMERIC

coherence_score NUMERIC

overall_score NUMERIC
```

---

# 36. Speaking Feedback Schema

## speaking_feedback

```sql
id UUID

session_id UUID

feedback JSONB
```

---

# 37. APIs

## Upload Audio

```http
POST /api/v1/speaking/upload
```

---

## Start Assessment

```http
POST /api/v1/speaking/analyze
```

---

## Get Result

```http
GET /api/v1/speaking/{sessionId}
```

---

## Speaking History

```http
GET /api/v1/speaking/history
```

---

# 38. Redis Strategy

Keys

```text
speaking:{sessionId}

transcript:{sessionId}

score:{sessionId}
```

---

# 39. Analytics

Track

```text
Speaking Attempts

Average Scores

Common Mistakes

Practice Time

Completion Rate
```

---

# 40. Error Handling

Cases

```text
Invalid Audio

No Speech Detected

Provider Timeout

Corrupted Audio
```

---

Recovery

```text
Retry

Fallback Provider

Manual Review
```

---

# 41. Security

Validate:

```text
File Type

File Size

Duration
```

---

Scan:

```text
Malicious Uploads
```

---

Encrypt:

```text
Audio Storage
```

---

# 42. Scalability

Targets

```text
1,000,000 Audio Files

100,000 Concurrent Users

10,000 Assessments/Minute
```

---

Strategy

```text
Kubernetes

BullMQ Workers

Redis

Horizontal Scaling
```

---

# 43. Future Roadmap

Phase 2

```text
Real-Time Pronunciation Correction
```

---

Phase 3

```text
Accent Reduction Coach
```

---

Phase 4

```text
Video Speaking Analysis
```

---

Phase 5

```text
Emotion & Confidence Detection
```

---

# Conclusion

Speaking Engine là hệ thống cốt lõi của nền tảng.

Bao gồm:

- Speech-to-Text
- Pronunciation Scoring
- Fluency Analysis
- Grammar Analysis
- Vocabulary Analysis
- AI Feedback
- IELTS Assessment

Được thiết kế để:

- Chính xác
- Mở rộng cao
- Chi phí tối ưu
- Hỗ trợ Real-Time Coaching

Tech Stack:

- NestJS
- PostgreSQL
- Prisma
- Redis
- BullMQ
- OpenAI Whisper
- GPT
- Claude
- Gemini
- Kubernetes