---
name: add-client-value
description: Use when one or more backlog Sub-Issues are missing Client Value (or have placeholder text) and need Client Value authored consistently from the surrounding Parent Issue/Sub-Issue context, optionally updating an existing backlog file.
---

# Add Client Value

## Overview

Add or repair `Client Value` text for one Sub-Issue or a set of Sub-Issues, using the surrounding backlog context (Parent Issue, Sub-Issue, and DoD) to write a **single-sentence, first-person, outcome-anchored** `Client Value` that makes the intent and user benefit concrete.

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

**Copy/paste mode** if the user provides a URL (or otherwise not writable): write `./scratchpaper/add-client-value-<timestamp>.md` containing Markdown + CSV snippets.

## Workflow

1) **Load the backlog and locate the target rows**

- Parse the backlog and confirm the column contract includes `Client Value` (and optionally IDs).
- Resolve the target rows using **exact string match** for names, or direct match for IDs.
- If the user selects by Parent Issue, enumerate all Sub-Issues under that Parent Issue and confirm the count.

2) **Decide fill vs rewrite**

Default behavior:
- Fill `Client Value` only when it is empty, `TBD`, or `[FILL IN]`.

If the user wants rewrites (non-empty values), ask for confirmation and scope (which rows to overwrite).

3) **Iterative Q&A (minimum 1–2 questions)**

Ask at least **1–2 questions** to avoid silent assumptions, then **STOP** (don’t draft yet) if answers materially affect what you’d write.

If the user forbids questions: proceed with conservative defaults; list `## Assumptions` and, if needed, `## Open questions (deferred)` in the output artifact (copy/paste mode) or a short `./scratchpaper/...md` note (table update mode).

High-leverage questions (pick the minimal subset):
- Who is “I” in this `Client Value` (role/persona)?
- What decision or next action should become easier after this Sub-Issue is done?
- Any phrases/terms to use or avoid (internal naming, client-facing language)?
- Should `Client Value` emphasize speed (triage), accuracy (avoid false alerts), explainability (tell the story), prioritization (rank), or workflow (route/escalate)?
- Any out-of-scope boundaries that should be reflected indirectly (what it should *not* promise)?

**Response constraint (Q&A step):** when stopping for clarification, respond with **only** the questions and a short “Reply with answers and I’ll write the Client Value updates” line.

4) **Write `Client Value` to the quality bar**

Rules:
- 1 sentence, first-person.
- Outcome-anchored; a “so I can …” clause is common but not required if the value and user benefit are already explicit.
- Must draw from the **specific Sub-Issue deliverable** implied by `Sub-Issue` + DoD (not generic value).
- Do not restate the Sub-Issue title in longer form; add the “why it matters” (what decision/action becomes easier).
- Avoid product-internal implementation language; focus on the user benefit and resulting decision/action.
- Prefer direct phrasing; avoid “so that” / “in order to” unless it materially clarifies the user action/decision.

Tight templates (adapt, don’t copy verbatim):
- “Help me quickly see **[what]** … so I can **[action]** …”
- “Alert me when **[condition]** … so I can **[respond]** …”
- “Give me a clear, structured explanation of **[pattern/story]** … so I can **[decide/communicate]** …”
- “Surface sustained, non-random **[events]** … so I’m not **[failure mode]** and can **[escalate/investigate]** …”
- “Let me confidently **[do the thing]** … by **[what this Sub-Issue provides]** …”
- “Make it easy for me to **[verify/compare/communicate]** … so I can **[approve/ship/act]** …”

Exemplar-style sentences (from `backlog.md`; ignore `[FILL IN]` rows):
- “Help me quickly see where **[metric]** is meaningfully high/low … so I can prioritize what needs attention and route follow-up without manually rebuilding the analysis.”
- “Alert me when **[metric]** meaningfully shifts (vs our own history and vs peers) … so I can catch emerging issues early without analyst support.”
- “Surface sustained, non-random events and patterns in **[metric]** … so I’m not falsely alerted to common cause variation and can justify escalation/investigation with clear time-series evidence.”
- “Give me a clear, structured explanation of where **[signals]** are concentrated and which underlying segmentations are contributing most.”

**Hard rule:** tailor `Client Value` to the Sub-Issue’s actual deliverable and user action. Don’t default to analytics/signal phrasing unless the Sub-Issue is actually about signals/metrics.

5) **Write the result**

Table update mode:
- Edit only the `Client Value` cell(s) for the selected row(s).
- Preserve column order and formatting; avoid touching unrelated rows.

Copy/paste mode:
- Write `./scratchpaper/add-client-value-<timestamp>.md` containing:
  - `## Assumptions` (only those actually used)
  - `## Parent/Sub-Issue selection` (what matched and why)
  - `## Updated rows (copy/paste)`:
    - A Markdown table snippet (header + updated rows) in the source column order
    - A CSV snippet (include header) in the source column order

## Example Quality Bar (Client Value)

If available, reference:
- `/home/ubuntu/repos/rnd/compass-core/.worktrees/hdd-artifacts-lineage/docs/new/backlog.md` (ignore rows where `Client Value` is `[FILL IN]`)

Expected traits:
- First-person and outcome-oriented (not “The system will…”).
- Specific about what the user gets (e.g., ranked list, clear explanation, reduced false alerts, an approval-ready artifact, faster workflow).
- Clearly tied to the Sub-Issue and its DoD artifacts (spec/PR/dataset/prototype/review notes—whatever fits).

## Common Mistakes

- Writing generic value that doesn’t reflect the specific Sub-Issue deliverable.
- Making `Client Value` a restatement of the Sub-Issue title (no “so I can” / no action).
- Promising implementation details or unverified outcomes.
- Overwriting existing `Client Value` without explicit permission.
- Name-based selection that is not exact or not unique.
