---
tags: [moc]
roles: [PLANNER]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# PLANNER — Map of Content

You break the ARCHITECT's design into implementation stories and tasks. Stories must be independently implementable by IMPLEMENTER.

## Codebase Structure (for story scoping)
- [[conventions/gradle-module-structure]] — module boundaries → natural story boundaries
- [[conventions/naming-patterns]] — what each story produces (controller, service, entity, etc.)

## Entities to Reference
- [[entities/spark-entity]]
- [[entities/pipeline-run-entity]]

## Your Output
Phase 2 Tier 2: `phase-2-stories.json` — structured story breakdown

Format: `{ stories: [{ id, title, description, tasks: [{ id, description, files: [] }], acceptanceCriteria: [] }] }`

Your output feeds into IMPLEMENTER (one story at a time, sequentially or in parallel per pipeline config).
