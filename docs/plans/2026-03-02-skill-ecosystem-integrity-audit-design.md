# Skill Ecosystem Integrity Audit (Manual-First) Design

**Date:** 2026-03-02  
**Repo:** `~/.codex/superpowers-codex`  
**Workstream slug:** `skill-ecosystem-integrity-audit`

## Goal

Perform a slow, painstaking, objective, file-by-file audit of this repository to:

1. Verify **referential integrity** within and between files.
2. Identify **friction / gaps** between skills so the ecosystem works seamlessly.
3. Identify **fluff / unnecessary content** that risks poisoning context, especially in frequently invoked skills.

## Scope (References to validate)

This audit treats the following as hard requirements to validate:

- `@file.md`-style mentions (local supporting docs).
- Inline file paths (for example: `docs/plans/...`, `skills/...`).
- Skill mentions (for example: `superpowers:<skill-name>` and YAML `name:` values).
- Quoted shell commands (ensure they are plausible for the repo/tooling and don’t rely on removed artifacts).
- External links (ensure they are still valid and represent the intended guidance).

## Acceptance Criteria

- Every `@...md` mention resolves to a real file in the intended directory (or is explicitly labeled as an example).
- Every inline file path resolves to a real file (or is explicitly labeled as an example).
- Every `superpowers:<skill>` mention refers to an actually available skill (and matches the ecosystem naming).
- README + high-frequency skills (especially `skills/using-superpowers/SKILL.md`) do not reference removed artifacts and do not contain redundant, confusing, or overly verbose guidance.
- Produce an audit report with each deficit captured as: evidence → impact → recommended remediation → verification approach.

## Audit Process

### Pass 0: Freeze the audited state

- Record the exact commit hash being audited.
- Ensure the worktree has a clean baseline `git status`.
- Review the last ~12 hours of commits to understand high-risk change areas (recent deletions/renames/consolidations).

### Pass 1: File-by-file audit (deterministic order)

Read in this order:

1. `README.md`
2. `docs/**`
3. `skills/*/SKILL.md` (alphabetical by skill directory)
4. Each skill’s local supporting files (`.md`, `.js`, `.dot`, etc.)

For each file, complete this rubric:

1. **Purpose clarity:** does this file have a single job? any redundant sections?
2. **Inbound refs:** what references this file? (targeted `rg` checks)
3. **Outbound refs:** list every reference type in “Scope” above and validate it
4. **Friction scan:** any contradictions with other skills’ gates, hard rules, or terminology?
5. **Context risk:** what would be harmful if this loads often? what can be trimmed or moved?

### Pass 2: Cross-file integrity sweep

Run focused searches to catch missed references:

- `docs/plans/`
- `@*.md`
- `superpowers:`
- any recently removed/renamed artifacts discovered during Pass 0

### Pass 3: Ecosystem seam checks

Verify the required bridges are consistent and unambiguous:

- `brainstorming` → `using-git-worktrees` → `writing-plans` → `executing-plans`
- no skill requires unavailable tools/modes without an explicit fallback

## Outputs

### Audit Report

Create: `docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md`

Include:

- Inventory of files reviewed (in order)
- Deficit table: `ID | Severity | File | Evidence | Why it matters | Proposed fix | Verification`
- Context-poisoning candidates and trimming/move recommendations

### Remediation Plan

After the audit report is reviewed, create:

- `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-remediations-implementation.md`

This plan will translate each deficit into bite-sized tasks (including verification steps and commits).

## Known likely deficit (seed)

`skills/requesting-code-review/SKILL.md` references `docs/plans/deployment-plan.md`, which does not appear to exist in this repo. This should be validated and either fixed or explicitly marked as an example during the audit.
