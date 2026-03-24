# JSON Backlog Shapes (Table Update Mode)

Confirm which shape you’re editing before selecting “row N” or `Sub-Issue ID=...`.

## Array of records

```json
[
  { "Sub-Issue ID": "PI-4-SI-2", "Parent Issue": "...", "Sub-Issue": "...", "Client Value": "...", "Definition of Done": "..." }
]
```

- “row 12” typically means the **1-based** array position (unless the user says 0-based).
- `Sub-Issue ID=...` means matching the `Sub-Issue ID` field (if present).

## Object keyed by Sub-Issue ID

```json
{
  "PI-4-SI-2": { "Parent Issue": "...", "Sub-Issue": "...", "Client Value": "...", "Definition of Done": "..." }
}
```

- `Sub-Issue ID=PI-4-SI-2` means the top-level key.

## Nested structures

If the JSON is nested (e.g. `{ "items": [...] }`), ask for the selection path (e.g. `items[12]` or `items[Sub-Issue ID=PI-4-SI-2]`).
