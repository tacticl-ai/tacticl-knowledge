---
tags: [convention]
roles: [IMPLEMENTER, REVIEWER]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Optional Return for Queries

## What
All repository query methods that may return no result MUST return `Optional<T>`, never `null` or a bare entity type.

## Why
Returning null for missing records forces all callers to null-check. Forgetting a null check causes NullPointerExceptions in production. `Optional<T>` makes the absence explicit and forces callers to handle it.

## How
Repository methods: return `Optional<T>`.
Service methods that look up by ID: return `Optional<T>` and let the controller handle 404.
Never call `.get()` on an Optional without a `.isPresent()` check or `.orElseThrow()`.

## Example
```java
// CORRECT — repository
public Optional<Spark> findById(String userId, String sparkId) {
    // ...
}

// CORRECT — service
public Optional<Spark> getSpark(String userId, String sparkId) {
    return sparkRepository.findById(userId, sparkId);
}

// CORRECT — controller
Spark spark = sparkService.getSpark(userId, sparkId)
    .orElseThrow(() -> new NotFoundException("Spark not found: " + sparkId));

// WRONG — returning null
public Spark findById(String sparkId) {
    return null; // never do this
}
```

## Related
- [[conventions/constructor-injection]]
- [[conventions/base-classes]]
