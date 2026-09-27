# tacticl-knowledge

> The **Tacticl PDLC knowledge vault**: an Obsidian vault of markdown pages that
> the Tacticl pipeline's agent roles read and that the **RETRO_ANALYST** role
> maintains after every pipeline run (Karpathy's "LLM Wiki" pattern). There is no
> code and no build. The markdown is the deliverable.

The pages describe **tacticl-core**: its conventions, entities, decisions and
gotchas (Gradle modules, Jackson 3, Firestore, PASETO auth). The design docs
behind this vault live there too. `README.md` links the PDLC v2 SAD and the
Knowledge Vault Design under `../tacticl-core/docs/superpowers/specs/`. On a push
to `main` that touches the pages, the indexer from **cidadel-ai-arbiter** loads
them into Qdrant (see below).

## Layout

```
tacticl-knowledge/
├── schema.md              the rules every page follows: format, AUTO vs PROPOSE, health check
├── raw/                   immutable source material
│   └── role-templates/    retro-analyst-boot.md, the RETRO_ANALYST prompt template
├── wiki/auto/             auto-committed facts, no approval needed
│   ├── conventions/       codebase conventions (Jackson 3 imports, base classes, naming, ...)
│   ├── entities/          domain entities (Spark, PipelineRun, SocialPost, Device)
│   └── moc/               one guide per PDLC role, 12 in all: <role>-guide.md
├── approved/              human-approved learnings
│   ├── decisions/
│   └── gotchas/
├── .obsidian/             shared vault config (app.json, graph.json); workspace files are gitignored
└── .github/workflows/     index-knowledge.yml
```

`wiki/proposed/` and `raw/run-summaries/`, which `README.md` and `schema.md`
name, don't exist yet. The first proposed page or run summary creates them.

## Use, check, index

- **Open it:** open the repo folder as a vault in Obsidian. The graph view colors
  MOCs, conventions, `approved/` and `wiki/proposed/` (`.obsidian/graph.json`).
- **Build and test:** there is nothing to build. The repo has no package files,
  tests or linter. The only check is the manual health check in `schema.md`, which
  whoever writes pages runs before committing.
- **Index:** `.github/workflows/index-knowledge.yml` runs on a push to `main` that
  touches `wiki/**` or `approved/**`, and on manual dispatch:
  ```bash
  gh workflow run index-knowledge.yml
  ```
  It checks out this repo, clones cidadel-ai-arbiter, and runs that repo's
  `scripts/index-knowledge.ts` with `PRODUCT: tacticl` to sync the markdown into
  Qdrant. It reads its credentials from repo secrets. Pushes that touch only other
  paths (root files, `raw/`, `.obsidian/`) don't start it.

## Writing pages

`schema.md` is authoritative. Read it before writing a page. In short:

- **Frontmatter:** `tags`, `roles`, `auto-approved`, `created`, `last-updated`,
  `pipeline-run`. The seed pages use `pipeline-run: seed`.
- **Sections, in order:** `## What`, `## Why`, `## How`, `## Example`,
  `## Related`. Every page must have all five and at least one example. The
  role MOCs in `wiki/auto/moc/` don't: they use the role-guide format instead.
- **One concept per file.** Write for an AI agent in the imperative ("do",
  "do not"), never "consider" or "may want to".
- **Backlinks:** link a new page back from every page in its `## Related`, and
  from the MOC of every role in its `roles:`.
- **Tier:** `wiki/auto/` is only for factual, repeated (at least 3 runs),
  non-security, non-architectural learnings. Everything else goes to
  `wiki/proposed/` through a PR. When in doubt, propose.

## Gotchas

- **Links are Obsidian wikilinks written as path suffixes, not markdown links**
  (`useMarkdownLinks: false` in `.obsidian/app.json`). Existing pages write
  `[[conventions/base-classes]]`, `[[entities/spark-entity]]` and
  `[[approved/gotchas/vault-https-localhost]]`. Obsidian rewrites links when it
  renames a file (`alwaysUpdateLinks`), but `git mv` doesn't, so fix inbound
  links by hand.
- **The role MOCs hold no wikilinks right now.** Commits `1ff37ee`, `cb6e64b`
  and `d0d2f1b` rewrote `wiki/auto/moc/*-guide.md` as full role guides and
  dropped the page links that `1e7dfbd` had. Health check 5 (every page is linked
  from its roles' MOCs) fails until those links come back.
- **The MOC bodies are copies.** Per its commit message, `d0d2f1b` copied them
  from tacticl-core's `role-identities/`. Nothing in this repo keeps them in sync,
  so they can drift from it. That rewrite also left `wiki/auto/moc/pm-guide.md`
  with no frontmatter.
- **RETRO_ANALYST's git flow isn't yours.** `raw/role-templates/retro-analyst-boot.md`
  has the pipeline agent run `git add .`, push `main`, and open PRs from
  `proposed/<run>-<slug>` branches, all inside its own clone at `/workspace/vault`.
  In this checkout, follow the commit rule below.
- **`raw/` and `approved/` are protected.** Per the boot template, never modify
  existing `raw/` content (only add run summaries), and change `approved/` pages
  only through a PR. Ask the owner before you edit either.
- **This repo is public** and its pages are indexed. Keep secrets, tokens and
  private hostnames out of every page.
- `docs/superpowers/` is gitignored as a local tool cache. Don't commit it.

## Commit as you go

This is a standing instruction, so don't wait to be asked: commit your own
finished work as you go. Commit each finished piece when it's done, and at the
latest before ending your turn. A machine crash (2026-09-26) orphaned work that
several parallel sessions had left uncommitted.

- **Commit only the paths you changed:** `git add -- <paths> && git commit -m '…' -- <paths>`.
  Never `git add -A` or `git commit -a`. Several sessions often share this
  checkout, and the other changes are theirs.
- **One commit per logical change**, with a real message, straight to `main`
  (solo developer). Then push. Pushing `main` with changes under `wiki/` or `approved/` runs `index-knowledge.yml`, which re-indexes the vault into Qdrant, and a push that touches only other paths triggers nothing.
- Leave something uncommitted only if the user said so, it's broken or
  mid-verification, or it isn't yours, and say which.
- Never keep the only copy of work in `/tmp` or a scratchpad (wiped on reboot),
  and land worktree branches on `main`.
- Uncommitted changes you find at session start may belong to a crashed or
  still-running session. Don't commit them as yours, and don't discard them,
  without checking.
- On the author's machine, a `commit-guard` hook (`~/.claude/hooks/`) enforces
  this. At the end of a turn it lists your uncommitted files once, and Claude
  Code labels that block "Stop hook error". Commit the listed paths.
