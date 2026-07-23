---
name: lf-hermes-memory-addon
description: Design and maintain a durable file-based memory system for agents and LLMs. Use when the injected memory store is too small, when you want a compact MOC router, or when memory should live in versioned markdown files on disk.
license: MIT
compatibility: Requires local filesystem access and git. Optional GitHub access is useful for publishing and backups.
metadata:
  author: Hermes Agent
  version: "1.1.0"
  copyright: "2026 LegionForge.org <info@legionforge.org>"
  attribution: "Created with ChatGPT 5.4-mini"
---

# lf-hermes-memory-addon

## Overview
Use this skill to build an expandable memory system for agents and LLMs.

The injected memory store should stay compact: it acts as a router and holds only
the hot facts that must be available immediately. Durable detail lives in versioned
markdown files on disk, where it can be searched, reviewed, backed up, and copied
to a new host.

## When to use
- The injected memory store is running out of space
- You want durable, version-controlled memory
- You need a predictable file-based retrieval workflow
- You want agents to load detail only when needed

## Core pattern
1. Keep the injected memory store as a router, not a dump.
2. Store detailed memory in small markdown files on disk.
3. Use section-bounded retrieval before loading whole files.
4. Keep the index smaller than the files it points to.
5. Back up and version the memory repo regularly.

## Support files
- `references/architecture.md` — general architecture, tiering, and deployment notes

## Pitfalls
1. **Putting always-on rules only on disk.** Immediate triggers must live in the injected tier.
2. **Duplicating the same fact in multiple places.** Use a single source of truth to prevent drift.
3. **Letting the router become storage.** The index should stay compact and mechanical.
4. **Loading whole files by default.** Prefer bounded retrieval first, full reads second.

## Publishing notes
- Do not hardcode machine-specific paths, hostnames, IP addresses, usernames, tokens, or private repo names in the published skill.
- Keep examples generic; use placeholders like `<memory-repo>` or `~/memory-backup`.
- If a detail only applies to one workstation, keep it out of the public skill.

## Verification
- [ ] Frontmatter starts at byte 0 with `---`
- [ ] `name` matches the directory name: `lf-hermes-memory-addon`
- [ ] Description is trigger-focused and specific
- [ ] Supporting detail lives under `references/`
- [ ] Root skill stays smaller than the files it routes to
