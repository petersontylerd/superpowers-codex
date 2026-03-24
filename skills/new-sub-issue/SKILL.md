---
name: new-sub-issue
description: Use when adding one or more new Sub-Issues under an existing backlog Parent Issue, identified from an existing backlog file path or URL plus a Parent Issue ID or Parent Issue name.
---

# New Sub-Issue

## Overview

Add **one or more new Sub-Issues** under an existing Parent Issue, using iterative Q&A to converge on crisp, action-oriented Sub-Issues with clear `Client Value` and **deliverable-anchored, checkable** Definitions of Done (DoD).

## When to Use / Not Use

Use when the Parent Issue already exists and you’re extending it with additional Sub-Issues.

Don’t use when creating a brand-new Parent Issue (use `new-parent-issue`).

Don’t use when the Sub-Issues already exist and only need tightening (use `refining-backlog-items`).

## Inputs (Required)

To select the Parent Issue, you must have:
- A backlog **file path** (CSV / Markdown table / JSON) **or** a backlog **URL**
- A `Parent Issue ID` (preferred, e.g., `PI-4`) **or** `Parent Issue` name (**exact match**; must match uniquely)

To create new Sub-Issues, you must have (can be discovered via Q&A):
- Desired outcome / capability you want to add under the Parent Issue

## Record Contract (New Rows)

Required per new Sub-Issue row:
- `Parent Issue ID` (if the backlog uses it)
- `Sub-Issue ID` (if the backlog uses it)
- `Parent Issue`
- `Sub-Issue`
- `Client Value`
- `Definition of Done`

Optional fields (preserve the target backlog’s schema; fill only if user wants, otherwise inherit defaults or leave blank):
- `Component`, `Epic ID`, `Hypothesis ID(s)`, `Status`, `Priority`

## Mode Decision (Must Differentiate)

**Table update mode (edit in-place)** if the input is a local backlog file path you can edit.

**Copy/paste mode** if the input is a URL (or otherwise not writable): write `./scratchpaper/new-sub-issue-<timestamp>.md` and include Markdown + CSV snippets that can be pasted into the backlog.

## Workflow

1) **Load the backlog and locate the Parent Issue**

If given a **file path**:
- Open and parse the backlog.
- Find rows matching the `Parent Issue ID` or `Parent Issue` name.
- If the `Parent Issue` name is not an exact match, **STOP** and ask for the `Parent Issue ID` (or the exact `Parent Issue` string).
- If multiple Parent Issues match by name, **STOP** and ask for the `Parent Issue ID`.

If given a **URL**:
- Fetch the content (prefer “raw” Markdown if it’s a Git-hosted link).
- If you can’t fetch or parse it reliably, **STOP** and ask the user to paste the relevant Parent Issue rows (the header row + all rows for that Parent Issue).

2) **Summarize existing Sub-Issues (briefly)**

List the existing Sub-Issue titles (and IDs) under the Parent Issue so you can:
- Avoid duplicates
- Identify the natural “next” missing Sub-Issue(s)
- Keep the Parent Issue coherent

3) **Iterative Q&A (required)**

Ask 5–10 high-leverage questions, then **STOP** (don’t draft Sub-Issues yet) if the answers materially affect what you’d write.

Minimum question set:
- What gap is this new Sub-Issue meant to fill relative to the existing Sub-Issues?
- What is the primary deliverable (spec, run outputs, dashboard, dataset, UI flow, etc.) and where will it live?
- Who is the primary user persona for this Parent Issue (confirm), and what decision/action becomes easier?
- What’s explicitly out of scope for this new Sub-Issue?
- Any required sources/specs/constraints (data source, privacy, performance, tooling)?
- Any reviewer/approver gate (and what constitutes approval)?
- How many new Sub-Issues do you want to add (if unknown, ask permission to propose a first cut like 1–3)?
- Should optional fields inherit from the existing Parent Issue rows (`Component`, `Epic ID`, `Hypothesis ID(s)`, `Status`, `Priority`)?

**Response constraint (Q&A step):** when stopping for clarification, respond with **only** the questions and a short “Reply with answers and I’ll draft the new Sub-Issue row(s)” line.

4) **Draft the new Sub-Issue(s)**

Quality bar:
- **Sub-Issue:** verb-first, single objective, outcome-oriented.
- **Sub-Issue conciseness pass (required):** remove filler phrasing while preserving meaning; prefer a single strong verb phrase; avoid “mechanism for / process for / in order to / designed to / that will”.
- **Client Value:** 1 sentence, first-person, outcome-anchored (“Help me… so I can…”). Avoid product-internal implementation language.
- **DoD:** 2–6 bullets, objectively checkable, each anchored to a deliverable (artifact + location/tool) and validation/review when appropriate.

Consistency rules:
- Keep terminology aligned with the Parent Issue.
- If the Parent Issue uses a repeatable pattern across Sub-Issues, prefer extending that pattern, but still apply the conciseness pass (remove filler phrasing) unless the user explicitly wants more formal wording.

5) **Assign IDs and optional columns**

**Sub-Issue ID rules (if the backlog uses IDs like `PI-4-SI-3`)**
- Find the max existing `SI-<n>` under this `Parent Issue ID`.
- Assign the next sequential IDs: `PI-<n>-SI-<max+1>`, `PI-<n>-SI-<max+2>`, ...
- Do not reuse or renumber existing IDs.

**Optional columns**
- If the Parent Issue rows have consistent values (e.g., same `Component` / `Epic ID` / `Hypothesis ID(s)`), inherit them by default.
- Otherwise leave blank unless the user provides values.

6) **Write the result**

Table update mode:
- Append new row(s) to the backlog table.
- Preserve column order and formatting.
- For multi-bullet DoD inside a table cell, use `<br>` separators (matching the deployed format).

Copy/paste mode:
- Write `./scratchpaper/new-sub-issue-<timestamp>.md` containing:
  - `## Assumptions` (only those actually used)
  - `## Open questions (deferred)` (only if needed)
  - `## Parent Issue selection` (what matched and why)
  - `## New rows (copy/paste)`:
    - A Markdown table snippet in the same column order as the source backlog
    - A CSV snippet (include header) in the same column order as the source backlog

## Example Quality Bar (PI-1..PI-6)

If available, reference:
- `/home/ubuntu/repos/rnd/compass-core/.worktrees/hdd-artifacts-lineage/docs/new/backlog.md` (PI-1..PI-6)

Common traits to emulate:
- Sub-Issues start with **Define / Validate / Apply** (not “Investigate”).
- `Client Value` is first-person and decision/action oriented.
- DoD is anchored to concrete artifacts (e.g., “written specification exists”, “run directory contains example outputs”, “client review notes confirm value”).

## Common Mistakes

- Parent Issue selection is ambiguous (matched by name when multiple exist).
- New Sub-Issue duplicates an existing one (same deliverable framed differently).
- `Client Value` is not first-person or is implementation-centric.
- DoD is subjective (“complete”, “reviewed”) instead of checkable artifacts.
- IDs collide or skip without explanation; existing IDs get renumbered.
- In table update mode, reformatting the whole file or touching unrelated rows.
