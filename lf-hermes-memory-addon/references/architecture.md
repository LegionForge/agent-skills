# lf-hermes-memory-addon — Reference Architecture

This directory is the design + reusable skill for a durable memory system.
It holds no runtime memory data. The live memory lives in a separate repo or
working tree chosen by the user.

## Problem
An injected memory store is small and must stay small. If everything is inlined,
growth eventually forces deletion under pressure.

## Solution
Use the tiny injected store as a **Map-of-Content (MOC) / router**; push the durable
detail to **on-disk markdown files** loaded on demand via `read_file` or a small
section-bounded retrieval helper.

## Tiers
- **Tier 0 — always injected** (`memory` store): this MOC + a hot-facts slice.
  Always-on rules live here, never deferred to disk.
- **Tier 1 — domain md files:** `soul`, `directives`, `user-profile`, `projects`, `notes`, `_MOC`.
- **Tier 2 — `archive/`:** finished/stale content; still searchable.

## Files (in the memory repo)
| file | holds |
|------|-------|
| `soul.md` | identity, persona, food, fear |
| `directives.md` | hard rules + name maps |
| `user-profile.md` | user profile + roster data |
| `projects.md` | active project state |
| `notes.md` | environment facts, conventions, lessons |
| `_MOC.md` | index: tiers, tags, load discipline, version-control notes |
| `archive/` | cold storage (grep-searchable) |
| `retrieve.py` | section-bounded retrieval (returns a whole section, not a whole file) |
| `integrity.sh` | verifies every `_MOC.md` pointer resolves to a real file |
| `commit.sh` | refreshes manifest.json, runs integrity, commits all |
| `backup.sh` | tar snapshot + optional `git push` to remote |
| `manifest_refresh.py` | writes `manifest.json` (per-file size + mtime) |

## Conventions
- Keep memory files terse and high-signal.
- Keep hard rules verbatim — never rewrite a parse-critical trigger.
- Prefer grep-before-load: retrieve the smallest useful section first.
- Use tags or keywords in `_MOC.md` for mechanical routing.
- Run integrity checks before committing changes.

## Version control
- The memory repo should be a git repo.
- `commit.sh` refreshes `manifest.json` then commits; `integrity.sh` gates the commit.
- `backup.sh` can write a local snapshot and optionally push to a remote.

## Design decisions
- Markdown over SQLite at this scale — human-editable, git-able, grep-able.
  Revisit SQLite/FTS5/vector indexing only if fact count grows large or semantic recall
  becomes necessary.
- Single source of truth: Tier 0 routes, never caches content.

## Deploy to a fresh host
1. Clone or copy the memory repo to a local path of your choice.
2. Inject the Tier-0 hot-facts block into the `memory` store.
3. Verify the retrieval helper and integrity check in the new location.

## Future
- Semantic retrieval: FTS5/vector SQLite index over the markdown files for top-k recall.
- Cron: daily `backup.sh`.
- Keep the skill repo public and the live memory repo private if desired.
