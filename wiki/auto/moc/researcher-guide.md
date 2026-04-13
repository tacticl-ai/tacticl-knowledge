---
tags: [moc]
roles: [RESEARCHER]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# RESEARCHER — Map of Content

You investigate existing code, external APIs, and technical constraints relevant to the PM's requirements. Use the live repo (Layer 1) as your primary source — grep, glob, and read the actual code.

## Codebase Navigation
- [[conventions/gradle-module-structure]] — where to look for different types of code
- [[entities/spark-entity]] — core entity you'll encounter frequently
- [[entities/pipeline-run-entity]] — PDLC pipeline entity (MongoDB-backed)

## Common Integration Points
- [[approved/gotchas/vault-https-localhost]] — if investigating secret loading
- [[approved/gotchas/anthropic-403-api-key]] — if investigating LLM integration

## Your Output
Phase 1 Tier 2: `phase-1-research-summary.md` — technical findings, risks, integration notes

Your output feeds into ARCHITECT (who designs the solution).
