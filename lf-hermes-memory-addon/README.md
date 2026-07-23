# lf-hermes-memory-addon

A public Agent Skill for designing and maintaining a durable file-based memory system for agents and LLMs.

## What this skill does

This skill teaches a pattern for extending memory beyond the small injected memory store by moving durable information into versioned markdown files on disk.

It is meant for cases where you want:
- a compact memory router instead of a giant memory dump
- human-readable memory files that can be reviewed and edited directly
- a predictable retrieval workflow that loads only the smallest useful section
- a memory system that can be backed up, copied, and restored across machines

### Core idea

Use the injected memory store only for hot facts and routing. Put long-lived detail into markdown files such as:
- `soul.md`
- `directives.md`
- `user-profile.md`
- `projects.md`
- `notes.md`
- `_MOC.md`

The skill is about the architecture of memory, not about storing one particular person's private notes.

## Why the architecture looks this way

The design is intentionally split into small pieces:

1. **Injected memory stays tiny**
   - The injected memory store is limited and should remain fast to load.
   - It should act like a router or index, not a warehouse.

2. **Durable content lives on disk**
   - Markdown is easy for humans to read, search, diff, and review.
   - Git makes the memory repo versioned and auditable.

3. **Section-bounded retrieval comes first**
   - Instead of loading a whole file, retrieve the smallest relevant section first.
   - This keeps context usage efficient and reduces noise.

4. **The index stays smaller than the content it points to**
   - The `_MOC.md` file should route, not duplicate.
   - Duplication causes drift, so the index should remain compact.

5. **The system is portable**
   - The same memory structure can be copied to a new host.
   - The public skill can be reused while the actual live memory repo stays private if desired.

## Installation

There are two common ways to install this skill.

### Option 1: Install from the public repo

If your Agent Skills toolchain supports repo-based install, use:

```bash
npx skills@latest add LegionForge/agent-skills/lf-hermes-memory-addon
```

To install globally across projects:

```bash
npx skills@latest add LegionForge/agent-skills/lf-hermes-memory-addon --global
```

### Option 2: Copy the skill directory manually

If you prefer a local install, copy the entire `lf-hermes-memory-addon/` directory into your agent skills library.

The important part is that the directory contains:
- `skill.md`
- `references/architecture.md`
- any additional support files you want to ship with it

### After installation

Once installed, load the skill whenever you need to:
- design a durable memory system
- create or maintain a memory MOC/router
- decide how to split hot facts from durable details
- package memory files into a reusable agent workflow

## Removal and deletion

There are two different actions people often mean here.

### Remove the skill from active use

This means the skill is no longer installed or selected by the agent, but the files may still exist on disk.

Common ways to do that:
- uninstall it from your Agent Skills library
- remove the skill directory from the skills path
- disable it if your toolchain supports per-skill enable/disable controls

### Delete the skill files

This means removing the skill directory entirely from disk.

If you want to fully delete it:
- delete the `lf-hermes-memory-addon/` directory from the skills library
- delete any local clone of the repository if you no longer need it
- if you published a fork or mirror, delete or archive that remote copy separately

### What to keep in mind before deleting

If the skill has already been used in other sessions or projects, deleting the local files does not necessarily remove copies that were already cloned, installed, or referenced elsewhere.

If you want to retire the skill cleanly, consider:
- removing it from the active skills library
- keeping the git history intact in the public repo
- marking the repo archived instead of deleting it outright

## Contents

- `skill.md` — the skill entry point
- `references/architecture.md` — supporting architecture notes
- `LICENSE` — MIT license
- `CONTRIBUTING.md` — contribution guidance

## License

MIT
