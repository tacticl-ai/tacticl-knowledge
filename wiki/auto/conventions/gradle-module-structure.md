---
tags: [convention]
roles: [IMPLEMENTER, ARCHITECT, DEVOPS, REVIEWER]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Gradle Module Structure

## What
Tacticl uses a nested multi-module Gradle layout with strict layering rules. Modules are grouped into: `service/`, `business/`, `data/`, `client/`, `application/`.

## Why
Violating layer boundaries creates circular dependencies, breaks the build, and couples layers that must stay independent.

## How
Follow these dependency rules. The rule is: lower layers cannot depend on higher layers.

```
service-*    → business-*, client-*, data-*, framework-*  (NEVER other service-*)
business-*   → other business-*, client-*, data-*, framework-*  (NEVER service-*)
client-*     → framework-* and client-base only
data-*       → framework-* only
```

Build commands:
- Full build: `./gradlew build`
- Skip tests: `./gradlew build -x test`
- Single module test: `./gradlew :service:service-agent:test`
- Show module tree: `./gradlew projects`

## Example
```kotlin
// In business-agent/build.gradle.kts — CORRECT
dependencies {
    implementation(project(":business:business-social"))
    implementation(project(":data:data-social"))
}

// WRONG — business depending on service
dependencies {
    implementation(project(":service:service-agent")) // circular!
}
```

## Related
- [[conventions/naming-patterns]]
- [[conventions/base-classes]]
