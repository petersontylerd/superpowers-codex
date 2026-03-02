# Skill Ecosystem Integrity Audit Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Produce an objective, file-by-file integrity audit report and a concrete remediation plan for the `superpowers-codex` skill ecosystem.

**Architecture:** Manual-first audit using a deterministic read order and a fixed rubric; use targeted `rg`/`git` checks to support (not replace) human judgment. Findings are recorded as a deficit table with evidence, impact, and recommended fixes.

**Tech Stack:** Git worktrees, ripgrep (`rg`), bash, Markdown

---

### Task 1: Preflight (worktree + baseline)

**Files:**
- Modify: `docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md`

**Step 1: Confirm worktree + branch**

Run: `pwd && git rev-parse --abbrev-ref HEAD && file .git`  
Expected: branch starts with `feat/` and `.git` is a file.

**Step 2: Record audited commit + confirm clean status**

Run: `git rev-parse HEAD && git status -sb`  
Expected: clean working tree.

**Step 3: Snapshot recent changes for audit context**

Run: `git log --since='12 hours ago' --oneline --decorate --stat`  
Expected: list of recent deletes/renames to pay attention to during reference checks.

**Step 4: Create audit report skeleton**

Run: `mkdir -p docs/reports`

Create: `docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md`

Template:
```md
# Skill Ecosystem Integrity Audit (Manual-First)

**Date:** 2026-03-02
**Audited commit:** <SHA>
**Worktree:** .worktrees/skill-ecosystem-integrity-audit

## Inventory (review order)
- [ ] README.md
- [ ] .codex/INSTALL.md
- [ ] docs/plans/*
- [ ] skills/* (see below)

## Deficits

| ID | Severity | File | Evidence | Why it matters | Proposed fix | Verification |
|---:|:--|:--|:--|:--|:--|:--|

## Context-Poisoning Candidates

## Seam Checks (workflow bridges)
```

**Step 5: Commit report skeleton**

Run:
```bash
git add docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md
git commit -m "docs(reports): add skill ecosystem integrity audit skeleton"
```

---

### Task 2: Audit README.md (entrypoint integrity)

**Files:**
- Modify: `docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md`
- Audit: `README.md`

**Step 1: Read and apply rubric**

Check:
- Purpose clarity and duplication
- Outbound refs: file paths, skill mentions, commands, external links
- Friction: claims vs actual repo contents

**Step 2: Validate internal references**

Run:
```bash
rg -n 'superpowers:|skills/|docs/|\\.codex/' README.md
```
Expected: every referenced file/skill is real or explicitly marked as an example.

**Step 3: Record findings as deficit rows (if any)**

Add rows to the Deficits table with concrete evidence (exact quote + file path).

Also add a short Evidence note in the report (even if no deficits):
- exact command(s) run
- 1-line summary of what was found (e.g., match count / which refs were checked)
- any referenced local paths you spot-checked with `test -f <path>`

---

### Task 3: Audit `.codex/INSTALL.md` (installation integrity)

**Files:**
- Modify: `docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md`
- Audit: `.codex/INSTALL.md`

**Step 1: Read and apply rubric**

Focus:
- The symlink instruction and any absolute paths
- Any commands that could drift across platforms

**Step 2: Validate symlink guidance matches current reality**

Run:
```bash
ls -la /home/ubuntu/.agents/skills/superpowers-codex
```
Expected: symlink points to `~/.codex/superpowers-codex/skills`.

**Step 3: Record findings**

Also add a short Evidence note in the report:
- exact command(s) run (including the symlink check)
- 1-line summary of whether the symlink target matches the expected path

---

### Task 4: Audit docs/plans (plan location + examples)

**Files:**
- Modify: `docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md`
- Audit: `docs/plans/.gitkeep`
- Audit: `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-design.md`

**Step 1: Confirm docs/plans minimalism doesn’t break references**

Run: `find docs/plans -maxdepth 1 -type f -print | sort`  
Expected: only `.gitkeep` plus any active audit/design docs.

**Step 2: Record any mismatch between examples and current repo policy**

---

### Task 5: Audit each skill SKILL.md (file-by-file, deterministic)

**Files:**
- Modify: `docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md`
- Audit: `skills/auditing-plan-execution/SKILL.md`
- Audit: `skills/auditing-writing-plans/SKILL.md`
- Audit: `skills/brainstorming/SKILL.md`
- Audit: `skills/executing-plans/SKILL.md`
- Audit: `skills/finishing-a-development-branch/SKILL.md`
- Audit: `skills/receiving-code-review/SKILL.md`
- Audit: `skills/requesting-code-review/SKILL.md`
- Audit: `skills/systematic-debugging/SKILL.md`
- Audit: `skills/test-driven-development/SKILL.md`
- Audit: `skills/using-git-worktrees/SKILL.md`
- Audit: `skills/using-superpowers/SKILL.md`
- Audit: `skills/verification-before-completion/SKILL.md`
- Audit: `skills/writing-plans/SKILL.md`
- Audit: `skills/writing-skills/SKILL.md`

**Step 1: For each file, validate YAML + triggering integrity**

Checklist:
- YAML `name` is kebab-case and matches directory intent
- YAML `description` is strictly “Use when…” triggers (no workflow summary shortcuts)

**Step 2: For each file, validate outbound references**

Run (per file):
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' <FILE>
```

Validation actions:
- Local `@...md` / `docs/...` / `skills/...` references: `test -f <path>` (or mark as example)
- Skill mentions: confirm the target skill exists under `skills/<name>/SKILL.md`
- Commands: sanity-check they fit this repo (no removed tooling)
- External links: spot-check in browser or via `curl -I` (record failures)

**Step 3: Record findings**

Each deficit row must include:
- exact snippet
- file path
- why it breaks integrity or adds friction
- smallest viable fix

---

### Task 6: Audit supporting files (non-SKILL.md) referenced by skills

**Files:**
- Modify: `docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md`
- Audit: `skills/requesting-code-review/code-reviewer.md`
- Audit: `skills/systematic-debugging/condition-based-waiting.md`
- Audit: `skills/systematic-debugging/defense-in-depth.md`
- Audit: `skills/systematic-debugging/root-cause-tracing.md`
- Audit: `skills/test-driven-development/testing-anti-patterns.md`
- Audit: `skills/writing-skills/persuasion-principles.md`
- Audit: `skills/writing-skills/testing-skills-under-pressure.md`
- Audit: `skills/writing-skills/graphviz-conventions.dot`
- Audit: `skills/writing-skills/render-graphs.js`

**Step 1: Validate any references inside these files**

Same outbound-reference checks as Task 5.

**Step 2: Context-poison scan**

Flag content that:
- is rarely needed but likely to be loaded frequently
- repeats guidance already present elsewhere
- increases confusion (multiple competing “hard gates”)

---

### Task 7: Cross-file integrity sweep (catch-all)

**Files:**
- Modify: `docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md`

**Step 1: Sweep for internal path references**

Run:
```bash
rg -n "docs/plans/|docs/reports/|skills/|\\.codex/|@[A-Za-z0-9_./-]+\\.md" -S .
```
Expected: every reference resolves or is explicitly marked as an example.

**Step 2: Sweep for legacy/removed artifacts**

Run (extend as you discover removed names during audit):
```bash
rg -n "opencode|cursor|claude-plugin|dispatching-parallel-agents|subagent-driven-development" -S . || true
```
Expected: no hits.

---

### Task 8: Seam checks (workflow bridges)

**Files:**
- Modify: `docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md`

**Step 1: Verify canonical bridge ordering is consistent**

Manually verify these handoffs in the skill texts:
- `brainstorming` requires `using-git-worktrees` after approval
- `writing-plans` gates to worktree + `feat/*` branch
- `executing-plans` references verification-before-completion (evidence over claims)
- `finishing-a-development-branch` closes the loop (cleanup)

**Step 2: Record any contradictions as deficits**

---

### Task 9: Produce remediation proposal (separate plan doc)

**Files:**
- Create: `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-remediations-implementation.md`

**Step 1: Convert each deficit row into a bite-sized remediation task**

Each remediation task must include:
- exact files + exact edits
- how to verify (search + sanity checks)
- commit step

**Step 2: Stop for human review**

Do not implement remediations until the remediation plan is approved.
