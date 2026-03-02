---
name: auditing-writing-plans
description: Use when an existing implementation plan (especially one written with superpowers:writing-plans) needs an independent quality audit for gaps, errors, missing concreteness, or oversized steps before execution.
---

# Auditing Writing-Plans Plans

## Overview

Independently evaluate a plan created with `superpowers:writing-plans` for gaps, errors, and weak execution guidance — and **apply fixes directly to the plan file** (not just commentary).

**Core principle:** Audit output is not “feedback”; it’s a patched plan plus a short audit report.

## When to Use

- The plan feels vague (“add tests”, “wire it up”, “deploy”), too large per step, or missing concrete paths/commands.
- You want to reduce execution risk before handing the plan to an executor.

## When NOT to Use

- There is no plan yet → use `superpowers:writing-plans`.
- You are implementing tasks right now → use `superpowers:executing-plans`.

## Required Input

- `plan_path` (required): Path to the plan markdown file to audit.

**Fail-fast requirement:** If `plan_path` is missing or empty, the audit MUST FAIL.

Required response (and then STOP — no audit, no suggested fixes, no patches):

`ERROR: Missing required input: plan_path`

Then ask the user to provide the plan path.

## Output Contract

1) A concise audit summary (what was wrong + what was fixed).
2) A patch applied to `plan_path` that fixes issues.
3) No trailing “Open Questions” lists. Any ambiguity MUST be resolved in one of these ways (in priority order):
   - **Infer** from the plan + user request.
   - **Discover** by running lightweight repo checks (e.g., `rg`, `ls`, reading config files) and patch the plan with concrete results.
   - **Plan the discovery** by adding explicit discovery steps *into the plan* (with commands + what to look for) so execution can proceed without needing follow-up chat.
   - **Lock a conservative assumption** by adding an `## Assumptions (Locked)` section in the plan that states the chosen default.

**Only if** a choice depends on user preference (and cannot be safely defaulted) you MUST ask the user 1–3 direct questions and STOP (do not patch the plan yet).

## Audit Workflow (Must Follow)

### Step 0: Load constraints
- Re-read the user request that the plan is meant to satisfy (if present).
- If constraints are unclear, do NOT add “Open Questions”. Resolve via discovery and/or lock a conservative assumption in the plan.

### Step 1: Validate writing-plans structure
Cross-check against `superpowers:writing-plans` requirements:
- Required header block (title, “For Claude” line, Goal, Architecture, Tech Stack, `---`)
- Task format + **Files:** with explicit paths
- Steps are bite-sized and include exact commands + expected outcomes
- TDD cadence appears (test → run fail → minimal impl → run pass → commit)

If missing, patch the plan to conform.

### Step 2: Evaluate each task for execution quality
For every task, ensure:
- The task is one cohesive deliverable (split if it’s doing 2+ things).
- Every step has: *where* (path), *how* (command/snippet), *verify* (expected output).

### Step 3: Apply fixes directly (required)
Apply edits to `plan_path` to resolve problems found:
- Replace vague steps with concrete steps (paths, code blocks, commands, expected results).
- Split oversized tasks into smaller tasks and renumber.
- Add missing **Files:** sections.
- Add missing test steps and verification commands.
- Add missing “Commit” steps (only as plan steps; do not actually commit code).

**Hard rule: Don’t invent repo-specific facts.**
- Prefer resolution over deferral:
  - If you can’t know an exact path/command, add a discovery step (concrete `rg`/`ls`/config-inspection commands + what the expected match looks like), then use the discovered result in subsequent steps.
  - If a placeholder is unavoidable, it MUST be paired with a discovery step that replaces it (no “ask later”).

### Step 4: Report
After patching:
- List the top issues fixed (3–8 bullets).
- List any assumptions you locked (if any) and why they’re safe defaults.

## Quick Reference (Checklist)

| Area | Red flag | Fix in plan |
|---|---|---|
| Header | Missing Goal/Architecture/Tech Stack | Insert required header block |
| Task sizing | A step takes 30+ minutes | Split into smaller steps/tasks |
| Files | “modify backend” (no path) | Add explicit file paths (or `rg` to discover) |
| Verification | No “Expected: FAIL/PASS” | Add explicit expected outcomes |
| TDD | Jumps to implementation | Add failing test + run + minimal impl |
| Ambiguity | Assumptions not stated | Add `## Assumptions (Locked)` + discovery steps (avoid “Open Questions”) |

## Common Mistakes

- Only writing feedback and not editing the plan file.
- Overwriting the author’s intent instead of tightening execution details.
- Guessing paths/commands instead of adding discovery steps (`rg`, `ls`, `pytest -q`, etc.).
- Adding new scope (features) that weren’t in the original goal.
