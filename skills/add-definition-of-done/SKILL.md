---
name: add-definition-of-done
description: Use when one or more backlog Sub-Issues are missing Definition of Done (or have placeholder text) and need deliverable-anchored, checkable DoD bullets authored from the surrounding Parent Issue/Sub-Issue context, optionally updating an existing backlog file.
---

# Add Definition of Done

## Overview

Add or repair `Definition of Done` for one Sub-Issue or a set of Sub-Issues, using the surrounding backlog context (Parent Issue, Sub-Issue, and `Client Value`) to write a **short, objective, checkable** DoD bullet list anchored to concrete deliverables.

## Inputs (Required)

You must have:
- A backlog **file path** (CSV / Markdown table / JSON) **or** a backlog **URL**
- A selection for which Sub-Issue row(s) to update:
  - Single Sub-Issue: `Sub-Issue ID` (preferred) or `Sub-Issue` name (exact match)
  - Multiple Sub-Issues: list of `Sub-Issue ID`s and/or exact `Sub-Issue` names
  - All Sub-Issues under a Parent Issue: `Parent Issue ID` (preferred) or `Parent Issue` name (exact match)

If selection is ambiguous, **STOP** and ask for `Sub-Issue ID`(s) or the exact strings to match.

## Mode Decision (Must Differentiate)

**Table update mode (edit in-place)** if the user provides a local backlog file path you can edit.

**Copy/paste mode** if the user provides a URL (or otherwise not writable): write `./scratchpaper/add-definition-of-done-<timestamp>.md` containing Markdown + CSV snippets.

## Workflow

1) **Load the backlog and locate the target rows**

- Parse the backlog and confirm the column contract includes `Definition of Done` (and optionally IDs).
- Resolve the target rows using **exact string match** for names, or direct match for IDs.
- If the user selects by Parent Issue, enumerate all Sub-Issues under that Parent Issue and confirm the count.

2) **Decide fill vs rewrite**

Default behavior:
- Fill `Definition of Done` only when it is empty, `TBD`, or `[FILL IN]`.

Never overwrite a non-empty DoD unless the user explicitly asks for rewrites (and specifies which rows).

3) **Iterative Q&A (minimum 1–2 questions)**

Ask at least **1–2 questions** to avoid silent assumptions, then **STOP** (don’t draft DoD yet) if answers materially affect what you’d write.

If the user forbids questions: proceed with conservative defaults; list `## Assumptions` and, if needed, `## Open questions (deferred)` in the output artifact (copy/paste mode) or a short `./scratchpaper/...md` note (table update mode).

High-leverage questions (pick the minimal subset):
- What is the primary deliverable for this Sub-Issue (spec, run outputs, dashboard, dataset, UI flow, etc.) and where does it live?
- What constitutes a “good” result (thresholds, correctness checks, acceptance gate)?
- Any required sources/specs/constraints (data source, privacy, performance, tooling)?
- Any reviewer/approver gate (who/what is the sign-off)?
- Should DoD include a client validation step (e.g., review notes confirming usefulness), or is internal validation sufficient?

**Response constraint (Q&A step):** when stopping for clarification, respond with **only** the questions and a short “Reply with answers and I’ll write the DoD updates” line.

4) **Write `Definition of Done` to the quality bar**

Rules:
- 2–6 bullets (prefer 2–4 unless the Sub-Issue genuinely needs more).
- Each bullet is objectively checkable and anchored to a deliverable (artifact + location/tool).
- Avoid redundant bullets: each bullet should add a distinct, non-implied check (remove bullets that are already covered by another bullet).
- Avoid subjective language (“complete”, “done”, “reviewed”) unless tied to an explicit artifact (“review notes exist”, “sign-off recorded”).
- Draw details from the surrounding Parent Issue/Sub-Issue context (don’t paste generic DoD).

Common DoD patterns (adapt to fit the context):

- **Spec + examples + validation (analytics / signal-style work)**
  - “A written specification exists that defines checks/thresholds/min-volume rules and any parameterization needed.”
  - “A run directory contains example outputs for multiple segmentations / slices that show triggers and evaluated values.”
  - “If client validation is required, review notes confirm multiple outputs are worth attention and align with expectations.”

- **Prioritization/ranking mechanism (analytics / triage)**
  - “A written prioritization specification exists that defines the ranking metric and how it’s calculated from underlying fields.”
  - “Example outputs show detected items + ranking metric + resulting rank order for multiple segmentations/slices.”

- **LLM interpretation mechanism (summarization / explanation)**
  - “A structured JSON contract is defined for the interpretation response.”
  - “The run output includes the exact prompt sent to the LLM.”
  - “Prompt and response are written to the run directory.”
  - “The response explains concentration patterns and identifies key contributing segmentations/drivers.”

- **Time-series event rules (e.g., Nelson rules)**
  - “A written rules specification defines in-scope rules, minimum history, thresholds/parameters, and minimum volume rules.”
  - “Example outputs show which rules triggered and include the supporting time-series evidence.”

- **Code change (library / service / pipeline)**
  - “A PR exists implementing **[the change]** with tests/validation appropriate to the repo.”
  - “A short doc/update exists describing behavior, assumptions, and how to run/verify it.”
  - “A runnable example (fixture / sample input) demonstrates expected outputs or behavior.”

- **Data artifact (dataset / table / schema)**
  - “A schema/contract exists (fields + definitions + provenance) and is reviewed/approved.”
  - “A dataset/table is produced in **[location]** with a reproducible generation method.”
  - “Basic correctness checks pass (row counts, null/uniqueness constraints, sanity checks).”

- **UI/UX artifact**
  - “A clickable flow / screen exists (implementation or prototype) covering the in-scope scenarios.”
  - “Acceptance checks cover key states/edge cases (empty/error/loading, permissions if applicable).”
  - “Review sign-off is recorded (design/product/eng as appropriate).”

**Hard rule:** pick patterns that match the Sub-Issue’s actual deliverable. “Run directory outputs” only belongs when the work produces run artifacts.

5) **Write the result**

Table update mode:
- Edit only the `Definition of Done` cell(s) for the selected row(s).
- Preserve column order and formatting; avoid touching unrelated rows.
- In Markdown tables, represent the DoD bullet list inside a cell using `<br>` separators (e.g., `- ...<br>- ...`), matching the deployed format.

Copy/paste mode:
- Write `./scratchpaper/add-definition-of-done-<timestamp>.md` containing:
  - `## Assumptions` (only those actually used)
  - `## Parent/Sub-Issue selection` (what matched and why)
  - `## Updated rows (copy/paste)`:
    - A Markdown table snippet (header + updated rows) in the source column order
    - A CSV snippet (include header) in the source column order

## Example Quality Bar (PI-1..PI-6 DoD)

If available, reference:
- `/home/ubuntu/repos/rnd/compass-core/.worktrees/hdd-artifacts-lineage/docs/new/backlog.md` (focus on Parent Issue IDs `PI-1`..`PI-6`; ignore `[FILL IN]` rows)

Expected traits:
- Bullets are deliverable-anchored (spec/PR/dataset/run outputs/review notes, as applicable).
- Includes concrete validation (example runs, evidence, rank order, tests, sanity checks—whatever fits the deliverable).
- Stays tightly aligned to the specific Sub-Issue rather than generic “AC”.

## Common Mistakes

- Writing generic DoD that doesn’t reflect the specific Sub-Issue deliverable.
- Using subjective checks without artifacts (“validated”, “reviewed”) with no evidence.
- Adding too many bullets, turning DoD into a design doc.
- Overwriting existing DoD without explicit permission.
- Name-based selection that is not exact or not unique.
