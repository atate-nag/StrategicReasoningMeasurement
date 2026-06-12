# Corpus Composition

## Canonical analysis corpus

- **300 AI-generated documents** = 25 entities × 3 seeds × 4 rungs (TMBA/E1/E2/E3)
- **25 human-authored MBA reports** = one per included entity (anonymised; not in this public release)
- All 325 carry a completed v5.3 coding pipeline analysis

## Inclusion line (per document)

- `word_count ≥ 500` AND coding pipeline `status='complete'`
- TMBA may legitimately produce 500-1500-word outputs (the minimal-prompt produces wider length variance); these are **included** as short-but-valid responses
- Documents below the 500-word line are stubs (model output its preamble but did not produce the case study) and **excluded**

## 95 excluded / audit-only artefacts in storage

All tagged with a machine-readable exclusion reason:

| Category | Count | Reason |
|---|---|---|
| Pre-truncation-fix superseded | 40 | Belonged to a tight-window batch where one rung produced a <500-word stub due to the 16K per-turn ceiling. Whole batch retired; replacement at max_tokens=32000. |
| Cleanup-retry duplicates | 33 | Same-title row from an earlier partial-or-older complete tight-window batch; superseded by the canonical batch via tiebreaker (complete tight-window first; then completed_at; then word_count). |
| C-fix-E05 stub batch | 4 | C-fix regen of ENTITY_05_seed1 produced a 40-word E3 stub at the 32K ceiling (early pause_turn, not max_tokens); whole batch retired and replaced with a standalone retry. |
| E19_seed3 stub batch | 4 | TMBA stub at 438 words (below 500-word inclusion line, above 200-word diagnostic threshold the original audit used); whole batch retired and replaced. |
| Duplicate stub with valid sibling | 2 | Two cells with a stub copy + a valid same-title sibling; stub copy tagged out, sibling promoted. |
| Coding-noise probe pseudo-docs | 6 | Pseudo-doc clones used to measure pipeline-only SD (the coding-noise floor); not corpus members. |
| Batch-smoke pseudo-docs | 2 | Pseudo-doc clones used to verify the batched-pipeline path; not corpus members. |
| Pilot capped seed-1 ENTITY_38 | 4 | Generated under the original `max_uses=8` web-search cap; superseded by the post-unbounding generation. |

Sum check: 300 canonical + 95 excluded + 25 human = 420 entity-named storage rows ✓
