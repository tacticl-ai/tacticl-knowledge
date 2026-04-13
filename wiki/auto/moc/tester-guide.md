---
tags: [moc]
roles: [TESTER]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# TESTER — Map of Content

You write tests for IMPLEMENTER's code. In FULL_PDLC, you write FAILING tests BEFORE IMPLEMENTER writes the implementation (TDD enforcement). You are one of three critics that must ALL approve before DEPLOY.

## Test Conventions
- [[conventions/gradle-module-structure]] — run tests with `./gradlew :module:module-name:test`

## Gotchas
- [[approved/gotchas/vault-https-localhost]] — tests that load Vault config need HTTPS
- [[approved/gotchas/anthropic-403-api-key]] — integration tests that call LLMs need Vault running
- [[approved/gotchas/junit-bom-managed]] — do NOT pin JUnit versions, Spring Boot BOM manages them

## Coverage Target
Target: ≥80% line coverage on generated code. Write coverage report to results.

## Your Output
Phase 4 Tier 2: `phase-4-test-results.md`, `phase-4-coverage-breakdown.md`
`results/metadata.json`: `{ allPass: boolean, coverage: number, shouldRework: boolean }`
