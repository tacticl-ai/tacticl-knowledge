---
tags: [decision]
roles: [ARCHITECT, IMPLEMENTER, REVIEWER]
auto-approved: false
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Firestore Hybrid Schema (Approach B)

## What
Tacticl uses a hybrid Firestore schema: user-owned data lives in subcollections under `tacticl_users/{userId}/`, operational data lives in flat top-level collections.

## Why
Subcollections for user data enforce ownership at the data layer — a user cannot accidentally access another user's devices. Flat collections for operational data (sparks, posts) allow efficient cross-user querying by the system.

## How
**Nested under user** (subcollections — require userId for path):
- `tacticl_users/{userId}/devices/`
- `tacticl_users/{userId}/social_integrations/`
- `tacticl_users/{userId}/repo_grants/`
- `tacticl_users/{userId}/agent_tokens/`
- `tacticl_users/{userId}/agent_memory/`

**Flat collections** (top-level — use standard findById):
- `sparks/`, `tactics/`, `execution_logs/`, `checkpoints/`
- `social_posts/`, `device_commands/`, `action_confirmations/`
- `agent_reminders/`, `agent_audit_log/`

Soft delete only: call `entity.delete()` which sets `isActive = false`. Never hard-delete Firestore documents.

PDLC pipeline state (`pipeline_runs`, `pipeline_events`) is in **MongoDB** not Firestore — do not create Firestore collections for pipeline data.

## Example
```java
// Subcollection — userId required
deviceRepository.findById(userId, deviceId);

// Flat collection — no userId
sparkRepository.findById(sparkId);
```

## Related
- [[approved/gotchas/subcollection-userid-param]]
- [[entities/spark-entity]]
- [[entities/pipeline-run-entity]]
