---
tags: [decision]
roles: [ARCHITECT, SECURITY_ANALYST, IMPLEMENTER]
auto-approved: false
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Auth Uses PASETO v4.local (Not JWT)

## What
Tacticl uses PASETO v4.local (symmetric encryption) for auth tokens. JWT is NOT used.

## Why
PASETO v4.local is simpler than JWT: no algorithm confusion attacks, no "none" algorithm vulnerability, no library fragmentation. The symmetric key is shared between Tacticl and Strategiz enabling cross-product SSO without token exchange.

## How
Token issuance: `cidadel-core` `framework-token-issuance` library. Do NOT implement your own token logic.

Token validation: use `@RequireAuth` annotation on controllers. The framework handles validation.

Claims: `userId`, `scopes[]`, `product` (`tacticl`), `deviceId` (optional), `issuedAt`, `expiresAt`.

Token lifetime: 15 minutes (access) + 30 days (refresh).

Vault path for symmetric key: managed by `framework-token-issuance` — do not read or use the raw key in application code.

## Example
```java
// CORRECT — use the annotation
@RequireAuth
@GetMapping("/v1/sparks")
public ResponseEntity<List<SparkResponse>> getSparks(AuthenticatedUser user) {
    return ok(sparkService.getSparks(user.getUserId()));
}

// WRONG — manual token parsing
String userId = jwtParser.parse(token).getClaim("userId"); // never do this
```

## Related
- [[approved/decisions/firestore-hybrid-schema]]
