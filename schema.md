# Knowledge Vault Schema

**For RETRO_ANALYST:** These are your rules. Follow them exactly. Every wiki page you write must conform to this schema. Health checks enforce it.

---

## Page Format

Every wiki page MUST have this structure — no exceptions:

```markdown
---
tags: [convention|pattern|decision|gotcha|entity]
roles: [PM, RESEARCHER, ARCHITECT, DESIGNER, PLANNER, IMPLEMENTER, REVIEWER, TESTER, SECURITY_ANALYST, TECHNICAL_WRITER, DEVOPS, RETRO_ANALYST]
auto-approved: true|false
created: YYYY-MM-DD
last-updated: YYYY-MM-DD
pipeline-run: run-{id}
---

# {Title}

## What
One sentence. What is this?

## Why
Why does this matter? What breaks if ignored?

## How
Concrete instructions. Do not use "consider" or "may want to". Use "do" and "do not".

## Example
Code, config, or command examples. Always include at least one.

## Related
- [[page-name]] — one-line description of the relationship
```

---

## Atomic Notes

One concept per file. If you find yourself writing "also, ..." — that is a new page.

---

## Backlinks

When you create a new page, you MUST:
1. Add a `[[this-new-page]]` entry to every page listed under `## Related`
2. Add the new page to the MOC of every role listed in `roles:` frontmatter

When you update a page, check if any Related pages need their backlinks updated.

---

## AUTO vs PROPOSE

**Write to `wiki/auto/` (auto-commit, no approval needed) when ALL of these are true:**
- The content is factual — it describes how the codebase currently works, not how it should work
- The same pattern appeared in ≥3 pipeline runs with no human rejection
- It is NOT security-related, NOT architectural (doesn't change how a role approaches a core task)
- No prior version of this page was rejected by the human reviewer

**Write to `wiki/proposed/` (create GitHub PR, await approval) when ANY of these is true:**
- It reverses or contradicts an existing approved or auto page
- It is security-related (authentication, authorization, secret handling, injection)
- It describes an architectural decision (database choice, framework choice, pattern change)
- It is a GOTCHA derived from a single run (wait for ≥2 runs before auto-committing gotchas)
- You are uncertain whether it should be auto or proposed

When in doubt: propose. It is better to slow down than to inject a wrong learning.

---

## Health Check (run every session)

After every write session, before committing:

1. Scan all pages for `[[broken-links]]` — links to pages that do not exist
2. Scan all pages listed in `## Related` to verify the backlink exists on the target page
3. Check all pages have all 5 required sections (What, Why, How, Example, Related)
4. Check for contradictions: if page A says "do X" and page B says "do not X", flag both for human review
5. Check all role MOCs — verify every page with `roles: [ROLE]` frontmatter is linked from that role's MOC

Report health check findings in `raw/run-summaries/{runId}.md` before committing.

---

## Run Summary Format

After every pipeline, write `raw/run-summaries/{YYYY-MM-DD}-{runId}.md`:

```markdown
---
pipeline-run-id: run-{id}
spark-id: spark-{id}
playbook: FULL_PDLC|BUG_FIX|...
outcome: COMPLETED|FAILED
date: YYYY-MM-DD
---

# Run Summary — {spark title}

## What Was Built
One paragraph.

## What Went Well
Bullet list. Be specific.

## What Failed or Was Reworked
Bullet list. Include rework counts.

## Learnings This Run
For each: what was learned, where it was written (wiki/auto/ or wiki/proposed/), why that tier.

## Health Check Results
Pass/fail for each check. List any issues found and how they were resolved.
```

---

## Tone

You are writing for an AI agent, not a human. Be explicit. Be direct. Use imperative mood.

- DO: "Always use `tools.jackson.databind.json.JsonMapper`. Never use `ObjectMapper`."
- DO NOT: "You may want to consider using Jackson 3's JsonMapper instead of the legacy ObjectMapper."

Short sentences. Code examples. No ambiguity.
