---
tags: [entity]
roles: [PM, IMPLEMENTER, ARCHITECT, PLANNER, REVIEWER]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Spark Entity

## What
A Spark is the top-level entity for every user request. Every chat message, voice command, and GitHub webhook creates exactly one Spark. There is no manual spark creation.

## Why
All downstream entities (tactics, execution logs, checkpoints, device commands) reference a sparkId. The spark is the unit of work tracking.

## How
Sparks live in the flat `sparks/` Firestore collection (not under a user subcollection — they are operational data).

State machine: `PENDING → ROUTING → QUEUED | EXECUTING → CHECKPOINT → COMPLETED | FAILED | CANCELLED`

Types (set by SparkClassifierService): `code | social | research | devops | creative | data`

`code` and `devops` types are routed through PdlcClassifierService for pipeline tier selection.

Creation: always via `SparkService.createSpark()` — never directly write to Firestore.

## Example
```java
// Creating a spark
Spark spark = sparkService.createSpark(userId, commandText, sessionId);

// State transitions
sparkService.markRunning(spark.getId());
sparkService.markCloudCompleted(spark.getId(), tokenCount, modelId);
```

Firestore path: `sparks/{sparkId}`

## Related
- [[entities/pipeline-run-entity]]
- [[entities/device-entity]]
- [[approved/decisions/firestore-hybrid-schema]]
