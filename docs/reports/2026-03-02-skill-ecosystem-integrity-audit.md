# Skill Ecosystem Integrity Audit (Manual-First)

**Date:** 2026-03-02  
**Audited commit:** 2ad0f51d0f6771a510b4e4cbddc60ba7cbf939c3  
**Worktree:** `.worktrees/skill-ecosystem-integrity-audit`

## Inventory (review order)

- [x] `README.md`
- [x] `.codex/INSTALL.md`
- [ ] `docs/plans/.gitkeep`
- [ ] `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-design.md`
- [ ] `skills/auditing-plan-execution/SKILL.md`
- [ ] `skills/auditing-writing-plans/SKILL.md`
- [ ] `skills/brainstorming/SKILL.md`
- [ ] `skills/executing-plans/SKILL.md`
- [ ] `skills/finishing-a-development-branch/SKILL.md`
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

## Deficits

| ID | Severity | File | Evidence | Why it matters | Proposed fix | Verification |
|---:|:--|:--|:--|:--|:--|:--|
| 1 | Medium | `.codex/INSTALL.md` | “Skills update instantly through the symlink.” | This conflicts with earlier “Restart Codex” guidance and can cause confusion about when a restart is required after updates. | Clarify update behavior (for example: restart required to discover new skills; restart recommended after updates to ensure reload). | Re-read `.codex/INSTALL.md` for a single, non-contradictory instruction; ensure it matches current repo guidance. |

## Context-Poisoning Candidates

None identified yet (only `README.md` and `.codex/INSTALL.md` reviewed).

## Seam Checks (workflow bridges)

Not reviewed yet.
