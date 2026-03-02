# Skill Network Lifecycle (Worktrees + Verification Gates) Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Make the existing skills compose into a consistent lifecycle: `brainstorming → using-git-worktrees → writing-plans → (executing-plans|subagent-driven-development) → verification-before-completion → finishing-a-development-branch`, with a hard human-review gate before merge/push/cleanup.

**Architecture:** Minimal edits to existing `SKILL.md` files to remove contradictions and add explicit required bridges + stop conditions. No new orchestrator skill.

**Tech Stack:** Markdown (`SKILL.md`), git worktrees/branches.

---

## Global Constraints

- All edits happen on branch `feat/skill-network-lifecycle` in worktree `.worktrees/skill-network-lifecycle`.
- Keep changes minimal and localized to the targeted skills.
- Do not introduce new skills or scripts unless strictly required.
- Preserve existing tone/style where possible; prefer small insertions over rewrites.

## Acceptance Checks (Process “Tests”)

These checks should fail before changes and pass after.

- `brainstorming` must require `using-git-worktrees` after design approval and before writing the plan.
- `using-git-worktrees` must always use project-local `.worktrees/` (no global option, no `/tmp`).
- `using-git-worktrees` must enforce naming: `.worktrees/<slug>` + `feat/<slug>`.
- `writing-plans` must hard-gate that planning happens in a worktree on `feat/*` unless explicit user consent.
- `executing-plans` must hard-gate that execution happens in a worktree on non-main branch.
- `subagent-driven-development` must require verification evidence per task.
- `verification-before-completion` must explicitly bind into execution/finishing workflows.
- `finishing-a-development-branch` must stop for explicit human “reviewed” confirmation before merge/push/cleanup actions.

---

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

**Step 1: Write failing checks (confirm global option exists)**

Run: `rg -n "~/.config/superpowers/worktrees" skills/using-git-worktrees/SKILL.md`
Expected: MATCH (remove it).

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

**Step 3: Re-run checks**

Run: `rg -n "~/.config/superpowers/worktrees" skills/using-git-worktrees/SKILL.md`
Expected: NO MATCH.

Run: `rg -n "\.worktrees/<slug>" skills/using-git-worktrees/SKILL.md`
Expected: MATCH.

Run: `rg -n "feat/<slug>" skills/using-git-worktrees/SKILL.md`
Expected: MATCH.

**Step 4: Commit**

Run:
- `git add skills/using-git-worktrees/SKILL.md`
- `git commit -m "docs(skills): enforce project-local worktrees and feat/* naming"`

---

### Task 3: Add a worktree/branch pre-flight gate to `writing-plans`

**Files:**
- Modify: `skills/writing-plans/SKILL.md`

**Step 1: Write failing check (confirm missing gate)**

Run: `rg -n "must be inside a worktree" skills/writing-plans/SKILL.md`
Expected: NO MATCH (we will add it).

**Step 2: Edit `skills/writing-plans/SKILL.md` minimally**

Add a short pre-flight checklist near the top:
- Confirm current branch matches `feat/*` and repo is a worktree.
- If on `main/master`: STOP and invoke `superpowers:using-git-worktrees` unless user explicitly requests working on main.

Keep existing guidance about `docs/plans/...` location.

**Step 3: Re-run check**

Run: `rg -n "feat/\*" skills/writing-plans/SKILL.md`
Expected: MATCH.

**Step 4: Commit**

Run:
- `git add skills/writing-plans/SKILL.md`
- `git commit -m "docs(skills): gate planning to worktree feat/* branches"`

---

### Task 4: Add execution pre-flight gates and evidence requirements to `executing-plans`

**Files:**
- Modify: `skills/executing-plans/SKILL.md`

**Step 1: Add Step 0 (pre-flight)**

Insert before Step 1:
- Verify not on `main/master`.
- Verify in a worktree (or explicitly note that worktree is required by default).
- If not: STOP and invoke `superpowers:using-git-worktrees`.

**Step 2: Add evidence requirement**

In Step 2/3 reporting, require:
- paste the exact verification command(s) from the plan
- paste the relevant output summary (pass/fail, exit code)

**Step 3: Commit**

Run:
- `git add skills/executing-plans/SKILL.md`
- `git commit -m "docs(skills): add worktree gates and verification evidence to executing-plans"`

---

### Task 5: Tighten evidence contract in `subagent-driven-development`

**Files:**
- Modify: `skills/subagent-driven-development/SKILL.md`

**Step 1: Minimal additions**

Add explicit requirements:
- Implementer must report exact verification commands run + outputs.
- Spec reviewer treats missing evidence as not complete.
- Code quality reviewer should also check evidence exists.

**Step 2: Commit**

Run:
- `git add skills/subagent-driven-development/SKILL.md`
- `git commit -m "docs(skills): require verification evidence per task in subagent-driven-development"`

---

### Task 6: Add Integration bindings to `verification-before-completion`

**Files:**
- Modify: `skills/verification-before-completion/SKILL.md`

**Step 1: Add Integration section**

Add a concise section that states this skill’s gate applies to:
- `executing-plans` reporting and task completion
- `subagent-driven-development` per-task completion
- `finishing-a-development-branch` before any “ready to merge” or “tests pass” claims

**Step 2: Commit**

Run:
- `git add skills/verification-before-completion/SKILL.md`
- `git commit -m "docs(skills): bind verification-before-completion into execution and finishing workflows"`

---

### Task 7: Add hard human review gate + fix cleanup consistency in `finishing-a-development-branch`

**Files:**
- Modify: `skills/finishing-a-development-branch/SKILL.md`

**Step 1: Add hard review gate**

After “tests pass” and before presenting options/executing:
- Print a short review checklist:
  - `git status`
  - `git log --oneline --decorate -n 20`
  - diff vs base branch (suggest: `git diff <base>...HEAD --stat`)
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

### Task 8: Cross-skill consistency pass (minimal)

**Files:**
- Modify: only if contradictions remain.

**Step 1: Run consistency searches**

Run:
- `rg -n "ONLY skill you invoke after" skills`
- `rg -n "~/.config/superpowers/worktrees" skills`
- `rg -n "Never start implementation on main/master" skills`

Expected:
- No leftover “ONLY after brainstorming” contradictions.
- No global worktree path references.
- Main-branch warnings remain, but now have explicit STOP+bridge behavior in planning/execution.

**Step 2: Optional doc touch-up (only if required)**

If you find a contradiction, fix it with the smallest possible edit and commit:
- `git commit -m "docs(skills): fix remaining lifecycle contradiction"`

---

## Final Verification

Run (in the worktree):
- `git status`
- `git log --oneline --decorate -n 20`
- `rg -n "\.worktrees/<slug>|feat/<slug>|reviewed" skills -S`

Expected:
- Clean working tree
- Commits for each task
- Updated skills contain the new required bridge + gates.
