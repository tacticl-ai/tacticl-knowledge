---
tags: [convention]
roles: [IMPLEMENTER, REVIEWER]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Constructor Injection

## What
All Spring dependencies are injected via constructor. Never use `@Autowired` on fields or setter methods.

## Why
Field injection hides dependencies, makes testing hard (cannot mock without reflection), and is not recommended by Spring since 5.x. Constructor injection makes dependencies explicit and allows `final` fields.

## How
Declare all dependencies as `private final` fields. Create one constructor with all dependencies. Spring autowires by type automatically if there is exactly one constructor.

Do NOT use `@Autowired` on the constructor — it is redundant in modern Spring and adds noise.

Do NOT use `@Autowired` on fields — ever.

## Example
```java
// CORRECT
@Service
public class SparkService extends BaseService {

    private final SparkRepository sparkRepository;
    private final SparkClassifierService classifier;

    public SparkService(SparkRepository sparkRepository,
                        SparkClassifierService classifier) {
        this.sparkRepository = sparkRepository;
        this.classifier = classifier;
    }
}

// WRONG — field injection
@Service
public class SparkService extends BaseService {
    @Autowired
    private SparkRepository sparkRepository; // never do this
}
```

## Related
- [[conventions/naming-patterns]]
- [[conventions/optional-return]]
