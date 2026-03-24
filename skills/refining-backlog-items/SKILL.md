---
name: refining-backlog-items
description: Use when grooming backlog items where Client Value or Definitions of Done are vague or not deliverable-anchored, especially for Parent/Sub-Issue record sets or when updating specific rows in CSV/Markdown/JSON backlog files.
---

# Refining Backlog Items

## Overview

Refine backlog records into **crisp, action-oriented** text with **deliverable-anchored, checkable** Definitions of Done (DoD).

Assume the audience is **internal team members**: keep wording tight and use relevant terms of art/proprietary context without over-explaining.

Don’t take rough issue prose too literally. Discern the domain/topic, identify gaps/pitfalls, and enrich the items with concise expert perspective so the backlog reflects what “good” looks like.

## When to Use / Not Use

Use when the user asks to refine / groom / tighten tickets, issues, sub-issues, user stories, acceptance criteria, or DoD.

Don’t use to invent new scope or produce project plans.

## Inputs (Record Contract)

Expected columns: `Parent Issue ID`, `Sub-Issue ID`, `Parent Issue`, `Sub-Issue`, `Client Value`, `Definition of Done`.

If fields are missing or record selection is ambiguous, ask clarifying questions before rewriting.

## Mode Decision (Must Differentiate)

**Table update mode (edit in-place)** if the user provides a backlog file path (CSV / Markdown table / JSON) *and* references specific record(s) (row number, `Sub-Issue ID`, or a unique key like `Parent Issue ID` + `Sub-Issue`).

Otherwise use **batch mode** (produce a refined set as a new markdown artifact).

## Workflow

1) **Validate + de-ambiguate**
- Confirm which fields to refine (default: `Parent Issue`, `Sub-Issue`, `Client Value`, `Definition of Done`).
- Treat `Parent Issue ID` / `Sub-Issue ID` as stable identifiers: do not rewrite them unless explicitly requested.
- In table mode, confirm selection keys; don’t guess if multiple rows match.

2) **Ask high-leverage questions (3–7)**
- What is the primary deliverable per Sub-Issue, and where does it live (tool/platform)?
- What’s out of scope / explicitly not required?
- Any required sources/specs/constraints?
- Any reviewer/approver gate?
- If unclear: what domain/outcome/KPI is this about, and what “good” means operationally?

If the user forbids questions: proceed with conservative defaults; list **Assumptions** + **Open Questions (deferred)**.

**Hard rule (batch mode):** if you asked clarifying questions that affect the rewrite, **STOP** and wait for answers. Ask questions in chat only. Do **not** write any `./scratchpaper/` file and do **not** rewrite records yet.

**Response constraint:** when stopping for clarification in batch mode, your response should contain **only** (a) the questions and (b) a short “Reply with answers and I’ll produce the refined backlog file” line—no rewritten records, no draft output.

3) **Rewrite to the quality bar**
- **Sub-Issue:** verb-first, single objective, outcome-oriented.
- **Sub-Issue conciseness pass (required):** remove filler phrasing while preserving meaning.
  - Prefer a single strong verb phrase; remove stacked helpers like “mechanism for”, “process for”, “approach to”, “in order to”, “designed to”, “that will”.
  - Prefer adjective+noun over “for automatically <verb>” (e.g., “automated detection” vs “a mechanism for automatically detecting”).
  - Replace “across different <plural noun>” with “across <plural noun>” unless “different” adds meaning.
  - Remove filler nouns unless they distinguish deliverables (“mechanism”, “framework”, “capability”, “methodology”).
  - Target: if you can remove ~25–40% of words from the title without losing meaning, do it.
- **Client Value:** 1 sentence, first-person, outcome-anchored (e.g., “Help me… so I can…”); avoid product-internal implementation language; state who benefits and what decision/action becomes easier.
- **DoD:** objective bullets anchored to a deliverable (artifact + tool/platform), plus review/sign-off when appropriate.
- **Style:** internal shorthand is fine; avoid long parentheticals, exhaustive definitions, and “painstaking” qualification of terms.
- **Expert lens (keep it tight):** add only what materially improves correctness/success—key dependencies, definitions/anchors, edge cases, risks, and validation.

4) **Produce artifacts**
- **Batch mode:** after questions are answered (or the user says “proceed with assumptions”), write `./scratchpaper/refined-backlog-<timestamp>.md` (create `./scratchpaper/` if missing) containing the refined records plus any assumptions used.
- **Table update mode:** edit only the specified row(s), preserving table format; optionally also write a short `./scratchpaper/...md` note with assumptions/questions.

## Quick Reference (Rewrite Rules)

- Replace “Investigate / look into / ensure” → “Define / measure / document / implement / validate / publish”
- Replace “Define and validate a X mechanism for automatically doing Y across different Z” → “Define and validate automated Y across Z”
- Replace subjective DoD (“reviewed”, “complete”) → objective checks (“artifact exists”, “numbers match”, “review sign-off recorded”)
- Anchor each record to one primary deliverable and where it lives (tool/platform).
- Write `Client Value` in a single first-person sentence that ties the work to a concrete user decision/action.
- If two deliverables exist, suggest splitting into two Sub-Issues.
- Keep terms of art/proprietary names, but don’t over-qualify (“CMS X”, “Vizient feed Y”) unless ambiguity would change acceptance.
- Don’t just restate the input—tighten it using expert judgment: clarify the “thing”, the anchor, the boundary, and how it’s verified.

## Example (Single Record)

**After**
- Parent Issue ID: “PI-7”
- Sub-Issue ID: “PI-7-SI-2”
- Parent Issue: “Root-cause data specification — Cardiology LOS outcome”
- Sub-Issue: “Document client-used LOS root-cause metrics (per site)”
- Client Value: “Let me quickly identify which LOS metrics and terminal fields matter for Cardiology RCA (per site), so I can investigate anomalies without reconstructing definitions from scratch.”
- Definition of Done:
  - “A completed worksheet exists per site listing metrics + required terminal fields.”
  - “A consolidated inventory identifies common vs site-specific metrics/fields.”

## Batch Output Format (Copy/Paste Friendly)

Default to **one record per section** (avoid markdown tables for multi-bullet DoD).

In `./scratchpaper/refined-backlog-<timestamp>.md`:
- `## Assumptions` (only those actually used)
- `## Refined records` with one `### Record N` section per record:
  - `Parent Issue ID: ...` (if present)
  - `Sub-Issue ID: ...` (if present)
  - `Parent Issue: ...`
  - `Sub-Issue: ...`
  - `Client Value: ...`
  - `Considerations:` (optional; max 3 bullets; only if it changes scope/DoD)
  - `Definition of Done:` (bullets)

**Avoid churn:** in batch mode, prefer writing a single “final” file after clarification rather than writing drafts and then rewriting.

## Table Update Notes (CSV / Markdown / JSON)

- Prefer selecting rows by `Sub-Issue ID` or row number; accept `Parent Issue` + `Sub-Issue` only if it matches uniquely.
- Preserve the file’s existing formatting; avoid re-wrapping/re-ordering unrelated content.
- For JSON: confirm which shape you’re editing (array vs keyed object vs nested) before interpreting “row N” or `Sub-Issue ID=...`. See `json-shapes.md`.

## Common Mistakes

- Editing wording without making DoD deliverable-anchored and objectively checkable.
- Inventing specifics (owners, systems, data sources) instead of asking or stating assumptions.
- Over-explaining internal terms; turning tickets into documentation.
- Treating rough prose as complete; missing obvious gaps (definitions, anchors, dependencies, validation).
- In table mode, reformatting the whole file or touching rows that weren’t requested.
