# Release Notes

## v1.0.15
GH#745 — declare per-step `output: {name, type}` on every execution step (tickets/text, priorities/text, workload/text, triage_report/text, polished_triage/text). Lights up the #744 rich flow-map. Content-only. (Completes the #745 sweep for this skrpt after the #754 error fix.)

## v1.0.14
GH#754 — restore the local `fetch-jira-tickets` prompt dropped by the GH#645 K-037 migration (the exec’s `prompt: fetch-jira-tickets` was resolving to the shared *skill* → `execution_prompt_wrong_type` ERROR). Reconcile the restored prompt’s connection target. Canonical scan clean.

## v1.0.13
Fix-forward after Row 3b v1.0.12 publish failure. The v1.0.12 per-skrpt CI's "Register version with Hub API" step failed because the consumer's source `manifest.id` (069d2091…) did not match the D1 catalogue row's id (87b76f93…) — a legacy drift from before Action 6 (`0bcc5ae0`) made publish-skrpt.mjs Step 2 INSERT use `manifest.id` for the D1 id column. v1.0.13 reconciles the source `manifest.id` to the catalogue authoritative value (Row-5-equivalent for consumers) and republishes. Per Adj-1: no re-tag of v1.0.12; the orphaned GitHub release artefact stays inert (no D1 versions row, no consumer pinned it).

## v1.0.12
GH#645 Row 3b — migrate to K-037 dep-referenced schema. Strip 4 inline shared-content files and declare 3 hub-shared deps (UUID id + slug name + version + checksum from `gen-dep-checksums.mjs`). Internal slug references rewritten for E2 rename/mirror-drop pair(s): ticket-fetch→fetch-jira-tickets. Closes pre-Step-3 inline-vendoring for this bundle.

## v1.0.11
Wave 2: re-signed with canonical engine signing pipeline.

## v1.0.10
Tags migrated inline into manifest (GH#586). tags.yaml retired.

## v1.0.9
Bundle re-signed with canonical engine signing pipeline (Wave 2 migration).

## v1.0.8
Signature fix — RELEASE_NOTES.md now included in integrity checksum.

## v1.0.7
Initial catalogue release with full structural and content-quality validation. All scanner checks pass.
