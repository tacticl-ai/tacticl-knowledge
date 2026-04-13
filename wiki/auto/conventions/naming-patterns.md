---
tags: [convention]
roles: [IMPLEMENTER, REVIEWER, ARCHITECT]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Naming Patterns

## What
Tacticl follows the Cidadel naming convention: all classes extend base classes named with a `Base` prefix. Controllers, services, entities, and clients all have required base classes.

## Why
The base classes provide shared behavior (auth enforcement, soft delete, error handling). Skipping them bypasses security and consistency guarantees.

## How
Every class in the right layer MUST extend the correct base:

| Layer | Class type | Must extend |
|-------|-----------|------------|
| service-* | REST controller | `BaseController` |
| business-* | Service class | `BaseService` |
| data-* | Firestore entity | `BaseEntity` |
| client-* | HTTP client | `BaseHttpClient` |

## Example
```java
// CORRECT
@RestController
public class SparkController extends BaseController {

@Service
public class SparkService extends BaseService {

@Document
public class Spark extends BaseEntity {
```

## Related
- [[conventions/base-classes]]
- [[conventions/constructor-injection]]
- [[conventions/gradle-module-structure]]
