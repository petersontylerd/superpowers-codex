---
name: new-parent-issue
description: Use when creating a new backlog Parent Issue with one or more Sub-Issues from scratch, where each Sub-Issue needs clear Client Value and deliverable-anchored Definitions of Done, optionally writing new rows into an existing CSV/Markdown/JSON backlog.
---

# New Parent Issue

## Overview

Create a **new Parent Issue** and **one or more Sub-Issues** from scratch, using iterative Q&A to converge on crisp, action-oriented items with **client-outcome `Client Value`** and **checkable, deliverable-anchored** Definitions of Done (DoD).

## When to Use / Not Use

Use when the user is adding net-new backlog items (not just tightening wording).

Don’t use when the user already has the Parent/Sub-Issue set and only needs refinement (use `refining-backlog-items`).

## Inputs (Record Contract)

Required per Sub-Issue:
- `Parent Issue`
- `Sub-Issue`
- `Client Value`
- `Definition of Done`

Identifiers (recommended; required if writing into an existing backlog table that expects them):
- `Parent Issue ID` (e.g., `PI-7`)
- `Sub-Issue ID` (e.g., `PI-7-SI-1`)

Optional fields (include only if the user wants them, or if the target backlog includes them):
- `Component`, `Epic ID`, `Hypothesis ID(s)`, `Status`, `Priority`

## Mode Decision (Must Differentiate)

**Table update mode (edit in-place)** if the user provides a backlog file path (CSV / Markdown table / JSON) and wants the new rows written into it.

Otherwise use **new file mode** and write a new Markdown file under `./scratchpaper/`.

## Workflow

1) **Decide the target artifact**
- If the user gives a backlog file path: plan to update it in place.
- If the user does not give a path: default to a new file `./scratchpaper/new-parent-issue-<timestamp>.md`.

2) **Iterative Q&A (required)**

Ask 5–10 high-leverage questions, then **STOP** (don’t draft records yet) if the answers materially affect what you’d write.

Minimum question set:
- What’s the **theme/outcome** of this Parent Issue (metric/product capability/domain)?
- Who is the **primary user persona** (and what decision/action should become easier)?
- What’s explicitly **out of scope** for this Parent Issue?
- What are the **key deliverables** you want (spec, run outputs, dashboard, dataset, UI flow, etc.), and where should they live?
- How many Sub-Issues do you want (if unknown, ask permission to propose a first cut like 3–6)?
- Any **hard constraints** (deadline, data sources, privacy, performance, approvals/review gate)?
- Should we set any optional fields (`Component`, `Epic ID`, `Hypothesis ID(s)`, `Status`, `Priority`) or leave them blank?

**Response constraint (Q&A step):** when stopping for clarification, respond with **only** the questions and a short “Reply with answers and I’ll draft the new Parent Issue + Sub-Issues” line.

3) **Draft the Parent Issue + Sub-Issues**

Quality bar:
- **Parent Issue:** a tight theme that can plausibly group multiple deliverables without becoming a roadmap.
- **Sub-Issue:** verb-first, single objective, outcome-oriented.
- **Sub-Issue conciseness pass (required):** remove filler phrasing while preserving meaning; prefer a single strong verb phrase; avoid “mechanism for / process for / in order to / designed to / that will”.
- **Client Value:** 1 sentence, first-person, outcome-anchored (“Help me… so I can…”). Avoid product-internal implementation language.
- **DoD:** 2–6 bullets, objectively checkable, each anchored to a deliverable (artifact + location/tool) and validation/review when appropriate.

**Domain enrichment (apply judgment, don’t bloat):**
- Add the minimum domain-specific anchors that make DoD checkable (schemas, specs, run directories, review notes, example outputs, validation criteria).
- If the domain resembles “signal detection / analytics”: prefer DoD patterns like **written spec**, **example run outputs**, and **client review notes** (see PI-1..PI-6 examples below).

4) **Review loop**
- Ask the user to confirm or adjust: Parent Issue wording, number of Sub-Issues, what’s in/out of scope, and whether DoD is “checkable enough”.
- If the user changes intent or scope, revise and (if needed) ask a small follow-up question set.

5) **Finalize IDs and write to the chosen artifact**

**ID rules**
- If writing into an existing backlog with `Parent Issue ID` / `Sub-Issue ID`:
  - Scan the backlog to find the next available `PI-<n>` (max `n` + 1).
  - Use `PI-<n>-SI-<m>` for Sub-Issue IDs starting at `1`, increasing by `1`.
- If creating a new file with no established numbering: ask the user for the desired ID scheme; otherwise use placeholders (`PI-TBD`, `PI-TBD-SI-1`, ...).

**Markdown table rules (if writing a table)**
- Preserve the file’s existing column order and formatting.
- For multi-bullet DoD inside a table cell, use `<br>` separators (matching the deployed format).

## Output Formats

### New file mode (default)

Write `./scratchpaper/new-parent-issue-<timestamp>.md` containing:
- `## Assumptions` (only those actually used)
- `## Open questions (deferred)` (only if needed)
- `## New Parent Issue` with one section per Sub-Issue:
  - `Parent Issue ID: ...` (if known)
  - `Sub-Issue ID: ...` (if known)
  - `Parent Issue: ...`
  - `Sub-Issue: ...`
  - `Client Value: ...`
  - `Definition of Done:` (bullets)
- `## Backlog table (copy/paste)` (optional): a Markdown table representing the same records

### Table update mode

Update only the target backlog rows/sections:
- Append the new rows (don’t reorder unrelated records unless asked).
- Keep optional columns blank unless the user provides values.

## Example Quality Bar (PI-1..PI-6)

If available, reference:
- `/home/ubuntu/repos/rnd/compass-core/.worktrees/hdd-artifacts-lineage/docs/new/backlog.md` (PI-1..PI-6)

Common traits to emulate:
- Sub-Issues start with **Define / Validate / Apply** (not “Investigate”).
- `Client Value` is first-person and decision/action oriented.
- DoD is anchored to concrete artifacts (e.g., “written specification exists”, “run directory contains example outputs”, “client review notes confirm value”).

## Common Mistakes

- Parent Issue is too broad (turns into a roadmap) or too narrow (only one deliverable).
- Sub-Issues describe activities (“work on…”, “investigate…”) instead of deliverables and outcomes.
- `Client Value` is implementation-centric or not first-person.
- DoD is subjective (“done”, “reviewed”) instead of objectively checkable artifacts.
- IDs collide or don’t match the `PI-<n>` / `PI-<n>-SI-<m>` convention.
- In table mode, reformatting the whole backlog or changing unrelated rows.
