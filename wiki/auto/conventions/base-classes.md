---
tags: [convention]
roles: [IMPLEMENTER, REVIEWER, ARCHITECT]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Required Base Classes

## What
Every controller, service, entity, and HTTP client MUST extend the appropriate Cidadel base class. These come from `cidadel-core` via GitHub Packages.

## Why
Base classes provide: auth enforcement (`@RequireAuth`), soft delete (`isActive` flag), error handling (`StandardErrorResponse`), and structured logging. Skipping them means manually re-implementing these — or missing them entirely.

## How
Import the base class from the correct Cidadel framework module. Add it as a dependency in the module's `build.gradle.kts` via the parent `build.gradle.kts` shared deps.

Soft delete: call `entity.delete()` which sets `isActive = false`. Never hard-delete Firestore documents.

Subcollection repositories: use `findById(userId, id)` — NOT `findById(id)`. The base class requires the user ID for subcollection path resolution.

## Example
```java
// Controller — from framework-authorization
import io.strategiz.framework.authorization.BaseController;

// Service — from framework-logging (or similar cidadel module)
import io.cidadel.framework.base.BaseService;

// Entity — from data-framework-base
import io.cidadel.data.base.BaseEntity;

// HTTP client — from client-base
import io.cidadel.client.base.BaseHttpClient;
```

## Related
- [[conventions/naming-patterns]]
- [[conventions/optional-return]]
- [[approved/gotchas/subcollection-userid-param]]
