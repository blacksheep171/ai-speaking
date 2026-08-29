# 19-SUBSCRIPTION-BILLING.md
# Subscription & Billing System Architecture

## AI English Speaking Platform

**Version:** 1.0  
**Module:** Subscription & Billing  
**Framework:** NestJS  
**Database:** PostgreSQL + Prisma  
**Payments:** Stripe  
**Cache:** Redis  
**Queue:** BullMQ

---

# 1. Overview

## Purpose

Subscription & Billing System chịu trách nhiệm:

- Subscription Management
- Payment Processing
- Plan Management
- Free Trial
- Coupons & Discounts
- Invoice Management
- Revenue Tracking
- Enterprise Billing

---

## Business Goals

```text
Increase Conversion

Increase MRR

Reduce Churn

Improve Renewal Rate
```

---

# 2. Monetization Model

Supported Models:

```text
Freemium

Monthly Subscription

Yearly Subscription

Enterprise Plan
```

---

# 3. High-Level Architecture

```text
User
 ↓
Checkout
 ↓
Stripe
 ↓
Webhook
 ↓
Billing Service
 ↓
Database
 ↓
Access Control
```

---

# 4. Subscription Plans

## Free Plan

Features:

```text
Basic Lessons

Limited AI Chat

Limited Speaking Sessions

Basic Analytics
```

---

## Premium Plan

Features:

```text
Unlimited AI Tutor

Unlimited Speaking Practice

Advanced Assessments

Learning Paths

Premium Courses
```

---

## Enterprise Plan

Features:

```text
Team Management

Admin Dashboard

Custom Reporting

Dedicated Support
```

---

# 5. Pricing Structure

Example:

| Plan | Monthly | Yearly |
|---------|---------|---------|
| Free | $0 | $0 |
| Premium | $19 | $190 |
| Enterprise | Custom | Custom |

---

# 6. Subscription Lifecycle

```text
Trial
 ↓
Active
 ↓
Past Due
 ↓
Canceled
 ↓
Expired
```

---

# 7. Trial System

Default Trial:

```text
7 Days
```

---

Alternative:

```text
14 Days

30 Days
```

---

# 8. Trial Rules

Restrictions:

```text
One Trial Per User

Requires Payment Method
```

---

# 9. Access Control

Purpose:

```text
Control Feature Access
```

---

Example:

```text
Premium User
 ↓
Unlimited Speaking
```

---

# 10. Feature Gating

```text
AI Tutor

Speaking Engine

Assessments

Premium Courses
```

---

# 11. Stripe Integration

Services:

```text
Checkout Session

Subscription API

Customer Portal

Invoices
```

---

# 12. Checkout Flow

```text
Choose Plan
 ↓
Stripe Checkout
 ↓
Payment Success
 ↓
Webhook
 ↓
Activate Subscription
```

---

# 13. Stripe Customer

Each User:

```text
1 Stripe Customer
```

---

Stored:

```text
stripe_customer_id
```

---

# 14. Subscription Entity

## subscriptions

```sql
id UUID

user_id UUID

plan_id UUID

status VARCHAR

stripe_subscription_id VARCHAR

started_at TIMESTAMP

expires_at TIMESTAMP
```

---

# 15. Plan Entity

## plans

```sql
id UUID

name VARCHAR

billing_cycle VARCHAR

price NUMERIC

features JSONB
```

---

# 16. Billing Cycle

Supported:

```text
Monthly

Quarterly

Yearly
```

---

# 17. Upgrade Flow

```text
Free
 ↓
Premium Monthly
```

---

Effect:

```text
Immediate Access
```

---

# 18. Downgrade Flow

```text
Premium
 ↓
Free
```

---

Effect:

```text
At End Of Billing Period
```

---

# 19. Subscription Renewal

Managed By:

```text
Stripe
```

---

Process:

```text
Auto Renewal
```

---

# 20. Cancellation Flow

Options:

```text
Immediate

End Of Billing Period
```

---

# 21. Reactivation

Allowed:

```text
Within Grace Period
```

---

Example:

```text
30 Days
```

---

# 22. Coupon System

Supported:

```text
Percentage Discount

Fixed Discount

Free Trial Extension
```

---

# 23. Coupon Rules

Example:

```text
20% OFF

Valid Until Dec 31
```

---

# 24. Coupon Entity

## coupons

```sql
id UUID

code VARCHAR

discount_type VARCHAR

discount_value NUMERIC

expires_at TIMESTAMP
```

---

# 25. Invoice Management

Provider:

```text
Stripe
```

---

Store:

```text
Invoice Metadata
```

---

# 26. Invoice Entity

## invoices

```sql
id UUID

subscription_id UUID

stripe_invoice_id VARCHAR

amount NUMERIC

status VARCHAR
```

---

# 27. Payment Methods

Supported:

```text
Credit Card

Debit Card

Apple Pay

Google Pay
```

---

# 28. Webhook Architecture

Events:

```text
checkout.session.completed

invoice.paid

invoice.payment_failed

customer.subscription.updated

customer.subscription.deleted
```

---

# 29. Webhook Flow

```text
Stripe Event
 ↓
Webhook Endpoint
 ↓
Verify Signature
 ↓
Process Event
 ↓
Update Database
```

---

# 30. Failed Payments

States:

```text
Retry

Past Due

Canceled
```

---

# 31. Dunning Management

Purpose:

```text
Recover Failed Payments
```

---

Actions:

```text
Email Reminder

Push Notification

Retry Payment
```

---

# 32. Grace Period

Example:

```text
7 Days
```

---

Access:

```text
Limited Premium Access
```

---

# 33. Revenue Recognition

Track:

```text
MRR

ARR

Recognized Revenue

Deferred Revenue
```

---

# 34. Revenue Metrics

```text
MRR

ARR

ARPU

LTV

CAC
```

---

# 35. Subscription Analytics

Track:

```text
New Subscribers

Renewals

Churn

Upgrades

Downgrades
```

---

# 36. Churn Analysis

Measure:

```text
Cancellation Rate

Revenue Churn

User Churn
```

---

# 37. Customer Portal

Features:

```text
Update Card

Download Invoices

Cancel Subscription

Upgrade Plan
```

---

# 38. APIs

## Get Plans

```http
GET /api/v1/plans
```

---

## Create Checkout

```http
POST /api/v1/billing/checkout
```

---

## Subscription Status

```http
GET /api/v1/subscriptions/me
```

---

## Cancel Subscription

```http
POST /api/v1/subscriptions/cancel
```

---

# 39. Redis Cache Strategy

Keys:

```text
subscription:{userId}

plan:{id}

billing-status:{userId}
```

---

TTL:

```text
15 Minutes

1 Hour
```

---

# 40. BullMQ Queues

```text
billing-webhooks

invoice-processing

subscription-updates

dunning-management
```

---

# 41. Security

Protect:

```text
Payment Information

Billing Data

Invoices
```

---

Rules:

```text
Never Store Card Data

Verify Webhooks

Encrypt Sensitive Data
```

---

# 42. Compliance

Requirements:

```text
PCI DSS

GDPR

CCPA
```

---

# 43. Performance Targets

Checkout Creation:

```text
< 500ms
```

---

Webhook Processing:

```text
< 1 Second
```

---

Subscription Lookup:

```text
< 100ms
```

---

# 44. Scalability Targets

Support:

```text
1 Million Subscribers

100,000 Payments/Day

10 Million Invoices
```

---

Architecture:

```text
Stripe

Redis

BullMQ

Kubernetes
```

---

# 45. Enterprise Billing

Support:

```text
Custom Pricing

Annual Contracts

Seat-Based Billing
```

---

# 46. Future Enhancements

Phase 2

```text
Usage-Based Billing
```

---

Phase 3

```text
Regional Pricing
```

---

Phase 4

```text
Affiliate Revenue Sharing
```

---

Phase 5

```text
Marketplace Billing
```

---

# 47. Integration Points

Connected To:

```text
User Module

AI Tutor

Speaking Engine

Course Module

Analytics Module

Notification Module
```

---

# 48. Success Metrics

Targets:

```text
+20% Conversion Rate

+15% Renewal Rate

-10% Churn

+25% MRR Growth
```

---

# Conclusion

Subscription & Billing System cung cấp:

- Freemium Model
- Stripe Billing
- Subscription Lifecycle
- Coupons & Discounts
- Invoice Management
- Revenue Analytics

Được thiết kế để:

- Tối ưu doanh thu
- Giảm churn
- Tăng chuyển đổi
- Scale tới hàng triệu subscribers

Tech Stack:

- NestJS
- PostgreSQL
- Prisma
- Redis
- BullMQ
- Stripe
- Kubernetes