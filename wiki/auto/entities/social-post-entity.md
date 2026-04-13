---
tags: [entity]
roles: [IMPLEMENTER, REVIEWER, TECHNICAL_WRITER]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# SocialPost Entity

## What
SocialPost tracks a piece of social media content through its publication lifecycle. Lives in flat `social_posts/` Firestore collection.

## Why
Posts go through async publishing — the scheduler picks up QUEUED posts and publishes them. The state machine prevents double-publishing and tracks failures.

## How
State machine: `DRAFT → QUEUED → PUBLISHING → PUBLISHED | FAILED | CANCELLED`

Supported platforms: `TWITTER`, `LINKEDIN`, `INSTAGRAM`, `GOOGLE_PHOTOS` (read-only source, not publish target).

Any publish action MUST require Tier 1 user confirmation before transitioning from DRAFT to QUEUED. Never auto-queue without user approval.

`PostPublisherJob` is `@Scheduled` — polls for QUEUED posts due for publishing. It uses `@Retryable(maxAttempts=3, backoff=@Backoff(delay=2000, multiplier=2))`.

## Example
```
DRAFT → (user confirms via Tier 1 confirmation) → QUEUED
QUEUED → (PostPublisherJob picks up) → PUBLISHING
PUBLISHING → (API call succeeds) → PUBLISHED
PUBLISHING → (API call fails after 3 retries) → FAILED
```

## Related
- [[entities/spark-entity]]
- [[approved/decisions/firestore-hybrid-schema]]
