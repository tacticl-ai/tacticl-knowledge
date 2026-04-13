---
tags: [decision]
roles: [IMPLEMENTER, REVIEWER]
auto-approved: false
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Jackson 3 Migration Decision

## What
Tacticl uses Jackson 3 (`tools.jackson.*`) — both cidadel-core 0.4.2 and Spring Boot 4.0.3 ship with Jackson 3.

## Why
Jackson 3 is a complete rewrite with a new package namespace (`tools.jackson.*` instead of `com.fasterxml.jackson.databind.*`). Spring Boot 4 requires it. There is no compatibility mode — you must use one or the other.

## How
Key package changes from Jackson 2 → Jackson 3:
- `ObjectMapper` → `JsonMapper` (import from `tools.jackson.databind.json.JsonMapper`)
- `JsonProcessingException` → `JacksonException` (import from `tools.jackson.core.JacksonException`)
- Annotation package UNCHANGED: `com.fasterxml.jackson.annotation.*` still works

Do NOT mix Jackson 2 and Jackson 3 classes in the same codebase — it will not compile.

## Example
```java
// Jackson 3 — CORRECT
import tools.jackson.databind.json.JsonMapper;
import tools.jackson.core.JacksonException;
import tools.jackson.databind.JsonNode;

JsonMapper mapper = new JsonMapper();
JsonNode node = mapper.readTree(json);

// Jackson 2 — WRONG, will not compile with Spring Boot 4
import com.fasterxml.jackson.databind.ObjectMapper; // wrong
```

## Related
- [[conventions/jackson-3-imports]]
