---
tags: [convention]
roles: [IMPLEMENTER, REVIEWER, TESTER, DEVOPS]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Jackson 3 Imports

## What
Tacticl uses Jackson 3 (`tools.jackson.*`). Jackson 2 (`com.fasterxml.jackson.*`) is NOT on the classpath.

## Why
Spring Boot 4.0.3 ships with Jackson 3. Jackson 2 imports will cause compile errors or ClassNotFoundExceptions at runtime.

## How
Always import from `tools.jackson.*`. Never import from `com.fasterxml.jackson.databind.*`.

Use `JsonMapper` instead of `ObjectMapper`. Use `JacksonException` instead of `JsonProcessingException`.

Annotation package is unchanged: `com.fasterxml.jackson.annotation.*` is still correct.

## Example
```java
// CORRECT — Jackson 3
import tools.jackson.databind.json.JsonMapper;
import tools.jackson.core.JacksonException;
import com.fasterxml.jackson.annotation.JsonProperty; // annotation package unchanged

// WRONG — Jackson 2, will not compile
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.core.JsonProcessingException;
```

## Related
- [[decisions/jackson-3-migration]]
- [[conventions/naming-patterns]]
