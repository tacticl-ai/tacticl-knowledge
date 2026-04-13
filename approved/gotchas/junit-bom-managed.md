---
tags: [gotcha]
roles: [IMPLEMENTER, TESTER, DEVOPS]
auto-approved: false
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Do Not Pin JUnit Versions

## What
JUnit versions are managed by the Spring Boot BOM (Bill of Materials). Do NOT add explicit JUnit version numbers to `gradle/libs.versions.toml` or any `build.gradle.kts`.

## Why
Spring Boot 4.0.3 provides JUnit 6.0.3 via its BOM. Pinning an explicit version causes version conflicts that result in cryptic test failures or build errors.

## How
Do not add `junit = "6.0.3"` or any JUnit version to `libs.versions.toml`.
Do not add `testImplementation("org.junit.jupiter:junit-jupiter:6.0.3")` with an explicit version.
Spring Boot's test starter (`spring-boot-starter-test`) pulls the correct JUnit version automatically.

## Example
```toml
# libs.versions.toml — CORRECT (no JUnit version)
[versions]
spring-boot = "4.0.3"
jackson = "3.0.0"
# (no junit entry)

# WRONG — explicit JUnit pin causes conflicts
[versions]
junit = "6.0.3"  # remove this
```

## Related
- [[conventions/gradle-module-structure]]
