# Pressure Scenarios (RED/GREEN)

These scenarios are used to validate that an agent can refine backlog records into crisp, action-oriented, deliverable-anchored items, while asking the *minimum necessary* clarifying questions and producing an output artifact.

## Scenario 1 — Batch records, ambiguous scope (pressure: speed + ambiguity)

**Input:** User pastes 5–20 records with columns: Parent Issue, Sub-Issue, Description, Definition of Done.

**Pressure:** “Please refine these quickly; don’t ask too many questions.”

**Baseline failure patterns to look for (RED):**
- Leaves `Parent Issue` untouched even when it’s long, inconsistent, or not outcome-oriented.
- Rewrites text but does not ask *any* clarifying questions despite ambiguous deliverables.
- Produces “better wording” but keeps DoD vague (e.g., “validated”, “complete”, “documented”) and not deliverable-anchored.
- Repeats the rough prose too literally; misses obvious gaps (definitions/anchors, dependencies, validation, risks).
- Returns text in chat only and does not create a `./scratchpaper/...md` artifact.
- Outputs as a markdown table even when DoD is multi-bullet and copy/paste becomes painful.
- Over-explains internal terms of art/proprietary context instead of keeping tickets punchy.

**Pass criteria (GREEN):**
- Asks 3–7 high-leverage questions (or states explicit assumptions when the user forbids questions).
- In batch mode, pauses after questions and waits for answers before writing any `./scratchpaper/refined-backlog-...md` file or rewriting records.
- Produces refined records as one-record-per-section (copy/paste friendly).
- DoD bullets are objective, checkable, and anchored to a deliverable (doc/table/PR/dashboard) plus review/approval when appropriate.
- Writes the refined output to `./scratchpaper/` by default (creating the directory if needed) and links to the file path.
- Keeps wording tight for an internal audience; retains terms of art but avoids long qualifiers/definitions.
- Adds concise expert considerations only where they materially improve correctness/success; avoids overengineering.

## Scenario 2 — Table update mode (pressure: do-not-touch-other-rows)

**Input:** User provides a table path (CSV / Markdown / JSON) and references a specific record (row number, `id`, or a stable key like `Parent Issue` + `Sub-Issue`).

**Pressure:** “Update only this row. Do not reformat the table.”

**Baseline failure patterns to look for (RED):**
- Updates the content in chat but does not edit the referenced file.
- Reformats the entire file (delimiter/quoting/whitespace/ordering), creating noisy diffs.
- Updates multiple rows despite being asked to update one.

**Pass criteria (GREEN):**
- Confirms the record selection key(s) are unambiguous; asks a clarification if not.
- Edits only the specified row(s) in-place and preserves the table format.
- Optionally writes a `./scratchpaper/...md` note containing the refined text and assumptions (but does not require it).

## Scenario 3 — Conflicting constraints (pressure: “no questions” + “be precise”)

**Input:** User says “Don’t ask clarifying questions,” but records lack key info (owner, artifact location, review gate, metric definition, etc.).

**Baseline failure patterns to look for (RED):**
- Hallucinates specifics (owners, systems, exact datasets) instead of stating assumptions.
- Produces DoD that cannot be evaluated without missing facts.

**Pass criteria (GREEN):**
- Uses an explicit “Assumptions” section with conservative defaults.
- Keeps scope narrow; avoids inventing domain specifics.
- Flags “Open Questions (deferred)” without blocking delivery of refined wording.
- If questions are allowed, asks them *before* producing a refined backlog artifact.

## Scenario 4 — JSON backlog file, ambiguous shape (pressure: “just update it”)

**Input:** User provides a JSON file path and says “update row 12” or “update id=abc”, but the JSON could be:
- An array of records, or
- An object keyed by id, or
- Nested under a field like `items`

**Baseline failure patterns to look for (RED):**
- Assumes an array and updates the wrong record.
- Assumes an `id` key exists when it doesn’t.
- Reformats the entire JSON file (whitespace/key ordering) creating noisy diffs.

**Pass criteria (GREEN):**
- Inspects the JSON shape and, if selection is ambiguous, asks a clarification (“array index vs id key vs nested path”).
- Updates only the referenced record(s) and preserves existing JSON formatting as much as possible.
