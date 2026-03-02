---
name: auditing-plan-execution
description: Use when a plan is already being implemented and an independent reviewer must compare the worktree to the plan, identify gaps or drift, and apply remediations before work continues.
---

# Auditing Plan Execution

## Overview

Provide an independent review of ongoing implementation **against a specific plan**. Catch misses, drift, and weak verification. Then **fix issues directly** in the worktree (and update the plan when needed) before handing work back.

**Core principle:** Stop-and-assess first; evidence before edits.

## Hard Guardrails (Do Not Finish The Plan)

This skill is an audit + remediation of **already-started** work. It is **not** a “finish the entire job” engine.

**Definition: “already-started task” (use evidence):** a plan task is started if you can point to concrete evidence in `worktree_path` that work began on it (commits/diff touching expected files, partially implemented code/tests, TODOs tied to that task, or artifacts the task would create).

**Allowed edits:**
- Fix drift/bugs in already-started tasks.
- Tighten verification for already-started tasks (tests, commands, expected outcomes).
- Patch the plan to add missing concrete execution detail (paths/commands/expected outcomes).
- Remove accidental scope creep (or explicitly justify it and patch the plan).

**Forbidden edits (hard stop):**
- Do **not** implement plan tasks that are clearly `not started`, even if doing so would make verification “green”.
- If a task is `not started`, your output is: diagnosis + handoff guidance (what to do next), not code.

**Stop condition:** If the next needed change is “start/implement a not-started task”, STOP remediation and produce the handoff message.

## Required Inputs (Fail Fast)

This skill requires two inputs:
- `plan_path`: path to the plan markdown file
- `worktree_path`: path to the worktree directory where the plan is being implemented

**Fail-fast requirement:** If either input is missing or empty, the review MUST FAIL.

Required response (and then STOP):

`ERROR: Missing required input(s): plan_path, worktree_path`

Then ask the user to provide both paths.

**Fail-fast path validation:** If provided but invalid, the review MUST FAIL.
- If `plan_path` does not exist as a file: `ERROR: plan_path does not exist or is not a file: <plan_path>`
- If `worktree_path` does not exist as a directory: `ERROR: worktree_path does not exist or is not a directory: <worktree_path>`

Then STOP and ask for corrected paths.

## Output Contract

1) **Execution audit report** (concise): strengths, gaps, drift, risks.
2) **Diagnosis** (explicit): for each gap/drift, state the likely root cause and the plan task it impacts.
3) **Remediations applied**: concrete code/tests/docs changes in `worktree_path` (and plan patches if needed), limited to remediating issues within **already-started** tasks.
4) **Verification evidence**: commands run + what passed/failed (include exact commands; no “tests pass” without evidence).
5) **Detailed handoff message** to the colleague: must be actionable enough that they can continue without asking follow-up questions.

**Handoff quality bar (hard requirement):**
- Include: current plan status, what changed (files), how to verify (commands + expected outcomes), what’s next, and known risks.
- If anything critical is unknown, resolve it now via repo discovery or ask 1–3 direct questions and STOP. Do not hand off with dangling “open questions” as a substitute for clarity.

## Workflow (Must Follow)

### Step 0: Stop and assess (no edits yet)

1) Read `plan_path` end-to-end.
2) Identify the plan’s intent and non-negotiables:
   - expected outputs/artifacts
   - verification commands and expected results
   - file paths the plan expects to change
3) Open a short scratch checklist:
   - Plan tasks: done / partial / not started
   - Known risks / unknowns

If the plan is missing critical details (paths/commands/expected outcomes), do not guess. Resolve ambiguity now:
- Patch the plan immediately to add concrete discovery steps (commands + what to look for) so execution can proceed.
- If a choice is preference-dependent and cannot be safely defaulted, ask the user 1–3 direct questions and STOP (do not proceed with edits).

### Step 1: Establish worktree ground truth (still no edits)

In `worktree_path`, gather evidence:
- `git status`
- `git log --oneline -n 30`
- `git diff` (and/or `git diff --stat`)
- Run ripgrep for “unfinished work” signals: `rg -n \"TODO|FIXME|HACK|TEMP|XXX\"`

If `worktree_path` isn’t a git repo, note it as a risk and ask the user whether it should be.

### Step 2: Compare work to plan

For each plan task:
- Confirm the expected files were created/modified.
- Confirm the verification step exists and was executed (or execute it now).
- Classify the task explicitly: `done / partial / not started` (with 1-line evidence).
- Identify drift:
  - skipped steps
  - merged steps (too large)
  - out-of-order steps that increase risk
  - scope creep not justified by the plan’s goal

**Guardrail reminder:** Only `done`/`partial` tasks are eligible for remediation edits. `not started` tasks become handoff items (and may justify plan patching for clarity), but must not be implemented.

**Per-issue loop (required):** For each gap/drift you find, do:
1) Evidence (what you observed; commands/files)
2) Diagnosis (why it happened; what it risks)
3) Fix (minimal change to bring work back to plan)
4) Verify (most specific verification that proves the fix)

### Step 3: Verification-first remediation strategy

Prefer to tighten correctness with verification before refactors:
- If tests exist for the area: run the most specific tests first.
- If tests don’t exist and behavior is changing: add tests before changing behavior.

**REQUIRED SUB-SKILL (when adding behavior or bugfixes):** use `superpowers:test-driven-development`.

If tests fail or behavior is confusing:
- **REQUIRED SUB-SKILL:** use `superpowers:systematic-debugging` before proposing fixes.

### Step 4: Apply remediations (edits allowed)

Fix what the audit finds, including:
- remediate drift/bugs in already-started plan tasks
- correct plan/implementation mismatches
- add or repair tests and verification steps
- remove accidental scope creep (or explicitly justify and patch plan)
- patch the plan if it is missing required execution detail (paths, commands, expected outcomes)

**Hard stop reminder:** If remediation would require starting a `not started` plan task, STOP and hand off. Do not “complete the plan” as part of the audit.

**Hard rule: Don’t invent facts.**
- Prefer discovery steps (e.g., `rg` to find where code lives) over guessing file paths.
- If something is unclear, prefer discovery and plan patching. Only ask questions when the answer is preference-dependent and cannot be safely defaulted.

### Step 5: Re-verify and summarize

Re-run the verification commands from the plan (or the most specific set you added).
Summarize:
- What was fixed (top 3–8 items)
- What risks remain (unknowns, deferred work)
- Exactly what commands passed and where results are visible

## Handoff Prompt Template (Copy/Paste)

End every execution audit with a message the user can paste to the colleague. This is a **success-critical deliverable**; do not be brief or cursory.

**Length sanity check:** If your handoff is shorter than ~20 lines, it is almost certainly missing required details. Expand until every required section is complete.

**Minimum required sections (use these headers):**

1) **Scope reviewed**
   - `plan_path`: …
   - `worktree_path`: …
   - `git status` summary (clean/dirty, branch)
   - `git rev-parse HEAD` (or equivalent) so the colleague can anchor the state
   - `git diff --stat` (or equivalent) for change magnitude
   - Plan checkpoint: which task(s) were under review

2) **Plan status (task-by-task)**
   - A short table/list: each plan task → `done / partial / not started` + 1-line evidence (files/commands)

3) **Diagnosis (why things were off)**
   - For each gap/drift: root cause + consequence + the plan task affected

4) **Changes applied**
   - Bullet list of files changed/added (explicit paths) + intent
   - Call out any plan edits (what changed in the plan and why)

5) **Verification evidence**
   - Exact commands run (copy/pasteable)
   - Results (PASS/FAIL) and where to see artifacts/logs
   - If something wasn’t run, say why and what must be run next

6) **Next actions (unblocked, concrete)**
   - The next 1–3 plan steps the colleague should execute, with the exact commands

7) **Risks / constraints**
   - Any remaining risks and how to mitigate
   - Any assumptions that were locked (and why they’re safe defaults)

**Tone requirement:** Firm, direct, and professional.
- Make expectations explicit (follow plan steps, verify as you go, ask before deviating).
- Avoid threats, insults, or personal commentary.
- It’s OK to state that work will be re-reviewed (e.g., “I will re-review before merge / before the next milestone”).

**Suggested closing lines (firm but professional):**
- “Please follow the plan task-by-task and run the verification steps exactly as written.”
- “If you need to deviate from the plan, stop and ask first; don’t improvise changes that create rework.”
- “I will re-review after you complete the next task and verification is green.”

## Common Mistakes

- Skipping the plan read and jumping to code changes.
- Writing “review comments” without applying fixes.
- Running broad tests without tying failures back to plan tasks.
- Allowing drift/scope creep without updating the plan.
- Finishing the entire plan instead of remediating already-started tasks + handing off.
- Guessing paths/intent instead of asking questions and documenting assumptions.
- Providing a short handoff that lacks file paths, commands, and plan status (this causes colleague failure).
