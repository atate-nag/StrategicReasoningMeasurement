# Corpus Build Record — 2026-06-10 to 2026-06-12

This document defines the **canonical analysis corpus** and inventories every artefact that exists in `strategy_documents` but is excluded from it, with reason.

---

## Canonical analysis corpus

**Definition** (Adrian 2026-06-12):

- **75 (entity × seed) cells** — 25 included entities × 3 seeds — each represented by a complete tight-window batch of **4 rungs** (TMBA, E1, E2, E3) generated back-to-back
- **300 AI-generated documents** total — all in `strategy_documents` with `generation_meta.superseded_by` UNSET
- **25 human-authored MBA reports** — one per included entity, anonymised, in `strategy_documents` with `source='human'`
- **Inclusion line for any single document**: `word_count ≥ 500` words AND coding pipeline analysis `status='complete'`. Anything below is a stub and excluded.
- All 300 AI canonical docs have a completed v5.3 coding pipeline analysis (`strategy_analyses.status='complete'`, `coder='corpus-v5_3-batch-2026-06-11'`)
- All 25 human canonical docs have a completed v5.3 coding pipeline analysis (`coder='corpus-v5_3-human-2026-06-11'`)

**TMBA word-count variance is accepted**: 17 TMBA docs fall in the 500-1500w range (still well above the stub line) — the minimal TMBA prompt produces wider length variance than the more-structured E1/E2/E3 rungs. These remain in the canonical set as **legitimate short responses to the unbounded prompt**, per Adrian's explicit ruling on 2026-06-11.

---

## Excluded artefacts in `strategy_documents` (95 rows total)

| Category | Count | Marker | Reason |
|---|---|---|---|
| Pre-truncation-fix superseded | 40 | `generation_meta.superseded_by='truncation_bug_fix_2026-06-11'` | Belonged to a tight-window batch where one rung produced a <500w stub due to 16K per-turn token-ceiling exhaustion. Whole batch retired wholesale per Adrian; replacement batches generated at `max_tokens=32000`. |
| Cleanup-retry duplicate (older complete or partial) | 33 | `generation_meta.superseded_by='corpus_dedup_2026-06-12'` | Same-title row from an earlier tight-window batch where the cell had ≥1 cleanup-retry batch (typically because the original batch was partial). Tiebreaker preferred complete tight-window batches; fell back to word-count when both complete. |
| Stub batch C-fix-E05 superseded | 4 | `generation_meta.superseded_by='cfix_retry_2026-06-12'` | C-fix regeneration of ENTITY_05_seed1 produced a 40-word E3 stub at the 32K ceiling; whole batch retired and replaced with a standalone retry that succeeded. |
| Stub batch E19_seed3 superseded | 4 | `generation_meta.superseded_by='e19_seed3_retry_2026-06-12'` | Same pattern for ENTITY_19_seed3 (TMBA stub at 438w; whole batch retired and replaced). |
| Duplicate stub with valid sibling | 2 | `generation_meta.superseded_by='duplicate_dedup_2026-06-11/12'` | ENTITY_35_E1_seed1 (16w) and ENTITY_19_TMBA_seed1 (381w) — each had a valid same-title sibling produced by the cleanup-retry; stub copy tagged out, sibling promoted to canonical. |
| Coding-noise probe pseudo-docs | 6 | `generation_meta.coding_probe_for=<source_id>` + title suffix `_codingprobe1..3` | Pseudo-doc clones of ENTITY_38_E2_seed1 and ENTITY_38_TMBA_seed1 used to measure pipeline-only SD on identical content (per [[project_pilot_test_battery_2026_06_10]] §4). Not corpus members. |
| Batch-smoke pseudo-docs | 2 | `generation_meta.batch_smoke_test=true` + title suffix `_batchsmoke` | Pseudo-doc clones used to verify the batched-pipeline path end-to-end (per pilot test 7). Not corpus members. |
| Pilot-audit (capped seed-1 ENTITY_38) | 4 | Title suffix `_pilot_audit_capped` + `generation_meta.pilot_audit_only=true` | ENTITY_38 seed-1's 4 rungs generated under the original `max_uses=8` web-search cap (before the 2026-06-10 unbounding decision). Retained as pilot audit artefact; not in the analysis corpus. |

**Total excluded**: 95 (40 + 33 + 4 + 4 + 2 + 6 + 2 + 4) rows in `strategy_documents` are tagged-out or carry a clear non-canonical flag.

**Sum check**: 300 canonical + 95 excluded + 25 human = 420 ENTITY_% rows in `strategy_documents` ✓ (matches DB count)

---

## Generation provenance highlights (also covered in B-series schemas)

### Per-turn token ceiling — bug-fix on 2026-06-11

After generation completed for 75 cells at `max_tokens=16000`, a diagnostic surfaced **11 stub rungs** (<500 words) caused by truncation at the 16K per-turn ceiling. Two distinct failure modes:

1. **`max_tokens` stubs (3 TMBA rungs)**: per-turn budget exhausted mid-thinking before final case-study output began. All TMBA-pattern.
2. **`pause_turn` stubs (8 across E1/E2/E3)**: server paused after web_search loop accumulated >1M input tokens; partial output of 7-19K tokens (introductory text only). All non-TMBA.

**Fix**: per-turn `MAX_TOKENS` raised from 16,000 to 32,000 in `scripts/generation/generate-document.ts`. This is a **per-turn ceiling change only — no prompt content or design changes**. Affected ten (entity × seed) cells regenerated as full tight-window batches at the new ceiling; old versions tagged-out and replaced wholesale.

**Residuals after C-fix**:
- 1 cell (ENTITY_05_seed1) still had an E3 stub at 32K (this time `pause_turn` early at 16K out tokens with 1.1M input) — one standalone retry succeeded (E3 4248w).
- 1 cell (ENTITY_19_seed3) had a TMBA stub at 438w that was below the diagnostic <200w threshold but above the inclusion <500w line — one standalone retry succeeded (clean 4-rung batch).

After all rescues: **all 75 cells × 4 rungs = 300 canonical AI docs, zero stubs**.

### Cleanup-retry dedup bug

`scripts/generation/ingest-generated-docs.ts` dedups on `b1_document_id` (generation-time UUID), not on title. When the cleanup-retry orchestrator regenerated tight-window batches for cells with partial original batches, both old (partial) and new (clean) sets of docs landed in DB. **33 such duplicate rows** were identified across 15 titles and tagged out via `scripts/generation/dedup-canonical-corpus.ts` on 2026-06-12.

Tiebreaker (per `dedup-canonical-corpus.ts` heuristic): prefer doc from complete tight-window batch (all 4 rungs in DB), then most recent `tight_window_completed_at`, then largest `word_count`.

---

## Verification queries

To verify the canonical-300 boundary:

```sql
-- 300 canonical AI docs
SELECT count(*) FROM strategy_documents
WHERE title ~ '^ENTITY_\d+_(TMBA|E[123])_seed\d+$'
  AND title NOT LIKE '%batchsmoke%'
  AND title NOT LIKE '%codingprobe%'
  AND title NOT LIKE '%pilot_audit_capped%'
  AND generation_meta->>'superseded_by' IS NULL;
-- Expected: 300

-- 300 canonical AI analyses, all complete
SELECT count(*) FROM strategy_analyses a
JOIN strategy_documents d ON a.document_id = d.id
WHERE d.title ~ '^ENTITY_\d+_(TMBA|E[123])_seed\d+$'
  AND d.title NOT LIKE '%batchsmoke%'
  AND d.title NOT LIKE '%codingprobe%'
  AND d.title NOT LIKE '%pilot_audit_capped%'
  AND d.generation_meta->>'superseded_by' IS NULL
  AND a.coding_method = 'pipeline'
  AND a.coder = 'corpus-v5_3-batch-2026-06-11'
  AND a.status = 'complete';
-- Expected: 300
```

For the 25 human canonical docs/analyses, replace title regex with `'^ENTITY_\d+_human$'` and coder with `'corpus-v5_3-human-2026-06-11'`.

---

## Cross-references

- Provenance pack Part B (schemas + freeze manifest): `B1`, `B3` (this file annotates B3 with the post-build truncation-fix entry)
- Routing policy: `ROUTING_POLICY.md`
- Memory: [[project_pilot_test_battery_2026_06_10]], [[project_strategy_zdr_routing_policy]], [[project_pipeline_resilience_2026_06_10]], [[project_pilot_variance_discipline]]
- Pre-committed analysis plan: TBD (Adrian to publish; lock before any reading of canonical-300 outputs)
