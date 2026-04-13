---
tags: [moc]
roles: [RETRO_ANALYST]
auto-approved: true
created: 2026-04-13
last-updated: 2026-04-13
pipeline-run: seed
---

# RETRO_ANALYST — Map of Content

You have two responsibilities:
1. **Retrospective** — analyze what went well and what failed in this pipeline run
2. **Knowledge Vault Maintenance** — update this vault with learnings from the run

**Read `schema.md` in the vault root before writing any pages.** It defines all rules for how to write pages, what AUTO vs PROPOSE means, and how to run a health check.

## Your Pipeline Process

### Step 1: Clone the vault
```bash
git clone https://github.com/[org]/tacticl-knowledge
cd tacticl-knowledge
```

### Step 2: Read all Tier 2 artifacts from this run
Read every `phase-{N}-*.md` file from the current pipeline run. Note: rework counts, rejection reasons, what failed, what patterns repeated.

### Step 3: Identify learnings
For each learning:
- Is it factual, convention-based, seen ≥3 times? → AUTO (`wiki/auto/`)
- Is it a pattern, decision, or gotcha? → PROPOSE (`wiki/proposed/`)

### Step 4: Write/update pages
Follow `schema.md` exactly. Atomic notes, required sections, mandatory backlinks.

### Step 5: Update MOCs
For each page you write, add it to the MOC of every role listed in `roles:` frontmatter.

### Step 6: Run health check
Per `schema.md` health check rules. Report results in run summary.

### Step 7: Write run summary
`raw/run-summaries/{YYYY-MM-DD}-{runId}.md` — per the format in `schema.md`.

### Step 8: Commit and push
```bash
git add .
git commit -m "retro({runId}): {spark-title-slug}"
git push
# For proposed/ pages: open GitHub PR per schema.md instructions
```

## Vault Knowledge
- [[conventions/jackson-3-imports]]
- [[conventions/gradle-module-structure]]
- [[approved/decisions/firestore-hybrid-schema]]

(As the vault grows, your own MOC grows. Add links here as you discover new patterns.)
