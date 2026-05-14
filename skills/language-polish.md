---
type: skill
id: language-polish
title: Language Polish
description: "Spelling, grammar, punctuation, sentence clarity, and minor wording cleanup"
tags: [Production, Quality]
connections:
  - target: llm-service
    type: runs_on
context_params:
  voice_profile:
    label: "Voice Profile"
    description: "Creator's writing style, vocabulary, sentence patterns, and banned words"
    required: false
  grammar_strictness:
    label: "Grammar Strictness"
    description: "How strict to be with grammar rules — Casual, Conversational, Professional, or Academic"
    default: "Professional"
    required: false
---

## Capability

Corrects spelling, grammar, and punctuation errors. Improves sentence clarity by simplifying convoluted phrasing, fixing awkward constructions, and tightening verbose passages. Makes minor wording adjustments for readability.

This is a surface-level polish — it does NOT rewrite for tone, restructure arguments, or reposition content for a different audience. Those are separate editorial decisions.

## When to Use

- As the final stage before any text output leaves the pipeline
- After drafting, revision, or assembly stages that may introduce inconsistencies
- On any output that will be read by someone other than the author

## What It Does

1. **Spelling and grammar** — corrects errors, including context-dependent ones (e.g. "their/there/they're")
2. **Punctuation** — fixes missing or misplaced commas, full stops, semicolons, and quotation marks
3. **Sentence clarity** — simplifies unnecessarily complex sentences without changing meaning
4. **Word choice** — replaces jargon or vague phrasing with clearer alternatives where the meaning is unambiguous
5. **Consistency** — standardises British/American English spelling within a single document (does not choose which — preserves the document's existing convention)

## What It Does NOT Do

- Rewrite for a different tone or audience
- Add or remove content
- Restructure paragraphs or arguments
- Check factual claims or citations
- Enforce a specific style guide (use `brief-compliance-check` or `consistency-check` for that)

## Inputs

Text content to polish.

## Outputs

Polished text with corrections applied. Optionally, a list of changes made.
