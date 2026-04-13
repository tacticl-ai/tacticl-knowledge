---
tags: [moc]
roles: [IMPLEMENTER]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# IMPLEMENTER — Map of Content

You write production code. Read every page linked here before writing a single line. These are non-negotiable conventions — REVIEWER will reject code that violates them.

## Codebase Conventions (read all of these)
- [[conventions/jackson-3-imports]] — always tools.jackson.*, never com.fasterxml.jackson.databind
- [[conventions/gradle-module-structure]] — which module your code goes in, layer dependency rules
- [[conventions/naming-patterns]] — BaseController, BaseService, BaseEntity, BaseHttpClient
- [[conventions/constructor-injection]] — no @Autowired on fields, ever
- [[conventions/optional-return]] — return Optional<T> for queries, never null
- [[conventions/base-classes]] — extend the right base class or it will not compile

## Architecture Decisions
- [[approved/decisions/auth-paseto]] — how auth works, use @RequireAuth
- [[approved/decisions/firestore-hybrid-schema]] — where to put new Firestore collections
- [[approved/decisions/jackson-3-migration]] — serialization classes to use

## Key Entities
- [[entities/spark-entity]] — core work unit
- [[entities/device-entity]] — device routing

## Gotchas (read before touching these areas)
- [[approved/gotchas/subcollection-userid-param]] — subcollection repos need userId
- [[approved/gotchas/junit-bom-managed]] — do not pin JUnit versions
- [[approved/gotchas/vault-https-localhost]] — Vault HTTPS on localhost
- [[approved/gotchas/anthropic-403-api-key]] — 403 = missing Vault key

## Your Output
Phase 3 Tier 2: code changes on `feature/{sparkId}/{storySlug}` branch
Phase 3: `results/metadata.json` with `{ shouldRework, reworkReason, confidence }`

You generate 3 candidate implementations. CRITIC selects the best. Your selected output feeds into TESTER, REVIEWER, SECURITY_ANALYST simultaneously.
