---
name: wordsmith
description: Use when the user provides prose that needs editing to be clearer, more concise, and easier to read while preserving meaning, facts, and tone constraints.
---

# Wordsmith

## Overview

`wordsmith` rewrites user-provided text into a clearer, tighter version without losing substance. It prioritizes plain language, strong structure, and careful meaning-preservation, drawing on widely taught principles from *The Elements of Style* (Strunk & White) without quoting it.

## Inputs

Required:
- The text to edit (can be Markdown).

Optional (use if provided):
- Audience (e.g., executives, engineers, customers)
- Tone (e.g., neutral, friendly, formal)
- Length target (e.g., “cut by ~30%”, “same length”)
- Must-keep terms/phrases (product names, legal phrases, slogans)
- Do-not-change regions (quotes, code blocks, citations)
- Dialect (US/UK), voice (1st/2nd/3rd person)

If none are given, assume: keep the same tone, US English, and broadly comparable length (but remove obvious filler).

## Workflow

### 1) Determine constraints (before editing)

Extract and honor:
- **Meaning constraints**: facts, numbers, dates, names, claims, quoted material.
- **Format constraints**: headings, bullets, links, tables, and any existing Markdown structure.
- **Voice constraints**: tense, person, and point-of-view.

If the request is ambiguous in a way that could change meaning (e.g., audience or purpose materially affects what “clear” means), ask **up to 2 short questions**. Otherwise proceed with conservative defaults.

### 2) Rewrite pass (clarity + concision)

Apply these heuristics (prioritize correctness over aggression):
- **Choose a suitable design and hold to it**: keep a consistent through-line; don’t reshuffle unless it improves clarity.
- **Omit needless words**: cut filler, hedges, throat-clearing, repeated qualifiers, and redundant phrases.
- **Prefer active voice** when it improves clarity and accountability.
- **Use strong verbs and concrete nouns**; reduce stacked nominalizations (“make an assessment” → “assess”) where safe.
- **Put statements in positive form** when it reads cleaner and doesn’t change intent.
- **Keep related words together**; move long modifiers next to what they modify.
- **Use parallel structure** in lists and paired constructions.
- **Make the paragraph structure do work**: one idea per paragraph; add headings/bullets when they improve scanability.
- **Prefer plain words** over inflated or trendy phrasing; define unavoidable jargon once.
- **Be specific**: replace vague placeholders with the user’s own concrete terms (do not invent new facts).
- **Trim preambles**: move key point earlier; lead with the conclusion when appropriate.

### 3) Clean-up pass (readability + correctness)

Perform a final check for:
- Unchanged facts (numbers, dates, names, attributions, requirements).
- No new claims or missing caveats.
- Consistent terminology (same concept, same label).
- Grammar, punctuation, and consistent capitalization.
- Preserved Markdown: don’t break links, code fences, tables, or list numbering.

## Elements of Style (paraphrased reminders)

Use these as a checklist, not as rigid laws:
- Make each paragraph about one topic; use topic sentences when helpful.
- Prefer active voice and positive form when clearer.
- Remove needless words; avoid strings of loose sentences.
- Use similar form for coordinate ideas; keep related words together.
- Keep tense consistent in summaries; place emphasis where it matters (often near the end).
- Avoid overwriting and overstatement; be clear; revise and rewrite.

## Output format

Default (most cases):
- Output **only** the revised text.

If the user asks for rationale, or if edits are substantial:
- Add a short `Notes` section listing the biggest structural/meaning-preserving changes and any unresolved ambiguities.

If the user requests multiple options:
- Provide `Option A (tighter)` and `Option B (closer to original)`.

## Guardrails (must follow)

- Do not add new facts, citations, dates, metrics, or product claims.
- Do not change legal/medical/financial meaning; if the text is high-stakes and unclear, ask clarifying questions instead of guessing.
- Preserve proper nouns, identifiers, filenames, and URLs exactly unless the user asks to normalize them.
- Do not “smooth over” uncertainty: keep explicit uncertainty if it exists in the source text.

## Common mistakes to avoid

- Over-compressing and dropping constraints, caveats, or acceptance criteria.
- Changing “may”/“must” strength, or softening/hardening requirements.
- Rewriting quoted material (unless explicitly permitted).
- Breaking Markdown formatting (links, code fences, tables).
