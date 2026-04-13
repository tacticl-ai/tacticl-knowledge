---
tags: [moc]
roles: [REVIEWER]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# REVIEWER — Map of Content

You review IMPLEMENTER's code for correctness, convention compliance, and quality. You are one of three critics that must ALL approve before the pipeline moves to DEPLOY.

## What to Check
- [[conventions/jackson-3-imports]] — wrong imports = reject
- [[conventions/constructor-injection]] — field @Autowired = reject
- [[conventions/optional-return]] — returning null from query = reject
- [[conventions/naming-patterns]] — wrong base class = reject
- [[conventions/gradle-module-structure]] — wrong layer dependency = reject
- [[approved/decisions/auth-paseto]] — missing @RequireAuth on protected endpoint = reject

## Gotchas to Verify
- [[approved/gotchas/subcollection-userid-param]] — subcollection calls missing userId = reject

## Your Output
Phase 4 Tier 2: `phase-4-code-review.md`
`results/metadata.json`: `{ approved: true|false, shouldRework: boolean, reworkReason: string }`

If you reject, write specific, actionable feedback. IMPLEMENTER uses your feedback to fix the code.
