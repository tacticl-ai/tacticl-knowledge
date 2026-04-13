---
tags: [entity]
roles: [ARCHITECT, IMPLEMENTER, DEVOPS]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# DeviceRegistration Entity

## What
DeviceRegistration represents one of the user's connected devices (phone, laptop, desktop). Stored in the user's subcollection: `tacticl_users/{userId}/devices/{deviceId}`.

## Why
Device capabilities, battery state, and settings determine routing. The entity is the source of truth for which devices can accept sparks and what execution engine they use.

## How
Device types: `MACOS`, `WINDOWS`, `LINUX` (desktop, priority=0), `IOS`, `ANDROID` (mobile, priority=1).

Desktop devices (priority=0) default to Claude Code CLI engine. Mobile always uses LEGACY engine.

`DeviceSettings` is embedded in the document (not a separate collection). Contains: `executionEngine` (CLAUDE_CODE | LEGACY | AUTO), `maxDaemons`, `autoWake`, `sparkPreferences`.

`ClaudeCodeConfig` is embedded in `DeviceSettings`. Contains: model, maxTurns, maxBudgetUsd, allowedTools, permissionMode.

Subcollection access: always pass `userId` to the repository — `deviceRepository.findById(userId, deviceId)`.

## Example
```java
Optional<DeviceRegistration> device = deviceRepository.findById(userId, deviceId);

// Check if desktop
boolean isDesktop = device.map(d -> d.getDeviceType().getPriority() == 0).orElse(false);
```

## Related
- [[entities/spark-entity]]
- [[conventions/optional-return]]
- [[approved/gotchas/subcollection-userid-param]]
