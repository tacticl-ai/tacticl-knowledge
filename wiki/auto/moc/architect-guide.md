---
tags: [moc]
roles: [ARCHITECT]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# ARCHITECT — Map of Content

You design the system architecture for the feature: API surface, data model, service boundaries, integration points. Your design must fit within Tacticl's existing architecture.

## Architecture Constraints
- [[conventions/gradle-module-structure]] — layer boundaries you must respect
- [[conventions/naming-patterns]] — base class requirements
- [[approved/decisions/auth-paseto]] — auth token system
- [[approved/decisions/firestore-hybrid-schema]] — where different data lives
- [[approved/decisions/jackson-3-migration]] — serialization library

## Key Entities
- [[entities/spark-entity]] — core work unit
- [[entities/pipeline-run-entity]] — PDLC pipeline state (MongoDB)
- [[entities/social-post-entity]] — social media state machine
- [[entities/device-entity]] — device routing

## Your Output
Phase 2 Tier 2: `phase-2-architecture.md` — system design, API endpoints, data model
Phase 2 Tier 2: `phase-2-erd.md` — entity relationship diagram

Your output feeds into DESIGNER and PLANNER simultaneously.
