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
- [ ] `skills/receiving-code-review/SKILL.md`
- [ ] `skills/requesting-code-review/SKILL.md`
- [ ] `skills/requesting-code-review/code-reviewer.md`
- [ ] `skills/systematic-debugging/SKILL.md`
- [ ] `skills/systematic-debugging/condition-based-waiting.md`
- [ ] `skills/systematic-debugging/defense-in-depth.md`
- [ ] `skills/systematic-debugging/root-cause-tracing.md`
- [ ] `skills/test-driven-development/SKILL.md`
- [ ] `skills/test-driven-development/testing-anti-patterns.md`
- [ ] `skills/using-git-worktrees/SKILL.md`
- [ ] `skills/using-superpowers/SKILL.md`
- [ ] `skills/verification-before-completion/SKILL.md`
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

## Deficits

| ID | Severity | File | Evidence | Why it matters | Proposed fix | Verification |
|---:|:--|:--|:--|:--|:--|:--|
| 1 | Medium | `.codex/INSTALL.md` | “Skills update instantly through the symlink.” | This conflicts with earlier “Restart Codex” guidance and can cause confusion about when a restart is required after updates. | Clarify update behavior (for example: restart required to discover new skills; restart recommended after updates to ensure reload). | Re-read `.codex/INSTALL.md` for a single, non-contradictory instruction; ensure it matches current repo guidance. |
| 2 | Medium | `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-design.md` | Remediation section says “After the audit report is reviewed, create: `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-implementation.md`.” | This is misleading because that file already exists and is the audit execution plan; it conflates “audit plan” with “remediation plan”, risking incorrect next steps. | Update the design doc to reference the remediation plan doc (`docs/plans/2026-03-02-skill-ecosystem-integrity-audit-remediations-implementation.md`) and clarify intent. | Ensure the design doc’s referenced remediation plan path exists and matches the plan’s Task 9 output path. |
| 3 | Low | `skills/auditing-writing-plans/SKILL.md` | H1 is `# Auditing Writing-Plans Plans` | The title is redundant and slightly confusing; it adds friction for humans and risks being repeated verbatim in future references. | Rename H1 to a single, clear name (for example: `# Auditing Writing Plans`). | Re-read the file and confirm the H1 is clear and consistent with the skill name. |
| 4 | Medium | `skills/brainstorming/SKILL.md` | YAML description is: `\"You MUST use this before any creative work - creating features, building components, adding functionality, or modifying behavior. Explores user intent, requirements and design before implementation.\"` | The description includes workflow summary (“Explores … before implementation”) rather than strictly trigger conditions, increasing the chance an agent shortcuts by following metadata instead of reading the skill (CSO trap). | Rewrite description to only state triggering conditions (start with “Use when …”), with no workflow/process summary. | Confirm `description:` remains under ~500 chars, starts with “Use when”, and does not describe steps. |
| 5 | Medium | `skills/finishing-a-development-branch/SKILL.md` | YAML description ends with “guides completion of development work by presenting structured options for merge, PR, or cleanup” | The description includes workflow summary rather than strictly triggers; this risks metadata-shortcut behavior and bloats the most frequently loaded prompt metadata. | Rewrite description to only state triggering conditions (start with “Use when …”), with no workflow/process summary. | Confirm `description:` starts with “Use when”, stays under ~500 chars, and does not describe steps. |

## Context-Poisoning Candidates

None identified yet (only `README.md` and `.codex/INSTALL.md` reviewed).

## Seam Checks (workflow bridges)

Not reviewed yet.
