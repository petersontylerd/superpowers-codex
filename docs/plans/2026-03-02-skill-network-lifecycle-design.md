# Skill Network Lifecycle (Worktrees + Verification Gates) — Design

**Date:** 2026-03-02
**Status:** Approved

## Problem

Our skills cover important lifecycle phases, but they do not reliably “compose”:
- Skills sometimes contradict each other about required next steps.
- Worktree + branch isolation is inconsistent.
- Verification is often skipped (or reported without evidence), leading to missed requirements and errors.
- Cleanup/merge steps can happen without an explicit human review gate.

## Goals (Non-Negotiables)

1) **Always isolate work**: create a project-local worktree in `.worktrees/<slug>` and a branch `feat/<slug>` (never work on `main` by default).
2) **Always verify**: no “done/fixed/passing” claims without fresh, in-message verification evidence.
3) **Always stop for human review** before merge/push/cleanup actions; never merge or delete worktrees/branches autonomously.
4) **Minimal changes**: edit the smallest set of `SKILL.md` files needed to enforce the workflow.
5) **Consistency across skills**: the same invariants and stop conditions appear (or are referenced) everywhere they apply.

## Non-Goals

- Building an automated “skill linter” or test harness (out of scope for minimal-change pass).
- Changing Codex/tooling behavior beyond what skill instructions can reliably influence.
- Introducing a new “orchestrator” skill unless required (we will coordinate via cross-references instead).

## Key Decisions

- **Worktrees are always project-local**: `.worktrees/` inside the target repo (never global, never `/tmp`).
- **Naming convention is fixed**:
  - Worktree path: `.worktrees/<slug>`
  - Branch name: `feat/<slug>`
- If `.worktrees/` exists but is **not ignored**, `using-git-worktrees` may **fix `.gitignore` on the base branch and commit** (explicitly allowed), then proceed.
- `finishing-a-development-branch` includes a **hard human review gate**: after tests pass, it prints a review checklist and stops until the user explicitly confirms they reviewed.
- **Multi-agent removal policy:** multi-agent workflow skills and related repo references are removed by **deleting** them (not deprecating).
- **Repo-wide cleanup allowed:** a broad `rg`-driven sweep to remove/replace any leftover multi-agent references is explicitly approved.

## External Best-Practice Alignment (Summary)

We bias toward:
- **Clarity and explicit stop conditions** (avoid ambiguous “should” language).
- **Progressive disclosure** (keep SKILL.md scannable; link out or reference other skills for depth).
- **Evidence before claims** (verification outputs are required artifacts, not “nice to have”).
- **Small, enforceable steps** (reduce room for rationalization).

## Target Lifecycle Contract (“Skill Network”)

This is the default workflow that the skills must collectively enforce.

1) `using-superpowers`: load relevant skills before any action.
2) `brainstorming`: design-only; no implementation until design approved.
3) **After design approval**: `using-git-worktrees` (mandatory bridge)
   - Creates `.worktrees/<slug>` and branch `feat/<slug>`.
   - Verifies `.worktrees/` is ignored; fixes `.gitignore` if needed (allowed on base branch).
   - Verifies baseline state (project-appropriate tests) and stops to ask if baseline is not clean.
4) In the worktree: write + commit the design doc to `docs/plans/YYYY-MM-DD-<topic>-design.md`.
5) `writing-plans`: write + commit the implementation plan to `docs/plans/YYYY-MM-DD-<topic>-implementation.md`.
6) Execute the plan:
   - Same session: `subagent-driven-development`, or
   - Separate session: `executing-plans`
7) Verification discipline:
   - `test-driven-development` for behavior/bugfix changes.
   - `verification-before-completion` whenever claiming progress, marking tasks complete, committing, opening PRs, or declaring completion.
8) Completion: `finishing-a-development-branch`
   - Verify tests pass.
   - **Human review gate**: show diff/commits/status checklist; STOP until explicit “reviewed”.
   - Present options (merge, PR, keep, discard).
   - Execute only the explicitly chosen option; never auto-merge or auto-delete.

## Minimal SKILL.md Changes (Concrete Targets)

The implementation plan will apply minimal edits to these files:

1) `skills/brainstorming/SKILL.md`
   - After design approval: MUST invoke `using-git-worktrees`, then proceed to docs + `writing-plans`.
   - Update checklist + flowchart accordingly.

2) `skills/using-git-worktrees/SKILL.md`
   - Always project-local `.worktrees/` (create if missing); remove global `/tmp` options.
   - Fixed naming: `.worktrees/<slug>` + `feat/<slug>`.
   - Clarify `.gitignore` repair on base branch is allowed.

3) `skills/writing-plans/SKILL.md`
   - Add pre-flight: confirm we are in a worktree on `feat/*` (unless explicit user consent).
   - Ensure plan is saved/committed in the worktree repo.

4) `skills/executing-plans/SKILL.md`
   - Add Step 0: confirm worktree + non-main branch; otherwise STOP and invoke `using-git-worktrees`.
   - Require verification-before-completion evidence when reporting completion.

5) `skills/subagent-driven-development/SKILL.md`
   - Require implementer to provide exact verification commands + outputs per task.
   - Treat “no evidence” as “not complete” for reviewers.

6) `skills/verification-before-completion/SKILL.md`
   - Add an Integration section explicitly binding it to the execution + finishing skills.

7) `skills/finishing-a-development-branch/SKILL.md`
   - Add a hard human review gate.
   - Make cleanup rules consistent and require explicit user confirmation before any destructive action.

## Success Criteria

- Running a typical “build feature” conversation causes:
  - consistent worktree creation in `.worktrees/` + `feat/*` branch
  - no progress/completion claims without pasted verification evidence
  - an explicit human review stop before merge/push/cleanup
  - no autonomous merge or deletion actions

## Open Questions

None. All required preferences were resolved:
- Always project-local `.worktrees/` (never `/tmp`)
- Branch prefix `feat/`
- Worktree directory matches branch suffix
- `.gitignore` fix allowed on base branch
- Human review gate required before any finish/integration actions
- Multi-agent workflow removal policy is **delete**
- Broad repo-wide reference cleanup is approved
