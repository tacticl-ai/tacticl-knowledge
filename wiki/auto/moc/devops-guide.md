---
tags: [moc]
roles: [DEVOPS]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# DEVOPS — Map of Content

You handle deployment: creating PRs, writing migration scripts, updating environment config, and ensuring the feature can deploy cleanly.

## Deploy Targets
- **QA**: `gcloud builds submit --config deployment/cloudbuild/cloudbuild-qa.yaml .`
- **Prod**: `gcloud builds submit --config deployment/cloudbuild/cloudbuild-prod.yaml .`
- GCP project: `tacticl`, region: `us-east1`
- QA service: `tacticl-core-qa` (2Gi), Prod service: `tacticl-core` (4Gi)

## Secrets
- All secrets in Vault — never in code or config files
- Vault context: `tacticl` for Tacticl secrets, `strategiz` for LLM keys
- [[approved/gotchas/vault-https-localhost]]

## Firestore
- No migrations needed for Firestore schema changes (schemaless)
- If removing a collection: soft-delete first (mark docs `migrated: true`), hard-delete after 30-day window
- [[approved/decisions/firestore-hybrid-schema]]

## MongoDB (PDLC state only)
- Schema changes handled by arbiter shell — do not touch from tacticl-core
- [[entities/pipeline-run-entity]]

## Your Output
Phase 5 Tier 2: `phase-5-deployment-notes.md` — migration steps, env var changes, rollback plan

Create PRs via GitHub API using the TECHNICAL_WRITER's PR descriptions. Link PRs to the original spark (include sparkId in PR description).
