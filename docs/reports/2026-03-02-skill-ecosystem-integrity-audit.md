# Skill Ecosystem Integrity Audit (Manual-First)

**Date:** 2026-03-02  
**Audited base commit (origin/main):** 2585b1e335d90ec3d963e4b17b9896e54a4f9929  
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
- [x] `skills/requesting-code-review/code-reviewer.md`
- [x] `skills/systematic-debugging/SKILL.md`
- [x] `skills/systematic-debugging/condition-based-waiting.md`
- [x] `skills/systematic-debugging/condition-based-waiting-example.ts`
- [x] `skills/systematic-debugging/defense-in-depth.md`
- [x] `skills/systematic-debugging/find-polluter.sh`
- [x] `skills/systematic-debugging/root-cause-tracing.md`
- [x] `skills/test-driven-development/SKILL.md`
- [x] `skills/test-driven-development/testing-anti-patterns.md`
- [x] `skills/using-git-worktrees/SKILL.md`
- [x] `skills/using-superpowers/SKILL.md`
- [x] `skills/verification-before-completion/SKILL.md`
- [x] `skills/writing-plans/SKILL.md`
- [x] `skills/writing-skills/SKILL.md`
- [x] `skills/writing-skills/persuasion-principles.md`
- [x] `skills/writing-skills/testing-skills-under-pressure.md`
- [x] `skills/writing-skills/graphviz-conventions.dot`
- [x] `skills/writing-skills/render-graphs.js`

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
test -f "docs/plans/$(printf %s deployment-plan.md)" || echo 'example plan file missing (example?)'
```

Result:
- YAML frontmatter present with `name: requesting-code-review` and `description:` starting with “Use when …”.
- Local reference: `requesting-code-review/code-reviewer.md` exists.
- Example section references an example plan path under `docs/plans/` (for example `deployment-plan.md`), which does not exist in this repo.

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

### `skills/writing-plans/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/writing-plans/SKILL.md
rg -n '^name:|^description:' skills/writing-plans/SKILL.md
test -f skills/executing-plans/SKILL.md
```

Result:
- YAML frontmatter present with `name: writing-plans` and `description:` starting with “Use when …”.
- Outbound references: `superpowers:using-git-worktrees`, `superpowers:brainstorming`, and `superpowers:executing-plans` (all exist as skills).
- Uses example commands like `pytest ...` and `git ...` (examples, not repo-specific).
- External links: none.

Deficits: none found.

### `skills/writing-skills/SKILL.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/writing-skills/SKILL.md
rg -n '^name:|^description:' skills/writing-skills/SKILL.md
test -f skills/writing-skills/testing-skills-under-pressure.md
test -f skills/writing-skills/persuasion-principles.md
test -f skills/writing-skills/render-graphs.js
test -f skills/writing-skills/graphviz-conventions.dot
```

Result:
- YAML frontmatter present with `name: writing-skills` and `description:` starting with “Use when …”.
- Local references:
  - `@testing-skills-under-pressure.md` resolves and exists.
  - `persuasion-principles.md`, `render-graphs.js`, and `graphviz-conventions.dot` exist.
- External links: none.

Deficits: see Deficit #11 (external repo/documentation references are too vague; plus some citations appear non-verifiable in this repo context).

### `skills/requesting-code-review/code-reviewer.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/requesting-code-review/code-reviewer.md || true
```

Result:
- No outbound references detected (0 matches).
- External links: none.

Deficits: none found.

### `skills/systematic-debugging/condition-based-waiting.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/systematic-debugging/condition-based-waiting.md || true
test -f skills/systematic-debugging/condition-based-waiting-example.ts
```

Result:
- Outbound references: local file `condition-based-waiting-example.ts` is referenced and exists.
- External links: none.

Deficits: none found.

### `skills/systematic-debugging/condition-based-waiting-example.ts`

Commands run:
```bash
rg -n "from '~/threads" skills/systematic-debugging/condition-based-waiting-example.ts
```

Result:
- Contains project-specific TypeScript imports from `~/threads/...` that do not exist in this repository.

Deficits: see Deficit #14 (example is not self-contained / implies non-existent modules).

### `skills/systematic-debugging/defense-in-depth.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/systematic-debugging/defense-in-depth.md || true
```

Result:
- No outbound references detected (no `@...md`, no file paths, no skill refs).
- External links: none.

Deficits: none found.

### `skills/systematic-debugging/find-polluter.sh`

Commands run:
```bash
rg -n 'find \\. -path \"\\$TEST_PATTERN\"|npm test \"\\$TEST_FILE\"' skills/systematic-debugging/find-polluter.sh
```

Result:
- Script is Node/npm-specific (`npm test "$TEST_FILE"`).
- Uses `find . -path "$TEST_PATTERN"` with an example pattern like `src/**/*.test.ts`, which may be misleading depending on `find`’s pattern semantics.

Deficits: see Deficit #15 (script/example is likely misleading without stronger constraints or alternate usage).

### `skills/systematic-debugging/root-cause-tracing.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/systematic-debugging/root-cause-tracing.md || true
test -f skills/systematic-debugging/find-polluter.sh
```

Result:
- Outbound references: local file `find-polluter.sh` is referenced and exists.
- External links: none.

Deficits: none found.

### `skills/test-driven-development/testing-anti-patterns.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/test-driven-development/testing-anti-patterns.md || true
test -d "docs/$(printf %s examples)" || echo 'docs examples dir missing (example?)'
```

Result:
- No local file references detected; contains a generic “docs examples” suggestion that does not exist in this repo.
- External links: none.

Deficits: see Deficit #12 (ambiguous docs examples reference).

### `skills/writing-skills/persuasion-principles.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/writing-skills/persuasion-principles.md || true
rg -n 'https?://' skills/writing-skills/persuasion-principles.md || true
```

Result:
- No outbound references detected (no `@...md`, no file paths, no skill refs).
- External links: none.

Deficits: see Deficit #13 (citations/claims not anchored to a reachable reference).

### `skills/writing-skills/testing-skills-under-pressure.md`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/writing-skills/testing-skills-under-pressure.md || true
rg -n 'persuasion-principles\\.md' skills/writing-skills/testing-skills-under-pressure.md
test -f skills/writing-skills/persuasion-principles.md
```

Result:
- Outbound references: `superpowers:test-driven-development` and a same-directory reference to `persuasion-principles.md` (exists).
- External links: none.

Deficits: none found.

### `skills/writing-skills/graphviz-conventions.dot`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/writing-skills/graphviz-conventions.dot || true
```

Result:
- No outbound references detected.
- External links: none.

Deficits: none found.

### `skills/writing-skills/render-graphs.js`

Commands run:
```bash
rg -n '@[A-Za-z0-9_./-]+\\.md|docs/|skills/|superpowers:[a-z0-9-]+|`[^`]+`|https?://' skills/writing-skills/render-graphs.js || true
```

Result:
- No outbound references detected (no `@...md`, no file paths, no skill refs, no external links).
- Contains shell command examples (`which dot`, `brew install graphviz`, `apt install graphviz`) as platform guidance.

Deficits: none found.

### Cross-file integrity sweep

Commands run:
```bash
rg -n "docs/plans/|docs/reports/|skills/|\\.codex/|@[A-Za-z0-9_./-]+\\.md" -S .
rg -n "\\bope[n]code\\b|\\bcur[s]or\\b|claude-plug[i]n|dispatching-parallel-agent[s]|subagent-driven-developmen[t]" -S . --glob '!docs/plans/*' || true
```

Result:
- References found include the known deficits already logged (for example the requesting-code-review example plan-path reference and the docs examples directory suggestion).
- No legacy artifact hits outside `docs/plans/*` using word-boundary matching.

## Deficits

| ID | Severity | File | Evidence | Why it matters | Proposed fix | Verification |
|---:|:--|:--|:--|:--|:--|:--|
| 1 | Medium | `.codex/INSTALL.md` | “Skills update instantly through the symlink.” | This conflicts with earlier “Restart Codex” guidance and can cause confusion about when a restart is required after updates. | Clarify update behavior (for example: restart required to discover new skills; restart recommended after updates to ensure reload). | Re-read `.codex/INSTALL.md` for a single, non-contradictory instruction; ensure it matches current repo guidance. |
| 2 | Medium | `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-design.md` | Remediation section says “After the audit report is reviewed, create: `docs/plans/2026-03-02-skill-ecosystem-integrity-audit-implementation.md`.” | This is misleading because that file already exists and is the audit execution plan; it conflates “audit plan” with “remediation plan”, risking incorrect next steps. | Update the design doc to reference the remediation plan doc (`docs/plans/2026-03-02-skill-ecosystem-integrity-audit-remediations-implementation.md`) and clarify intent. | Ensure the design doc’s referenced remediation plan path exists and matches the plan’s Task 9 output path. |
| 3 | Low | `skills/auditing-writing-plans/SKILL.md` | H1 is `# Auditing Writing-Plans Plans` | The title is redundant and slightly confusing; it adds friction for humans and risks being repeated verbatim in future references. | Rename H1 to a single, clear name (for example: `# Auditing Writing Plans`). | Re-read the file and confirm the H1 is clear and consistent with the skill name. |
| 4 | Medium | `skills/brainstorming/SKILL.md` | YAML description is: `\"You MUST use this before any creative work - creating features, building components, adding functionality, or modifying behavior. Explores user intent, requirements and design before implementation.\"` | The description includes workflow summary (“Explores … before implementation”) rather than strictly trigger conditions, increasing the chance an agent shortcuts by following metadata instead of reading the skill (CSO trap). | Rewrite description to only state triggering conditions (start with “Use when …”), with no workflow/process summary. | Confirm `description:` remains under ~500 chars, starts with “Use when”, and does not describe steps. |
| 5 | Medium | `skills/finishing-a-development-branch/SKILL.md` | YAML description ends with “guides completion of development work by presenting structured options for merge, PR, or cleanup” | The description includes workflow summary rather than strictly triggers; this risks metadata-shortcut behavior and bloats the most frequently loaded prompt metadata. | Rewrite description to only state triggering conditions (start with “Use when …”), with no workflow/process summary. | Confirm `description:` starts with “Use when”, stays under ~500 chars, and does not describe steps. |
| 6 | Medium | `skills/receiving-code-review/SKILL.md` | “You’re absolutely right!” is labeled an explicit `CLAUDE.md` violation; and it instructs `gh api repos/{owner}/{repo}/pulls/{pr}/comments/{id}/replies`. | Repo intent is Codex-only; `CLAUDE.md` reference is ecosystem-noise. The `gh api` endpoint is likely wrong for “reply to a PR review comment” and may mislead users. | Replace `CLAUDE.md` mention with a repo-agnostic “explicit instruction violation” phrasing. Replace the `gh api` example with either a correct endpoint (with a note to confirm via `gh api --help`) or remove the endpoint and instruct “reply in-thread via GitHub UI / correct API; avoid top-level comment”. | Validate the file has no `CLAUDE.md` mentions; validate any retained `gh api` example is accurate (or is clearly marked as an example + verification step). |
| 7 | Medium | `skills/requesting-code-review/SKILL.md` | Example includes `PLAN_OR_REQUIREMENTS: Task 2 from docs/plans/<your-plan>.md` | This path does not exist in this repo; it reads like a real dependency and breaks referential integrity for readers and for any automated checks. | Change to a clearly hypothetical example path (e.g. `docs/plans/<your-plan>.md`) or reference a file that exists in-repo. | Ensure the example no longer references the old `deployment-plan.md` example path under `docs/plans/`, or add an explicit “example path” disclaimer. |
| 8 | Medium | `skills/using-git-worktrees/SKILL.md` | YAML description ends with “creates isolated git worktrees with smart directory selection and safety verification” | The description includes workflow summary rather than strictly triggers; this risks metadata-shortcut behavior and bloats prompt metadata for a frequently invoked workflow skill. | Rewrite description to only state triggering conditions (start with “Use when …”), with no workflow/process summary. | Confirm `description:` starts with “Use when”, stays under ~500 chars, and does not describe steps. |
| 9 | Medium | `skills/using-superpowers/SKILL.md` | YAML description includes “establishes how to find and use skills, requiring relevant skill loading before ANY response…” | The description includes workflow/behavioral instruction (“requiring … before ANY response”) rather than strictly triggers; this risks metadata-shortcut behavior and increases context cost. | Rewrite description to only state triggering conditions (start with “Use when …”), with no workflow/process summary. | Confirm `description:` starts with “Use when”, stays under ~500 chars, and does not describe steps. |
| 10 | Medium | `skills/verification-before-completion/SKILL.md` | YAML description includes “requires running verification commands and confirming output…” | The description includes workflow summary rather than strictly trigger conditions; this risks metadata-shortcut behavior and increases context cost. | Rewrite description to only state triggering conditions (start with “Use when …”), with no workflow/process summary. | Confirm `description:` starts with “Use when”, stays under ~500 chars, and does not describe steps. |
| 11 | Low | `skills/writing-skills/SKILL.md` | “Official guidance: Follow Codex skills authoring and discovery conventions (see OpenAI Codex skills documentation and the `openai/skills` repository).” and mentions of “Cialdini, 2021; Meincke et al., 2025” without in-repo references. | As written, this is not a resolvable reference from within this repo (no URL or pinned reference), which reduces trust and increases friction for readers trying to follow it. | Replace with a concrete pointer (URL) or a repo-local reference file; otherwise rephrase as non-cited general guidance. | Confirm the referenced guidance is reachable (or that the text no longer implies a specific source the reader can’t access). |
| 12 | Low | `skills/test-driven-development/testing-anti-patterns.md` | “Examine actual API response from docs examples” | A `docs` examples directory does not exist in this repo; this reads like a repo-local reference and reduces trust in the guidance. | Rephrase as conditional (“from your project’s docs examples, if available”) or remove the path-like wording and just say “from real API docs examples”. | Ensure the file no longer implies a local docs examples directory exists in this repo (or add it, if that’s desired). |
| 13 | Low | `skills/writing-skills/persuasion-principles.md` | Contains specific research claims (e.g. “Meincke et al. (2025)… N=28,000… 33% → 72%…”) and literature citations without any link/identifier. | Readers can’t verify these claims from inside the repo; this can reduce trust and makes it harder to maintain correctness over time. | Add a concrete link/identifier for each cited work (or explicitly label the numbers as illustrative and remove the appearance of precise sourcing). | Confirm the file either contains working links/identifiers or no longer implies precise, citable numeric claims without sources. |
| 14 | Medium | `skills/systematic-debugging/condition-based-waiting-example.ts` | Imports `ThreadManager` and event types from `~/threads/...` | This is presented as a “complete implementation”, but it is not self-contained and implies modules/types that don’t exist in this repo, creating confusion and context pollution. | Rewrite as a self-contained example (define minimal types/interfaces inline) or convert to Markdown pseudocode that doesn’t pretend to compile. | Confirm the example has no repo-external imports (or that it is explicitly labeled as non-compilable pseudocode). |
| 15 | Low | `skills/systematic-debugging/find-polluter.sh` | Uses `find . -path "$TEST_PATTERN"` with example `src/**/*.test.ts` and always runs `npm test "$TEST_FILE"`. | As written, this is likely to fail or mislead in many projects: `find -path` pattern semantics differ, `**` may not work, and `npm test` may not accept a bare file arg without `--`. | Clarify constraints (Node projects only), adjust example to a `find` pattern that works broadly, or accept a test-command template. | Run the script on a small fixture (or document an explicit “works with Jest/Vitest when…” constraint) and confirm the example pattern is correct. |
| 16 | Medium | `skills/executing-plans/SKILL.md` (and/or `skills/finishing-a-development-branch/SKILL.md`) | Neither file references `superpowers:verification-before-completion` despite relying on “verification output” and “tests pass” claims. | This is a workflow seam: the verification gate exists but is not explicitly wired into the execution/finish loop, increasing risk of “completion” claims without the verification discipline skill being loaded. | Add an explicit `**REQUIRED SUB-SKILL:** Use superpowers:verification-before-completion` before any completion/pass claims (e.g., in `executing-plans` Step 3 and/or in `finishing-a-development-branch` Step 1/3). | `rg -n "verification-before-completion" skills/executing-plans/SKILL.md skills/finishing-a-development-branch/SKILL.md` returns at least one intentional reference. |

## Remediation status

The deficit table above reflects audit-time findings. Current remediation status in `feat/skill-ecosystem-integrity-audit`:

- Deficit #1: fixed (83b3747)
- Deficit #2: fixed (1edc90a)
- Deficit #3: fixed (18900d0)
- Deficit #4: fixed (3c670b5)
- Deficit #5: fixed (328cdfd)
- Deficit #6: fixed (61d3331)
- Deficit #7: fixed (02303c4)
- Deficit #8: fixed (c39aa21)
- Deficit #9: fixed (a415241)
- Deficit #10: fixed (12ffe9a)
- Deficit #11: fixed (276653c)
- Deficit #12: fixed (1a7ac8a)
- Deficit #13: fixed (7a37afa)
- Deficit #14: fixed (5c5d6f7)
- Deficit #15: fixed (2c606ad)
- Deficit #16: fixed (07592e9)

## Context-Poisoning Candidates

- `skills/systematic-debugging/condition-based-waiting-example.ts` (Deficit #14): non-portable “complete implementation” code.
- YAML `description:` fields that summarize workflow (Deficits #4, #5, #8, #9, #10): higher risk of metadata-shortcut behavior.
- `skills/writing-skills/persuasion-principles.md` (Deficit #13): precise numeric claims without reachable sources.

## Seam Checks (workflow bridges)

Commands run:
```bash
rg -n "using-git-worktrees|writing-plans|\\.worktrees/|feat/" skills/brainstorming/SKILL.md
rg -n "worktree|feat/\\*|main/master|using-git-worktrees|executing-plans" skills/writing-plans/SKILL.md
rg -n "worktree|main/master|using-git-worktrees|/review|finishing-a-development-branch" skills/executing-plans/SKILL.md
rg -n "Verify Tests|npm test|cargo test|pytest|go test|worktree remove" skills/finishing-a-development-branch/SKILL.md
rg -n "verification-before-completion" skills/*/SKILL.md || true
```

Result:
- `brainstorming` → `using-git-worktrees` → `writing-plans` handoff is explicitly stated and consistent.
- `writing-plans` properly gates to worktree + `feat/*` and stops on `main`/`master`.
- `executing-plans` properly gates to worktree + non-`main`/`master` and hands off to `finishing-a-development-branch`.
- Gap: `verification-before-completion` is not referenced by `executing-plans` or `finishing-a-development-branch` (Deficit #16).
