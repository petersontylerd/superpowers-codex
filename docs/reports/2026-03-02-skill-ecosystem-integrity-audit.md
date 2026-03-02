# Skill Ecosystem Integrity Audit (Manual-First)

**Date:** 2026-03-02  
**Audited commit:** 2ad0f51d0f6771a510b4e4cbddc60ba7cbf939c3  
**Worktree:** `.worktrees/skill-ecosystem-integrity-audit`

## Inventory (review order)

- [x] `README.md`
- [x] `.codex/INSTALL.md`
- [x] `docs/plans/.gitkeep`
- [x] `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-design.md`
- [x] `skills/auditing-plan-execution/SKILL.md`
- [x] `skills/auditing-writing-plans/SKILL.md`
- [x] `skills/brainstorming/SKILL.md`
- [x] `skills/executing-plans/SKILL.md`
- [x] `skills/finishing-a-development-branch/SKILL.md`
- [x] `skills/receiving-code-review/SKILL.md`
- [x] `skills/requesting-code-review/SKILL.md`
- [ ] `skills/requesting-code-review/code-reviewer.md`
- [x] `skills/systematic-debugging/SKILL.md`
- [ ] `skills/systematic-debugging/condition-based-waiting.md`
- [ ] `skills/systematic-debugging/defense-in-depth.md`
- [ ] `skills/systematic-debugging/root-cause-tracing.md`
- [x] `skills/test-driven-development/SKILL.md`
- [ ] `skills/test-driven-development/testing-anti-patterns.md`
- [x] `skills/using-git-worktrees/SKILL.md`
- [x] `skills/using-superpowers/SKILL.md`
- [x] `skills/verification-before-completion/SKILL.md`
- [ ] `skills/writing-plans/SKILL.md`
- [ ] `skills/writing-skills/SKILL.md`
- [ ] `skills/writing-skills/persuasion-principles.md`
- [ ] `skills/writing-skills/testing-skills-under-pressure.md`
- [ ] `skills/writing-skills/graphviz-conventions.dot`
- [ ] `skills/writing-skills/render-graphs.js`

## Evidence (completed checks)

### `README.md`

Commands run:
```bash
rg -n 'superpowers:|skills/|docs/|\\.codex/' README.md
```

Result: 3 matches (expected). Referenced files checked:
- `.codex/INSTALL.md` (exists)
- `skills/writing-skills/SKILL.md` (exists)
- `LICENSE` (exists)

Deficits: none found.

### `.codex/INSTALL.md`

Commands run:
```bash
ls -la /home/ubuntu/.agents/skills/superpowers-codex
```

Result: symlink points to `/home/ubuntu/.codex/superpowers-codex/skills` (expected).

### `docs/plans/*`

Commands run:
```bash
find docs/plans -maxdepth 1 -type f -print | sort
```

Result:
- `docs/plans/.gitkeep`
- `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-design.md`
- `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-implementation.md`

Deficits: see Deficit #2 (design doc references remediation plan path incorrectly).

### `skills/auditing-plan-execution/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/auditing-plan-execution/SKILL.md
test -f skills/test-driven-development/SKILL.md
test -f skills/systematic-debugging/SKILL.md
rg -n '^name:|^description:' skills/auditing-plan-execution/SKILL.md
```

Result:
- YAML frontmatter present with `name: auditing-plan-execution` and `description:` starting with “Use when …”.
- Outbound references: only `superpowers:test-driven-development` and `superpowers:systematic-debugging` (both exist as skills).
- External links: none.

Deficits: none found.

### `skills/auditing-writing-plans/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/auditing-writing-plans/SKILL.md
test -f skills/writing-plans/SKILL.md
test -f skills/executing-plans/SKILL.md
rg -n '^name:|^description:' skills/auditing-writing-plans/SKILL.md
rg -n '^# ' skills/auditing-writing-plans/SKILL.md | head
```

Result:
- YAML frontmatter present with `name: auditing-writing-plans` and `description:` starting with “Use when …”.
- Outbound references: `superpowers:writing-plans` and `superpowers:executing-plans` (both exist as skills).
- External links: none.

Deficits: see Deficit #3 (skill title is redundant/confusing).

### `skills/brainstorming/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/brainstorming/SKILL.md
rg -n '^name:|^description:' skills/brainstorming/SKILL.md
test -f skills/using-git-worktrees/SKILL.md
test -f skills/writing-plans/SKILL.md
```

Result:
- YAML frontmatter present with `name: brainstorming`.
- Outbound references: `.worktrees/<slug>`, `docs/plans/...-design.md`, and mentions of `using-git-worktrees` / `writing-plans` (both exist as skills).
- External links: none.

Deficits: see Deficit #4 (description field is workflow-heavy and not strictly “Use when …” triggers).

### `skills/executing-plans/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/executing-plans/SKILL.md
rg -n '^name:|^description:' skills/executing-plans/SKILL.md
test -f skills/using-git-worktrees/SKILL.md
test -f skills/writing-plans/SKILL.md
test -f skills/finishing-a-development-branch/SKILL.md
rg -n 'Create TodoWrite' skills/executing-plans/SKILL.md
```

Result:
- YAML frontmatter present with `name: executing-plans` and `description:` starting with “Use when …”.
- Outbound references: `superpowers:using-git-worktrees`, `superpowers:writing-plans`, `superpowers:finishing-a-development-branch` (all exist as skills).
- Mentions `/review` (CLI-specific) but does not link to external docs.

Deficits: none found.

### `skills/finishing-a-development-branch/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/finishing-a-development-branch/SKILL.md
rg -n '^name:|^description:' skills/finishing-a-development-branch/SKILL.md
test -f skills/executing-plans/SKILL.md
test -f skills/using-git-worktrees/SKILL.md
```

Result:
- YAML frontmatter present with `name: finishing-a-development-branch`.
- Outbound references: only `superpowers:using-git-worktrees` (as plain text) in the pre-flight failure instruction; and “executing-plans” in Integration (no `superpowers:` prefix).
- External links: none.

Deficits: see Deficit #5 (description includes workflow summary and is not strictly “Use when …” triggers).

### `skills/receiving-code-review/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/receiving-code-review/SKILL.md
rg -n '^name:|^description:' skills/receiving-code-review/SKILL.md
rg -n 'CLAUDE\\.md' skills/receiving-code-review/SKILL.md
rg -n 'gh api|gh pr' skills/receiving-code-review/SKILL.md
```

Result:
- YAML frontmatter present with `name: receiving-code-review` and `description:` starting with “Use when …”.
- Outbound references include a `CLAUDE.md` mention (conceptual) and a GitHub CLI example command for replying to PR comments.
- External links: none.

Deficits: see Deficit #6 (mentions `CLAUDE.md` even though this repo is Codex-only; plus `gh api` command is likely incorrect for that endpoint).

### `skills/requesting-code-review/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/requesting-code-review/SKILL.md
rg -n '^name:|^description:' skills/requesting-code-review/SKILL.md
test -f skills/requesting-code-review/code-reviewer.md
test -f docs/plans/deployment-plan.md || echo 'docs/plans/deployment-plan.md missing (example?)'
```

Result:
- YAML frontmatter present with `name: requesting-code-review` and `description:` starting with “Use when …”.
- Local reference: `requesting-code-review/code-reviewer.md` exists.
- Example section references `docs/plans/deployment-plan.md`, which does not exist in this repo.

Deficits: see Deficit #7 (broken plan-path reference in the example).

### `skills/systematic-debugging/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/systematic-debugging/SKILL.md
rg -n '^name:|^description:' skills/systematic-debugging/SKILL.md
test -f skills/systematic-debugging/root-cause-tracing.md
test -f skills/systematic-debugging/defense-in-depth.md
test -f skills/systematic-debugging/condition-based-waiting.md
test -f skills/test-driven-development/SKILL.md
test -f skills/verification-before-completion/SKILL.md
```

Result:
- YAML frontmatter present with `name: systematic-debugging` and `description:` starting with “Use when …”.
- Local references to `root-cause-tracing.md`, `defense-in-depth.md`, and `condition-based-waiting.md` resolve and exist.
- Related skills referenced: `superpowers:test-driven-development` and `superpowers:verification-before-completion` (both exist).
- External links: none.

Deficits: none found.

### `skills/test-driven-development/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/test-driven-development/SKILL.md
rg -n '^name:|^description:' skills/test-driven-development/SKILL.md
test -f skills/test-driven-development/testing-anti-patterns.md
```

Result:
- YAML frontmatter present with `name: test-driven-development` and `description:` starting with “Use when …”.
- Local `@testing-anti-patterns.md` reference resolves and exists.
- External links: none.

Deficits: none found.

### `skills/using-git-worktrees/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/using-git-worktrees/SKILL.md
rg -n '^name:|^description:' skills/using-git-worktrees/SKILL.md
```

Result:
- YAML frontmatter present with `name: using-git-worktrees`.
- Outbound references are repo-local paths (e.g., `.worktrees/<slug>`) and standard git commands; no external links.

Deficits: see Deficit #8 (description includes workflow summary and is not strictly “Use when …” triggers).

### `skills/using-superpowers/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/using-superpowers/SKILL.md
rg -n '^name:|^description:' skills/using-superpowers/SKILL.md
rg -n 'PlanMode|TodoWrite|Create TodoWrite' skills/using-superpowers/SKILL.md
```

Result:
- YAML frontmatter present with `name: using-superpowers`.
- Outbound references are conceptual (skill discovery paths, “PlanMode”, “TodoWrite” as a mechanism name); no repo-local file paths or external links.

Deficits: see Deficit #9 (description includes workflow summary and is not strictly “Use when …” triggers).

### `skills/verification-before-completion/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/verification-before-completion/SKILL.md
rg -n '^name:|^description:' skills/verification-before-completion/SKILL.md
test -f skills/executing-plans/SKILL.md
test -f skills/finishing-a-development-branch/SKILL.md
rg -n 'superpowers:executing-plans|superpowers:finishing-a-development-branch' skills/verification-before-completion/SKILL.md
```

Result:
- YAML frontmatter present with `name: verification-before-completion`.
- Outbound references: `superpowers:executing-plans` and `superpowers:finishing-a-development-branch` (both exist as skills).
- External links: none.

Deficits: see Deficit #10 (description includes workflow summary and is not strictly “Use when …” triggers).

## Deficits

| ID | Severity | File | Evidence | Why it matters | Proposed fix | Verification |
|---:|:--|:--|:--|:--|:--|:--|
| 1 | Medium | `.codex/INSTALL.md` | “Skills update instantly through the symlink.” | This conflicts with earlier “Restart Codex” guidance and can cause confusion about when a restart is required after updates. | Clarify update behavior (for example: restart required to discover new skills; restart recommended after updates to ensure reload). | Re-read `.codex/INSTALL.md` for a single, non-contradictory instruction; ensure it matches current repo guidance. |
| 2 | Medium | `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-design.md` | Remediation section says “After the audit report is reviewed, create: `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-implementation.md`.” | This is misleading because that file already exists and is the audit execution plan; it conflates “audit plan” with “remediation plan”, risking incorrect next steps. | Update the design doc to reference the remediation plan doc (`docs/plans/2026-03-02-skill-ecosystem-integrity-audit-remediations-implementation.md`) and clarify intent. | Ensure the design doc’s referenced remediation plan path exists and matches the plan’s Task 9 output path. |
| 3 | Low | `skills/auditing-writing-plans/SKILL.md` | H1 is `# Auditing Writing-Plans Plans` | The title is redundant and slightly confusing; it adds friction for humans and risks being repeated verbatim in future references. | Rename H1 to a single, clear name (for example: `# Auditing Writing Plans`). | Re-read the file and confirm the H1 is clear and consistent with the skill name. |
| 4 | Medium | `skills/brainstorming/SKILL.md` | YAML description is: `\"You MUST use this before any creative work - creating features, building components, adding functionality, or modifying behavior. Explores user intent, requirements and design before implementation.\"` | The description includes workflow summary (“Explores … before implementation”) rather than strictly trigger conditions, increasing the chance an agent shortcuts by following metadata instead of reading the skill (CSO trap). | Rewrite description to only state triggering conditions (start with “Use when …”), with no workflow/process summary. | Confirm `description:` remains under ~500 chars, starts with “Use when”, and does not describe steps. |
| 5 | Medium | `skills/finishing-a-development-branch/SKILL.md` | YAML description ends with “guides completion of development work by presenting structured options for merge, PR, or cleanup” | The description includes workflow summary rather than strictly triggers; this risks metadata-shortcut behavior and bloats the most frequently loaded prompt metadata. | Rewrite description to only state triggering conditions (start with “Use when …”), with no workflow/process summary. | Confirm `description:` starts with “Use when”, stays under ~500 chars, and does not describe steps. |
| 6 | Medium | `skills/receiving-code-review/SKILL.md` | “You’re absolutely right!” is labeled an explicit `CLAUDE.md` violation; and it instructs `gh api repos/{owner}/{repo}/pulls/{pr}/comments/{id}/replies`. | Repo intent is Codex-only; `CLAUDE.md` reference is ecosystem-noise. The `gh api` endpoint is likely wrong for “reply to a PR review comment” and may mislead users. | Replace `CLAUDE.md` mention with a repo-agnostic “explicit instruction violation” phrasing. Replace the `gh api` example with either a correct endpoint (with a note to confirm via `gh api --help`) or remove the endpoint and instruct “reply in-thread via GitHub UI / correct API; avoid top-level comment”. | Validate the file has no `CLAUDE.md` mentions; validate any retained `gh api` example is accurate (or is clearly marked as an example + verification step). |
| 7 | Medium | `skills/requesting-code-review/SKILL.md` | Example includes `PLAN_OR_REQUIREMENTS: Task 2 from docs/plans/deployment-plan.md` | This path does not exist in this repo; it reads like a real dependency and breaks referential integrity for readers and for any automated checks. | Change to a clearly hypothetical example path (e.g. `docs/plans/<your-plan>.md`) or reference a file that exists in-repo. | Ensure the example no longer references `docs/plans/deployment-plan.md`, or add an explicit “example path” disclaimer. |
| 8 | Medium | `skills/using-git-worktrees/SKILL.md` | YAML description ends with “creates isolated git worktrees with smart directory selection and safety verification” | The description includes workflow summary rather than strictly triggers; this risks metadata-shortcut behavior and bloats prompt metadata for a frequently invoked workflow skill. | Rewrite description to only state triggering conditions (start with “Use when …”), with no workflow/process summary. | Confirm `description:` starts with “Use when”, stays under ~500 chars, and does not describe steps. |
| 9 | Medium | `skills/using-superpowers/SKILL.md` | YAML description includes “establishes how to find and use skills, requiring relevant skill loading before ANY response…” | The description includes workflow/behavioral instruction (“requiring … before ANY response”) rather than strictly triggers; this risks metadata-shortcut behavior and increases context cost. | Rewrite description to only state triggering conditions (start with “Use when …”), with no workflow/process summary. | Confirm `description:` starts with “Use when”, stays under ~500 chars, and does not describe steps. |
| 10 | Medium | `skills/verification-before-completion/SKILL.md` | YAML description includes “requires running verification commands and confirming output…” | The description includes workflow summary rather than strictly trigger conditions; this risks metadata-shortcut behavior and increases context cost. | Rewrite description to only state triggering conditions (start with “Use when …”), with no workflow/process summary. | Confirm `description:` starts with “Use when”, stays under ~500 chars, and does not describe steps. |

## Context-Poisoning Candidates

None identified yet (only `README.md` and `.codex/INSTALL.md` reviewed).

## Seam Checks (workflow bridges)

Not reviewed yet.
