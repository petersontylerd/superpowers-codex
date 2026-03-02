# Skill Network Lifecycle (Worktrees + Verification Gates) Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Make the existing skills compose into a single, consistent lifecycle: `brainstorming → using-git-worktrees → writing-plans → executing-plans (+ /review checkpoints) → verification-before-completion → finishing-a-development-branch`, with a hard human-review gate before merge/push/cleanup.

**Architecture:** Minimal edits to existing `SKILL.md` files to remove contradictions and add explicit required bridges + stop conditions. No new orchestrator skill.

**Tech Stack:** Markdown (`SKILL.md`), git worktrees/branches.

---

## Global Constraints

- All edits happen on branch `feat/skill-network-lifecycle` in worktree `.worktrees/skill-network-lifecycle`.
- Keep changes minimal and localized to the targeted skills.
- Do not introduce new skills or scripts unless strictly required.
- Preserve existing tone/style where possible; prefer small insertions over rewrites.
- Implementation plan filenames use the `-implementation.md` suffix (paired with `-design.md`).
- Multi-agent workflow removal policy is **delete** (remove skills/dirs and references, do not deprecate).
- A broad repo-wide `rg` sweep to remove/replace any leftover multi-agent references is explicitly approved.

## Assumptions (Locked)

- Base branch is `main` (fallback `master` if this repo uses it).
- All commands are run from the worktree root at `.worktrees/skill-network-lifecycle` unless explicitly stated.
- `rg` (ripgrep) is available; if not, replace `rg -n <pattern> <path>` with `grep -RIn <pattern> <path>`.

## Acceptance Checks (Process “Tests”)

These checks should fail before changes and pass after.

- `brainstorming` must require `using-git-worktrees` after design approval and before writing the plan.
- `using-git-worktrees` must always use project-local `.worktrees/` (no global option, no `/tmp`).
- `using-git-worktrees` must enforce naming: `.worktrees/<slug>` + `feat/<slug>`.
- `writing-plans` must hard-gate that planning happens in a worktree on `feat/*` unless explicit user consent.
- `writing-plans` must save implementation plans to `docs/plans/YYYY-MM-DD-<topic>-implementation.md` (paired with `...-design.md`).
- `writing-plans` must not offer multi-agent execution pathways; it must hand off to `executing-plans` only.
- `executing-plans` must hard-gate that execution happens in a worktree on non-main branch and must require `/review` checkpoints.
- `verification-before-completion` must explicitly bind into execution/finishing workflows.
- `finishing-a-development-branch` must stop for explicit human “reviewed” confirmation before merge/push/cleanup actions.
- Repo must not contain multi-agent workflow skills (or references) that compete with the single-path lifecycle.

---

## Preflight (One-Time)

Run from the *base* checkout (not the worktree yet):

- `cd /home/ubuntu/.codex/superpowers-codex`
- `git fetch origin`
- `git worktree list`
  - Expected: a line for `.worktrees/skill-network-lifecycle`
  - If missing, create it:
    - `mkdir -p .worktrees`
    - `git worktree add .worktrees/skill-network-lifecycle -b feat/skill-network-lifecycle`

Then enter the worktree and verify you’re in the right place:

- `cd /home/ubuntu/.codex/superpowers-codex/.worktrees/skill-network-lifecycle`
- `git rev-parse --show-toplevel`
  - Expected: ends with `.worktrees/skill-network-lifecycle`
- `git branch --show-current`
  - Expected: `feat/skill-network-lifecycle`
- `git status -sb`
  - Expected: clean working tree before starting Task 1

### Task 1: Update `brainstorming` bridge to worktrees

**Files:**
- Modify: `skills/brainstorming/SKILL.md`

**Step 1: Write a failing check (confirm old/incorrect constraint exists)**

Run: `rg -n "ONLY skill you invoke after brainstorming is writing-plans" skills/brainstorming/SKILL.md`
Expected: MATCH (current contradiction).

**Step 2: Edit `skills/brainstorming/SKILL.md` minimally**

Change requirements so that after design approval it MUST:
- invoke `superpowers:using-git-worktrees` (create `.worktrees/<slug>` + `feat/<slug>`)
- write/commit the design doc in the worktree
- then invoke `superpowers:writing-plans`

Update:
- Checklist items (insert a worktree step after approval)
- DOT graph nodes/edges (insert using-git-worktrees between approval and docs/plans)
- Replace “ONLY skill after” language with the new required bridge.

**Step 3: Re-run check (verify new behavior text exists)**

Run: `rg -n "using-git-worktrees" skills/brainstorming/SKILL.md`
Expected: MATCH (mandatory bridge is described).

Run: `rg -n "ONLY skill you invoke after brainstorming" skills/brainstorming/SKILL.md`
Expected: NO MATCH.

**Step 4: Commit**

Run:
- `git add skills/brainstorming/SKILL.md`
- `git commit -m "docs(skills): require worktree bridge after brainstorming"`

---

### Task 2: Make `using-git-worktrees` always project-local and enforce naming

**Files:**
- Modify: `skills/using-git-worktrees/SKILL.md`

**Step 1: Write failing checks (confirm global option + subagent references exist)**

Run: `rg -n "~/.config/superpowers/worktrees" skills/using-git-worktrees/SKILL.md`
Expected: MATCH (remove it).

Run: `rg -n "subagent-driven-development" skills/using-git-worktrees/SKILL.md`
Expected: MATCH (remove/replace it).

**Step 2: Edit `skills/using-git-worktrees/SKILL.md` minimally**

Required changes:
- Directory selection: always `.worktrees/` (create if missing); remove user choice and global location path.
- Safety verification: keep `git check-ignore` gate; clarify `.gitignore` fix on base branch is allowed.
- Naming:
  - Introduce `slug` concept.
  - Worktree path must be `.worktrees/<slug>`.
  - Branch must be `feat/<slug>`.
  - Command example should use: `git worktree add .worktrees/<slug> -b feat/<slug>`.
- Explicitly forbid `/tmp` and other non-repo worktree paths.
- Remove/replace any guidance that makes `subagent-driven-development` REQUIRED; execution should flow through `executing-plans` (with `/review` checkpoints) instead.

**Step 3: Re-run checks**

Run: `rg -n "~/.config/superpowers/worktrees" skills/using-git-worktrees/SKILL.md`
Expected: NO MATCH.

Run: `rg -n "\.worktrees/<slug>" skills/using-git-worktrees/SKILL.md`
Expected: MATCH.

Run: `rg -n "feat/<slug>" skills/using-git-worktrees/SKILL.md`
Expected: MATCH.

Run: `rg -n "subagent-driven-development" skills/using-git-worktrees/SKILL.md`
Expected: NO MATCH.

**Step 4: Commit**

Run:
- `git add skills/using-git-worktrees/SKILL.md`
- `git commit -m "docs(skills): enforce project-local worktrees and feat/* naming"`

---

### Task 3: Gate `writing-plans` to worktrees and remove multi-path handoff

**Files:**
- Modify: `skills/writing-plans/SKILL.md`

**Step 1: Write failing check (confirm missing gate)**

Run: `rg -n "must be inside a worktree" skills/writing-plans/SKILL.md`
Expected: NO MATCH (we will add it).

**Step 2: Edit `skills/writing-plans/SKILL.md` minimally**

Add a short pre-flight checklist near the top:
- Confirm current branch matches `feat/*` and repo is a worktree.
- If on `main/master`: STOP and invoke `superpowers:using-git-worktrees` unless user explicitly requests working on main.

Update execution handoff to a single pathway:
- Remove references to `subagent-driven-development` (and multi-path execution choices).
- Replace with: after saving the plan, **REQUIRED**: use `superpowers:executing-plans` to implement it (and rely on `/review` checkpoints inside that skill).

Update the naming guidance:
- Change “Save plans to” / examples to `docs/plans/YYYY-MM-DD-<topic>-implementation.md`.

Keep existing guidance about `docs/plans/...` location.

**Step 3: Re-run check**

Run: `rg -n -F "feat/*" skills/writing-plans/SKILL.md`
Expected: MATCH.

**Step 4: Commit**

Run:
- `git add skills/writing-plans/SKILL.md`
- `git commit -m "docs(skills): gate planning to worktree feat/* branches"`

---

### Task 4: Make `executing-plans` the single execution pathway (add /review checkpoints)

**Files:**
- Modify: `skills/executing-plans/SKILL.md`

**Step 0: Write failing check (confirm `/review` isn’t already required)**

Run: `rg -n "/review" skills/executing-plans/SKILL.md`
Expected: NO MATCH (we will add it).

**Step 1: Add Step 0 (pre-flight)**

Insert before Step 1:
- Verify not on `main/master`.
- Verify in a worktree (or explicitly note that worktree is required by default).
- If not: STOP and invoke `superpowers:using-git-worktrees`.

**Step 2: Add evidence requirement**

In Step 2/3 reporting, require:
- paste the exact verification command(s) from the plan
- paste the relevant output summary (pass/fail, exit code)

**Step 3: Bake in `/review` checkpoints**

Add explicit instructions to run Codex CLI `/review`:
- After each batch report (Step 3), run `/review` on the current changes and include the review summary.
- If review finds issues: fix, re-run verification, and (optionally) re-run `/review` to confirm.
- Before Step 5 (Complete Development), run `/review` one more time after final verification.

This bakes independent review into the default workflow using the CLI’s slash command (instead of multi-agent skill flows).

**Step 4: Commit**

Run:
- `git add skills/executing-plans/SKILL.md`
- `git commit -m "docs(skills): add worktree gates and verification evidence to executing-plans"`

---

### Task 5: Remove multi-agent workflow skills and repo-wide references

**Files:**
- Delete: `skills/subagent-driven-development/SKILL.md` (and its directory contents)
- Delete: `skills/dispatching-parallel-agents/SKILL.md` (and its directory contents)
- Modify: `skills/writing-plans/SKILL.md` (if any references remain after Task 3)
- Modify: `skills/executing-plans/SKILL.md` (ensure no references to subagents remain)
- Modify: `skills/finishing-a-development-branch/SKILL.md` (remove “Called by subagent-driven-development” references)
- Modify: `README.md` / `docs/README.codex.md` if they enumerate or recommend multi-agent workflows
- Modify/Delete: any `skills/writing-skills/*subagent*` references/files (see Task 6)

**Step 1: Write failing checks (locate all multi-agent references)**

Run:
- `rg -n "subagent|multi-agent|parallel agent|dispatch" skills docs README.md -S --glob '!docs/plans/**'`
Expected: MATCHES exist today (we will remove/replace).

**Step 2: Delete multi-agent workflow skills**

Prefer `git rm` so deletions are tracked cleanly:
- `git rm -r skills/subagent-driven-development skills/dispatching-parallel-agents`
Expected:
- `git status -sb` shows both directories removed.

**Step 3: Remove/replace references**

- Replace workflow guidance that previously relied on subagents with `/review` + `executing-plans` checkpoints.
- Ensure no skill references removed skills as REQUIRED.

**Step 4: Re-run checks**

Run:
- `rg -n "subagent|parallel agent|dispatch" skills docs README.md -S --glob '!docs/plans/**'`
Expected: NO MATCH.

**Step 5: Commit**

Run:
- `git add -A`
- `git commit -m "docs(skills): remove multi-agent workflow path; standardize on executing-plans + /review"`

---

### Task 6: Update `writing-skills` to avoid subagent dependence (use /review for pressure testing)

**Files:**
- Modify: `skills/writing-skills/SKILL.md`
- Modify/Delete: `skills/writing-skills/testing-skills-with-subagents.md` (and any other subagent-specific references)
- Modify: `skills/writing-skills/render-graphs.js` (remove subagent-driven-development examples from CLI usage/help output)

**Step 1: Write failing checks**

Run:
- `rg -n "subagent" skills/writing-skills -S`
Expected: MATCH (we will replace/remove).

**Step 2: Minimal edits**

- Replace “subagent pressure scenarios” with a Codex CLI-native loop:
  - Baseline: run scenario without the skill text and capture the failure mode (if possible).
  - Write/edit the skill.
  - Use `/review` and a second-pass self-check to find loopholes and rationalizations.
- Remove or rewrite `testing-skills-with-subagents.md` so it does not require multi-agent features.
- Update `render-graphs.js` usage examples so they do not reference removed skills.

**Step 3: Re-run checks**

Run:
- `rg -n "subagent" skills/writing-skills -S`
Expected: NO MATCH.

**Step 4: Commit**

Run:
- `git add skills/writing-skills`
- `git commit -m "docs(skills): update writing-skills to use /review instead of subagents"`

---

### Task 7: Add Integration bindings to `verification-before-completion`

**Files:**
- Modify: `skills/verification-before-completion/SKILL.md`

**Step 1: Add Integration section**

Add a concise section that states this skill’s gate applies to:
- `executing-plans` batch reporting and task completion (including `/review` checkpoint reporting)
- `finishing-a-development-branch` before any “ready to merge” or “tests pass” claims

**Step 2: Commit**

Run:
- `git add skills/verification-before-completion/SKILL.md`
- `git commit -m "docs(skills): bind verification-before-completion into executing-plans and finishing"`

---

### Task 8: Add hard human review gate + fix cleanup consistency in `finishing-a-development-branch`

**Files:**
- Modify: `skills/finishing-a-development-branch/SKILL.md`

**Step 0: Write failing checks**

Run: `rg -n "type.*reviewed" skills/finishing-a-development-branch/SKILL.md`
Expected: NO MATCH (we will add it).

Run: `rg -n -F "reviewed" skills/finishing-a-development-branch/SKILL.md`
Expected: NO MATCH (we will add a hard gate).

**Step 1: Add hard review gate**

After “tests pass” and before presenting options/executing:
- Print a short review checklist:
  - `git status`
  - `git log --oneline --decorate -n 20`
  - diff vs base branch (suggest: `git diff <base>...HEAD --stat` where `<base>` is `main` or `master`)
- Then STOP and ask user to type `reviewed` to proceed.

**Step 2: Make cleanup rules consistent**

Ensure Step 5 / quick reference table / “Common Mistakes” all agree:
- Never cleanup automatically without user choice.
- Never merge/push/delete without explicit selection.
- Keep discard confirmation as typed `discard`.

**Step 3: Commit**

Run:
- `git add skills/finishing-a-development-branch/SKILL.md`
- `git commit -m "docs(skills): add human review gate and align cleanup rules"`

---

### Task 9: Cross-skill consistency pass (minimal)

**Files:**
- Modify: only if contradictions remain.

**Step 1: Run consistency searches**

Run:
- `rg -n "ONLY skill you invoke after" skills`
- `rg -n "~/.config/superpowers/worktrees" skills`
- `rg -n "Never start implementation on main/master" skills`
- `rg -n "subagent|parallel agent|dispatch" skills docs README.md -S --glob '!docs/plans/**'`

Expected:
- No leftover “ONLY after brainstorming” contradictions.
- No global worktree path references.
- Main-branch warnings remain, but now have explicit STOP+bridge behavior in planning/execution.
- No multi-agent workflow references remain.

**Step 2: Optional doc touch-up (only if required)**

If you find a contradiction, fix it with the smallest possible edit and commit:
- `git commit -m "docs(skills): fix remaining lifecycle contradiction"`

---

## Final Verification

Run (in the worktree):
- `git status`
- `git log --oneline --decorate -n 20`
- `rg -n "\.worktrees/<slug>|feat/<slug>|reviewed" skills -S`
- `rg -n "subagent|parallel agent|dispatch" skills docs README.md -S --glob '!docs/plans/**'`

Expected:
- Clean working tree
- Commits for each task
- Updated skills contain the new required bridge + gates.
- No remaining multi-agent workflow references (outside `docs/plans/`).
