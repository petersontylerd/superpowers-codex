# Codex-Only Superpowers Repository Design

**Date:** 2026-03-02  
**Status:** Approved (ready for implementation plan)

## Goal

Thin `/home/ubuntu/.codex/superpowers` into a **Codex-only** repository, minimizing risk of breaking Codex skills discovery and usage.

## Non-Goals

- Add net-new skills or workflows.
- Re-architect the skill content beyond what is needed to support the Codex-only repo goal.
- Preserve compatibility with Claude Code, Cursor, or OpenCode.

## Background / Current State

This repository currently contains:

- A `skills/` tree (Codex-relevant).
- Platform packaging and support artifacts for Claude Code, Cursor, and OpenCode (not needed for Codex native skill discovery).
- Docs and tests largely oriented around those other platforms.

Codex skills best practices:

- Skills are discovered from both `$HOME/.agents/skills` and repo-local `.agents/skills` (from `$CWD` upward to repo root).
- Each skill is a directory containing `SKILL.md`.
- Symlinked skill folders are supported (our current install uses `~/.agents/skills/superpowers-codex -> ~/.codex/superpowers-codex/skills`).  

References:
- OpenAI `openai/skills` repository docs (Codex).  

## Decisions

### Decision 1: “Codex-only” means remove other platforms entirely

We will delete (or replace) artifacts that exist to support:

- Claude Code plugin/hooks
- Cursor plugin manifest
- OpenCode plugin implementation

### Decision 2: Keep `docs/plans/` — but only for this repo’s own work

Codex does not require `docs/` for skill discovery or execution. However, this refactor changes the superpowers repository itself, so the design and plan artifacts should live here for traceability.

We will keep `docs/plans/` in this repo, but:

- Delete all existing plan/design docs that are not Codex-only.
- Replace them with a minimal set:
  - This design doc
  - The implementation plan doc produced next

### Decision 3: Update skill wording to avoid “plans belong in the skills repo” confusion

We will update both:

- `skills/brainstorming/SKILL.md`
- `skills/writing-plans/SKILL.md`

To explicitly state:

- Design/plan artifacts are saved in `docs/plans/...` **in the target repository/worktree being built** (create the folder if missing).
- A shared skills library clone (like `~/.codex/superpowers-codex`) should only receive plans when the user is actively changing that repo.

## Proposed Keep/Delete Map (Repository Level)

### Keep (Codex runtime-critical)

- `skills/` (entire tree)

### Keep (Codex-only install/docs)

- `.codex/INSTALL.md`
- `docs/README.codex.md`
- `LICENSE`

### Keep but rewrite (Codex-only)

- `README.md` (remove non-Codex platform instructions and references)
- `.gitignore`, `.gitattributes` (optional; keep unless they reference deleted platform tooling)

### Delete (non-Codex platform support and clutter)

- `.claude-plugin/`
- `.cursor-plugin/`
- `.opencode/`
- `hooks/`
- `commands/`
- `lib/`
- `tests/`
- `agents/`
- `RELEASE-NOTES.md`
- `docs/README.opencode.md`
- `docs/testing.md`
- `docs/windows/`
- `docs/plans/*` (existing files; replace with only this doc + new implementation plan)

## Safety / Verification Strategy

### Pre-flight (before deletions)

1. Confirm symlink resolves as expected:
   - `readlink -f ~/.agents/skills/superpowers-codex` should resolve to this repo’s `skills/`.
2. Prove `skills/**` does not depend on deleted paths:
   - `rg` through `skills/` for references to directories/files slated for deletion.

### Post-change verification (after deletions + rewrites)

1. Ensure repo no longer contains non-Codex platform references:
   - `rg -n "claude|cursor|opencode|CLAUDE_PLUGIN_ROOT|hooks\\.json|\\.cursor-plugin|\\.claude-plugin|\\.opencode"`.
2. Manual Codex smoke test:
   - Restart Codex.
   - Verify skills are discoverable and invocable via Codex (at minimum `using-superpowers` and one additional skill).

### Rollback

Implementation should be structured as 2 commits:

1. “chore: remove non-codex platform artifacts”
2. “docs: codex-only docs + skill wording”

So any portion can be reverted cleanly with `git revert`.

## Implementation Plan Next Step

Create `docs/plans/2026-03-02-codex-only-superpowers-implementation.md` following the `writing-plans` skill, using cautious tasks and verification gates.
