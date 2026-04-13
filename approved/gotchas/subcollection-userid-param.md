---
tags: [gotcha]
roles: [IMPLEMENTER, REVIEWER]
auto-approved: false
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Subcollection Repos Require userId Parameter

## What
Repository classes for Firestore subcollections use `findById(userId, id)` — NOT `findById(id)`. The userId is required to construct the subcollection path.

## Why
Subcollections live at `tacticl_users/{userId}/{collection}/{docId}`. Without the userId, the Firestore path cannot be constructed. Calling `findById(id)` on a subcollection repo will either fail to compile or return wrong results.

## How
Always pass the authenticated user's ID as the first argument to subcollection repository calls. The authenticated userId comes from the `AuthenticatedUser` object in the controller, passed down through the service to the repo.

Affected subcollections: `devices/`, `social_integrations/`, `repo_grants/`, `agent_tokens/`, `agent_memory/`.

Flat collections (sparks, tactics, social_posts) use the standard `findById(id)`.

## Example
```java
// CORRECT — subcollection repo
Optional<DeviceRegistration> device = deviceRepository.findById(userId, deviceId);

// WRONG — missing userId
Optional<DeviceRegistration> device = deviceRepository.findById(deviceId); // compile error or wrong

// CORRECT — flat collection repo
Optional<Spark> spark = sparkRepository.findById(sparkId);
```

## Related
- [[conventions/optional-return]]
- [[conventions/base-classes]]
- [[entities/device-entity]]
