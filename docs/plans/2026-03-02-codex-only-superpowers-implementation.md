# Codex-Only Superpowers Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Remove all non-Codex platform artifacts from `~/.codex/superpowers` while preserving Codex native skill discovery and usage.

**Architecture:** Keep `skills/` as the only runtime-critical tree. Retain only Codex install/docs. Delete Claude/Cursor/OpenCode packaging, hooks, commands, tests, and non-Codex docs. Update skill wording so design/plan docs are saved in the *target repo/worktree being built* (not “the skills repo” by default).

**Tech Stack:** Git, Markdown, Codex skills discovery (`~/.agents/skills`)

---

### Task 1: Create an isolated work branch/worktree

**Files:**
- Modify: none

**Step 1: Create branch**

Run: `git checkout -b chore/codex-only`
Expected: switched to new branch.

**Step 2: Confirm clean baseline**

Run: `git status -sb`
Expected: `## chore/codex-only` and no local changes.

**Step 3: Commit**

No commit in this task.

---

### Task 2: Verify Codex install invariants before changes

**Files:**
- Modify: none

**Step 1: Confirm skills symlink resolves**

Run: `readlink -f ~/.agents/skills/superpowers`
Expected: path ending in `/.codex/superpowers/skills`.

**Step 2: Confirm skills exist**

Run: `find skills -maxdepth 2 -name SKILL.md -print | wc -l`
Expected: non-zero count.

**Step 3: Commit**

No commit in this task.

---

### Task 3: Prove `skills/` does not reference soon-to-be-deleted paths

**Files:**
- Modify: none

**Step 1: Ripgrep for references**

Run:
- `rg -n "\\.opencode/|\\.claude-plugin/|\\.cursor-plugin/|^hooks/|\\bhooks\\b|\\blib/|\\bcommands/|\\btests/|CLAUDE_PLUGIN_ROOT|opencode|cursor" skills`

Expected: either no hits, or hits that are clearly narrative-only and can be rewritten without changing behavior.

**Step 2: If hits exist, capture them**

Run: `rg -n "\\.opencode/|\\.claude-plugin/|\\.cursor-plugin/|hooks|\\blib/|\\bcommands/|\\btests/|CLAUDE_PLUGIN_ROOT|opencode|cursor" skills`
Expected: output lines identifying what must be rewritten before deletion.

**Step 3: Commit**

No commit in this task.

---

### Task 4: Delete non-Codex platform directories and files

**Files:**
- Delete: `.claude-plugin/`
- Delete: `.cursor-plugin/`
- Delete: `.opencode/`
- Delete: `hooks/`
- Delete: `commands/`
- Delete: `lib/`
- Delete: `tests/`
- Delete: `agents/`
- Delete: `RELEASE-NOTES.md`
- Delete: `docs/README.opencode.md`
- Delete: `docs/testing.md`
- Delete: `docs/windows/`

**Step 1: Delete directories/files**

Run:
`rm -rf .claude-plugin .cursor-plugin .opencode hooks commands lib tests agents docs/windows`

Run:
`rm -f RELEASE-NOTES.md docs/README.opencode.md docs/testing.md`

Expected: files/directories removed.

**Step 2: Confirm they’re gone**

Run:
- `test ! -e .claude-plugin && test ! -e .cursor-plugin && test ! -e .opencode && test ! -e hooks && test ! -e commands && test ! -e lib && test ! -e tests && test ! -e agents`
Expected: exit code 0.

**Step 3: Commit**

Run:
`git add -A && git commit -m "chore: remove non-codex platform artifacts"`

Expected: commit created.

---

### Task 5: Replace `docs/plans/` contents with Codex-only artifacts

**Files:**
- Delete: `docs/plans/2025-11-22-opencode-support-design.md`
- Delete: `docs/plans/2025-11-22-opencode-support-implementation.md`
- Delete: `docs/plans/2025-11-28-skills-improvements-from-user-feedback.md`
- Keep: `docs/plans/2026-03-02-codex-only-superpowers-design.md`
- Keep/Create: `docs/plans/2026-03-02-codex-only-superpowers-implementation.md` (this file)

**Step 1: Delete old plan docs**

Run:
`rm -f docs/plans/2025-11-22-opencode-support-design.md docs/plans/2025-11-22-opencode-support-implementation.md docs/plans/2025-11-28-skills-improvements-from-user-feedback.md`

Expected: only Codex-only plan docs remain in `docs/plans/`.

**Step 2: Confirm directory contents**

Run: `ls -la docs/plans`
Expected: only `2026-03-02-codex-only-superpowers-design.md` and `2026-03-02-codex-only-superpowers-implementation.md` (and `.` / `..`).

**Step 3: Commit**

Run:
`git add -A && git commit -m "docs: keep only codex-only plans"`

Expected: commit created.

---

### Task 6: Rewrite `README.md` to be Codex-only

**Files:**
- Modify: `README.md`

**Step 1: Replace installation/usage sections**

Edit `README.md` so it only documents Codex:
- What superpowers is (skills library)
- Codex install: clone to `~/.codex/superpowers`, symlink `~/.agents/skills/superpowers -> ~/.codex/superpowers/skills`, restart Codex
- How skill discovery works at a high level (brief, Codex-focused)
- Remove Claude Code / Cursor / OpenCode sections and links

**Step 2: Verify no non-Codex mentions remain**

Run:
`rg -n "Claude Code|Cursor|OpenCode|opencode|\\.claude-plugin|\\.cursor-plugin|\\.opencode|hooks\\.json|CLAUDE_PLUGIN_ROOT" README.md`
Expected: no matches.

**Step 3: Commit**

Run:
`git add README.md && git commit -m "docs: make README codex-only"`

Expected: commit created.

---

### Task 7: Update skill wording for plan/design doc location (target repo/worktree)

**Files:**
- Modify: `skills/brainstorming/SKILL.md`
- Modify: `skills/writing-plans/SKILL.md`

**Step 1: Update `skills/brainstorming/SKILL.md`**

Change the “Write design doc” instruction to explicitly mean:
- Save to `docs/plans/YYYY-MM-DD-<topic>-design.md` **in the target repo/worktree being built**.
- Create `docs/plans/` if missing.
- Only write into the `~/.codex/superpowers` repo when the user is actually changing this repo.

**Step 2: Update `skills/writing-plans/SKILL.md`**

Change:
- “Save plans to: `docs/plans/...`”
To clarify the same “target repo/worktree” scope.

**Step 3: Commit**

Run:
`git add skills/brainstorming/SKILL.md skills/writing-plans/SKILL.md && git commit -m "docs(skills): clarify plans saved in target repo"`

Expected: commit created.

---

### Task 8: Post-change repo hygiene checks + Codex smoke test

**Files:**
- Modify: none

**Step 1: Ensure deleted paths aren’t referenced anywhere**

Run:
`rg -n "opencode|OpenCode|\\.opencode|CLAUDE_PLUGIN_ROOT|\\.claude-plugin|Claude Code|\\.cursor-plugin|Cursor|hooks\\.json|hooks/" .`

Expected: no matches (or only matches in historical git data excluded from working tree).

**Step 2: Confirm `skills/` still intact**

Run:
- `find skills -maxdepth 2 -name SKILL.md -print | sort`
Expected: list of skills unchanged.

**Step 3: Manual Codex smoke test**

Restart Codex, then:
- Invoke `using-superpowers` automatically (fresh session should do this).
- Explicitly request one other skill by name (e.g., `brainstorming`) and confirm it loads.

**Step 4: Commit**

No commit in this task.

