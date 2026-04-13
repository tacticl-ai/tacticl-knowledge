---
tags: [entity]
roles: [ARCHITECT, IMPLEMENTER, DEVOPS, RETRO_ANALYST]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# PipelineRun Entity

## What
A PipelineRun tracks the full lifecycle of one PDLC pipeline execution. One PipelineRun per spark that is classified as PLAYBOOK or FULL_PDLC tier. Lives in MongoDB (NOT Firestore — this is v2 architecture).

## Why
Pipeline state is complex (12 roles, parallel phases, rework loops, checkpoints). Firestore cannot efficiently query or aggregate this data. MongoDB's document model handles nested phase/role state naturally.

## How
MongoDB database: `tacticl_pdlc`, collection: `pipeline_runs`.

Top-level fields: `_id` (runId UUID), `sparkId`, `userId`, `playbook`, `status`, `sparkRequest`, `repoUrl`, `skipRoles[]`, `costCeilingUsd`, `totalCostUsd`, `createdAt`, `updatedAt`.

Nested `phases` map: keyed by phase ID (`PRODUCT`, `DESIGN`, `DEVELOPMENT`, `QUALITY`, `DEPLOY`). Each phase has: `status`, `startedAt`, `completedAt`, `roles` map, `checkpointId`, `checkpointStatus`.

State: `PENDING → RUNNING → PAUSED_AT_CHECKPOINT → RUNNING → COMPLETED | FAILED | CANCELLED`

Do NOT write to this collection from tacticl-core — it is owned by the arbiter shell. tacticl-core reads via gRPC `GetPipelineStatus`.

## Example
```json
{
  "_id": "run-abc123",
  "sparkId": "spark-xyz789",
  "playbook": "FULL_PDLC",
  "status": "RUNNING",
  "phases": {
    "PRODUCT": { "status": "COMPLETED", "roles": { "PM": { "status": "COMPLETED", "costUsd": 2.10 } } }
  }
}
```

## Related
- [[entities/spark-entity]]
- [[approved/decisions/firestore-hybrid-schema]]
