# gstack — vendored into this repo

This directory contains [Garry Tan's **gstack**](https://github.com/garrytan/gstack)
(version `1.58.5.0`) vendored as project-local Claude Code skills, so the gstack
slash-command suite (`/office-hours`, `/spec`, `/plan-*`, `/review`, `/qa`,
`/ship`, `/cso`, `/browse`, etc.) is available to anyone running Claude Code in
this repository.

## Layout

- `gstack/` — the full gstack framework (the top-level `SKILL.md` is the
  `gstack` router skill; each subdirectory is an individual skill).
- `gstack-<name>/` — per-skill discovery entries. Each contains a relative
  symlink `SKILL.md -> ../gstack/<name>/SKILL.md`, mirroring what gstack's own
  `setup --local --prefix` produces. This is what makes each skill
  individually discoverable by Claude Code at `.claude/skills/<dir>/SKILL.md`.

## What was vendored vs. omitted

To keep the commit reasonable, regenerable/non-skill content was pruned from the
upstream tarball:

- Test fixtures (`**/test/`, e.g. the 28 MB browser security-bench fixtures)
- The 9 MB prebuilt `lib/diagram-render/dist/` artifact

The skill definitions (`SKILL.md` files and their `sections/`, `specialists/`,
and checklist support files), plus `lib/`, `bin/`, and `scripts/`, are intact.

## Full functionality

Skill prompts/behaviors work as-is. Some skills also rely on runtime tooling
(Bun scripts, browser automation) that upstream wires up via its `./setup`
script (requires Bun 1.0+). To enable those parts, run gstack's installer
locally:

```
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack && cd ~/.claude/skills/gstack && ./setup
```

See `gstack/README.md` and `gstack/CLAUDE.md` for full documentation.
