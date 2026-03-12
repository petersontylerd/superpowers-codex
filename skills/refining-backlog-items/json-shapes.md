# JSON Backlog Shapes (Table Update Mode)

Confirm which shape you’re editing before selecting “row N” or `id=...`.

## Array of records

```json
[
  { "id": "A-12", "Parent Issue": "...", "Sub-Issue": "...", "Description": "...", "Definition of Done": "..." }
]
```

- “row 12” typically means the **1-based** array position (unless the user says 0-based).
- `id=...` means matching the `id` field (if present).

## Object keyed by id

```json
{
  "A-12": { "Parent Issue": "...", "Sub-Issue": "...", "Description": "...", "Definition of Done": "..." }
}
```

- `id=A-12` means the top-level key.

## Nested structures

If the JSON is nested (e.g. `{ "items": [...] }`), ask for the selection path (e.g. `items[12]` or `items[id=A-12]`).

