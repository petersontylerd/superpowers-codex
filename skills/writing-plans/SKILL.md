---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, how to test it. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain. Assume they don't know good test design very well.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

## Pre-Flight (Hard Gate)

Before writing the plan:

- Confirm you're in a worktree (default requirement). If you're not: STOP and invoke `superpowers:using-git-worktrees`.
  - Quick check: in a worktree, `.git` is typically a file (not a directory).
- Confirm your current branch matches `feat/*`.
  - If you're on `main`/`master`: STOP and invoke `superpowers:using-git-worktrees` unless the user explicitly requests working on `main`/`master`.

**Save plans to:** `docs/plans/YYYY-MM-DD-<topic>-implementation.md` **in the target repository/worktree you are actively changing**.

**Paired design doc (recommended):** `docs/plans/YYYY-MM-DD-<topic>-design.md` (typically created during `superpowers:brainstorming`).

If `docs/plans/` does not exist in that repo, create it. Do **not** save project implementation plans into a shared skills library clone (for example `~/.codex/superpowers-codex`) unless the work is to modify that skills library repo itself.

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

**Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

**Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

**Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

**Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## Remember
- Exact file paths always
- Complete code in plan (not "add validation")
- Exact commands with expected output
- Reference relevant skills with @ syntax
- DRY, YAGNI, TDD, frequent commits

## Execution Handoff

After saving the plan, the next step is REQUIRED:

- Use `superpowers:executing-plans` to implement the plan task-by-task (with review checkpoints).
