---
tags: [gotcha]
roles: [IMPLEMENTER, DEVOPS, TESTER]
auto-approved: false
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# Anthropic 403 = Missing API Key in Vault

## What
An HTTP 403 response from the Anthropic API during local dev or testing almost always means the API key is not loaded from Vault, not that the key is invalid.

## Why
Tacticl loads LLM API keys from Vault at startup (`secret/strategiz/anthropic`, key: `api-key`). If Vault is not running, the token is not exported, or the secret path is wrong, the key loads as null or empty — causing a 403 on the first LLM call.

## How
When you see a 403 from Anthropic:
1. Check Vault is running: `vault status` (should show `Sealed: false`)
2. Check `VAULT_TOKEN` is exported in your shell
3. Check the secret exists: `vault kv get secret/strategiz/anthropic` — look for `api-key` in the output
4. Restart tacticl-core after fixing Vault — keys are loaded at startup, not per-request

Vault context for Anthropic: `strategiz` (NOT `tacticl`). The LLM keys are shared with Strategiz.

## Example
```bash
# Check Vault status
vault status

# Verify the key exists
vault kv get secret/strategiz/anthropic
# Expected output includes: api-key = sk-ant-...

# If missing, set it
vault kv put secret/strategiz/anthropic api-key=sk-ant-YOUR-KEY-HERE
```

## Related
- [[approved/gotchas/vault-https-localhost]]
