# Skill Ecosystem Integrity Audit Remediations Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Fix all audited referential-integrity, seam, and context-poisoning deficits in `superpowers-codex`, and re-run the cross-file sweep to confirm no broken references remain.

**Architecture:** Apply minimal, surgical edits per deficit. After each change, run the narrowest verification command(s) that prove the fix and avoid regressions. Conclude with the audit plan’s Task 7 sweep commands.

**Tech Stack:** Git, ripgrep (`rg`), bash, Markdown

---

### Task 1: Fix `.codex/INSTALL.md` restart/update ambiguity (Deficit #1)

**Files:**
- Modify: `.codex/INSTALL.md`

**Step 1: Edit text to remove contradiction**

Update the “Updating” section to avoid implying that a restart is never needed. Keep one consistent instruction aligned with “restart required to discover skills at startup”.

**Step 2: Verify**

Run:
```bash
rg -n "Restart Codex|update instantly|symlink" .codex/INSTALL.md
```
Expected: no contradictory “instant update” claim that conflicts with restart guidance.

**Step 3: Commit**

```bash
git add .codex/INSTALL.md
git commit -m "docs(codex): clarify restart vs update behavior"
```

---

### Task 2: Fix audit design doc remediation-plan path (Deficit #2)

**Files:**
- Modify: `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-design.md`

**Step 1: Update remediation plan reference**

Replace the “Remediation Plan” section to reference:
- `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-remediations-implementation.md`

**Step 2: Verify**

Run:
```bash
rg -n "audit-remediations-implementation\\.md|skill-ecosystem-integrity-audit-implementation\\.md" docs/plans/2026-03-02-skill-ecosystem-integrity-audit-design.md
```
Expected: design doc references the remediation plan doc, not the audit execution plan doc.

**Step 3: Commit**

```bash
git add docs/plans/2026-03-02-skill-ecosystem-integrity-audit-design.md
git commit -m "docs(plans): fix remediation plan reference in audit design"
```

---

### Task 3: Fix redundant H1 in `auditing-writing-plans` (Deficit #3)

**Files:**
- Modify: `skills/auditing-writing-plans/SKILL.md`

**Step 1: Rename H1 only**

Change:
- `# Auditing Writing-Plans Plans`
To:
- `# Auditing Writing Plans`

**Step 2: Verify**

Run:
```bash
rg -n "^# " skills/auditing-writing-plans/SKILL.md | head
```
Expected: first heading is `# Auditing Writing Plans`.

**Step 3: Commit**

```bash
git add skills/auditing-writing-plans/SKILL.md
git commit -m "docs(skills): simplify auditing-writing-plans title"
```

---

### Task 4: Rewrite `brainstorming` YAML description to be trigger-only (Deficit #4)

**Files:**
- Modify: `skills/brainstorming/SKILL.md`

**Step 1: Update YAML `description:`**

Replace the existing description with a trigger-only version (no workflow summary), for example:
```yaml
description: Use when a user wants to build or change something and requirements or design are not yet clearly specified.
```

**Step 2: Verify**

Run:
```bash
rg -n "^description:" skills/brainstorming/SKILL.md
```
Expected: starts with `Use when` and does not describe steps (“explores”, “before implementation”, etc.).

**Step 3: Commit**

```bash
git add skills/brainstorming/SKILL.md
git commit -m "docs(skills): make brainstorming description trigger-only"
```

---

### Task 5: Rewrite `finishing-a-development-branch` YAML description to be trigger-only (Deficit #5)

**Files:**
- Modify: `skills/finishing-a-development-branch/SKILL.md`

**Step 1: Update YAML `description:`**

Replace the existing description with a trigger-only version, for example:
```yaml
description: Use when a feature branch is complete and you need to decide whether to merge, open a PR, keep, or discard it.
```

**Step 2: Verify**

Run:
```bash
rg -n "^description:" skills/finishing-a-development-branch/SKILL.md
```

**Step 3: Commit**

```bash
git add skills/finishing-a-development-branch/SKILL.md
git commit -m "docs(skills): make finishing description trigger-only"
```

---

### Task 6: Remove `CLAUDE.md` reference and fix GitHub reply guidance (Deficit #6)

**Files:**
- Modify: `skills/receiving-code-review/SKILL.md`

**Step 1: Replace “explicit CLAUDE.md violation” phrasing**

Change that bullet to repo-agnostic wording, for example:
- “(explicit instruction violation)” or “(explicit instruction-set violation)”

**Step 2: Fix the `gh api` endpoint or remove it**

Update the example to match GitHub’s “Create a reply for a review comment” endpoint:
- `gh api -X POST repos/{owner}/{repo}/pulls/{pull_number}/comments/{comment_id}/replies -f body='...'`

If you cannot confidently maintain this endpoint long-term, remove the endpoint and keep only the “reply in-thread, not top-level” guidance.

**Step 3: Verify**

Run:
```bash
rg -n "CLAUDE\\.md" skills/receiving-code-review/SKILL.md && exit 1 || true
rg -n "gh api" skills/receiving-code-review/SKILL.md || true
```
Expected: no `CLAUDE.md` mentions; any retained `gh api` example is coherent.

**Step 4: Commit**

```bash
git add skills/receiving-code-review/SKILL.md
git commit -m "docs(skills): make receiving-code-review codex-only and correct gh api example"
```

---

### Task 7: Fix broken plan path in requesting-code-review example (Deficit #7)

**Files:**
- Modify: `skills/requesting-code-review/SKILL.md`

**Step 1: Replace `docs/plans/deployment-plan.md` with a clearly hypothetical path**

Prefer one of:
- `docs/plans/<your-plan>.md`
- `docs/plans/YYYY-MM-DD-<topic>-implementation.md`

Also add a short note that it is an example path.

**Step 2: Verify**

Run:
```bash
rg -n "docs/plans/deployment-plan\\.md" skills/requesting-code-review/SKILL.md && exit 1 || true
```
Expected: no hits.

**Step 3: Commit**

```bash
git add skills/requesting-code-review/SKILL.md
git commit -m "docs(skills): fix requesting-code-review example plan path"
```

---

### Task 8: Rewrite `using-git-worktrees` YAML description to be trigger-only (Deficit #8)

**Files:**
- Modify: `skills/using-git-worktrees/SKILL.md`

**Step 1: Update YAML `description:`**

Replace with trigger-only wording, for example:
```yaml
description: Use when starting isolated work on a new branch or executing a plan and you need a clean git worktree.
```

**Step 2: Verify**

Run:
```bash
rg -n "^description:" skills/using-git-worktrees/SKILL.md
```

**Step 3: Commit**

```bash
git add skills/using-git-worktrees/SKILL.md
git commit -m "docs(skills): make using-git-worktrees description trigger-only"
```

---

### Task 9: Rewrite `using-superpowers` YAML description to be trigger-only (Deficit #9)

**Files:**
- Modify: `skills/using-superpowers/SKILL.md`

**Step 1: Update YAML `description:`**

Replace with trigger-only wording, for example:
```yaml
description: Use when starting a Codex session and you need to decide which skills apply to the user request.
```

**Step 2: Verify**

Run:
```bash
rg -n "^description:" skills/using-superpowers/SKILL.md
```

**Step 3: Commit**

```bash
git add skills/using-superpowers/SKILL.md
git commit -m "docs(skills): make using-superpowers description trigger-only"
```

---

### Task 10: Rewrite `verification-before-completion` YAML description to be trigger-only (Deficit #10)

**Files:**
- Modify: `skills/verification-before-completion/SKILL.md`

**Step 1: Update YAML `description:`**

Replace with trigger-only wording, for example:
```yaml
description: Use when about to make any completion or “tests pass” claim, especially before commits, pushes, or PRs.
```

**Step 2: Verify**

Run:
```bash
rg -n "^description:" skills/verification-before-completion/SKILL.md
```

**Step 3: Commit**

```bash
git add skills/verification-before-completion/SKILL.md
git commit -m "docs(skills): make verification-before-completion description trigger-only"
```

---

### Task 11: Make writing-skills “official guidance” references concrete (Deficit #11)

**Files:**
- Modify: `skills/writing-skills/SKILL.md`

**Step 1: Replace vague references with concrete URLs**

Update the “Official guidance” line to include stable references, for example:
- `https://developers.openai.com/codex/skills`
- `https://developers.openai.com/codex/skills#create-a-skill`
- `https://github.com/openai/skills`

**Step 2: Verify**

Run:
```bash
rg -n "developers\\.openai\\.com/codex/skills|github\\.com/openai/skills" skills/writing-skills/SKILL.md
```
Expected: at least one match for each intended reference.

**Step 3: Commit**

```bash
git add skills/writing-skills/SKILL.md
git commit -m "docs(skills): add concrete Codex skills guidance references"
```

---

### Task 12: Fix `docs/examples` implication in testing anti-patterns (Deficit #12)

**Files:**
- Modify: `skills/test-driven-development/testing-anti-patterns.md`

**Step 1: Rephrase “docs/examples”**

Change the guidance to be conditional and repo-agnostic, for example:
- “Examine an actual API response from your project’s docs/examples (if available), or from real API docs/examples.”

**Step 2: Verify**

Run:
```bash
rg -n "docs/examples" skills/test-driven-development/testing-anti-patterns.md && exit 1 || true
```

**Step 3: Commit**

```bash
git add skills/test-driven-development/testing-anti-patterns.md
git commit -m "docs(skills): make testing anti-patterns repo-agnostic"
```

---

### Task 13: Anchor persuasion-principles citations to reachable sources (Deficit #13)

**Files:**
- Modify: `skills/writing-skills/persuasion-principles.md`

**Step 1: Add concrete links/identifiers for cited works**

Add a link for:
- “Call Me A Jerk…” (Wharton Generative AI Labs research page)
- “Influence, New and Expanded” (publisher page)

If you want to keep numeric claims, ensure the linked source contains them; otherwise remove the numbers.

**Step 2: Verify**

Run:
```bash
rg -n "https?://" skills/writing-skills/persuasion-principles.md
```
Expected: at least two citations with URLs.

**Step 3: Commit**

```bash
git add skills/writing-skills/persuasion-principles.md
git commit -m "docs(skills): add sources for persuasion-principles citations"
```

---

### Task 14: Make condition-based waiting example self-contained (Deficit #14)

**Files:**
- Modify: `skills/systematic-debugging/condition-based-waiting-example.ts`

**Step 1: Remove repo-external imports**

Replace the `~/threads/...` imports by defining minimal types inline at top of file, for example:
```ts
export type LaceEventType = string;
export type LaceEvent = { type: LaceEventType; data?: unknown };
export interface ThreadManager {
  getEvents(threadId: string): LaceEvent[];
}
```

**Step 2: Verify**

Run:
```bash
rg -n "from '~/threads" skills/systematic-debugging/condition-based-waiting-example.ts && exit 1 || true
```
Expected: no hits.

**Step 3: Commit**

```bash
git add skills/systematic-debugging/condition-based-waiting-example.ts
git commit -m "docs(skills): make condition-based waiting example self-contained"
```

---

### Task 15: Fix `find-polluter.sh` pattern semantics and clarify constraints (Deficit #15)

**Files:**
- Modify: `skills/systematic-debugging/find-polluter.sh`

**Step 1: Make `src/**/*.test.ts` example actually work**

Prefer using bash `globstar` expansion instead of `find -path`. For example:
- `shopt -s globstar nullglob`
- Expand `$TEST_PATTERN` into an array of files.

**Step 2: Make test runner invocation more robust**

Prefer `npm test -- "$TEST_FILE"` and consider a fallback if needed.

**Step 3: Verify**

Run:
```bash
rg -n "globstar|nullglob|npm test --" skills/systematic-debugging/find-polluter.sh
```

**Step 4: Commit**

```bash
git add skills/systematic-debugging/find-polluter.sh
git commit -m "docs(skills): make find-polluter script globstar-friendly"
```

---

### Task 16: Wire verification-before-completion into execution workflow (Deficit #16)

**Files:**
- Modify: `skills/executing-plans/SKILL.md`

**Step 1: Add explicit required sub-skill**

In Step 3 (“Report”), add:
- `**REQUIRED SUB-SKILL:** Use superpowers:verification-before-completion`

Place it directly before completion/pass claims guidance.

**Step 2: Verify**

Run:
```bash
rg -n "verification-before-completion" skills/executing-plans/SKILL.md
```
Expected: at least one match.

**Step 3: Commit**

```bash
git add skills/executing-plans/SKILL.md
git commit -m "docs(skills): wire verification-before-completion into executing-plans"
```

---

### Task 17: Final integrity sweep (must be green)

**Files:**
- Modify: `docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md`

**Step 1: Re-run the audit plan’s Task 7 sweep commands**

Run:
```bash
rg -n "docs/plans/|docs/reports/|skills/|\\.codex/|@[A-Za-z0-9_./-]+\\.md" -S .
rg -n "\\bopencode\\b|\\bcursor\\b|claude-plugin|dispatching-parallel-agents|subagent-driven-development" -S . --glob '!docs/plans/*' || true
```

**Step 2: Verify previously-broken references are gone**

Run:
```bash
rg -n "docs/plans/deployment-plan\\.md" -S . && exit 1 || true
rg -n "docs/examples" -S . && exit 1 || true
```

**Step 3: Update the audit report status**

Add a short “Remediation status” section noting which deficits were fixed and in which commit(s).

**Step 4: Commit**

```bash
git add docs/reports/2026-03-02-skill-ecosystem-integrity-audit.md
git commit -m "docs(audit): record remediation status"
```

