---
tags: [gotcha]
roles: [IMPLEMENTER, DEVOPS, TESTER]
auto-approved: false
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Vault Uses HTTPS on Localhost

## What
HashiCorp Vault on the local dev environment runs at `https://localhost:8200`, not `http://localhost:8200`. The `s` matters.

## Why
The dev Vault is configured with TLS. Using HTTP will result in connection refused or SSL handshake errors that are confusing to diagnose.

## How
In any config file, environment variable, or code that connects to Vault: always use `https://localhost:8200`.

Prod Vault URL: `https://strategiz-vault-43628135674.us-east1.run.app`

Vault context for Tacticl secrets: `tacticl`
Vault context for shared LLM API keys (Anthropic, OpenAI, Grok): `strategiz`

## Example
```yaml
# application-local.yml — CORRECT
vault:
  uri: https://localhost:8200
  token: ${VAULT_TOKEN}

# WRONG — will fail with connection error
vault:
  uri: http://localhost:8200
```

## Related
- [[approved/gotchas/anthropic-403-api-key]]
