# Inclusion Criterion (Two-Test Principle)

The 42 entities considered for the human-matched corpus were screened under a two-test principle. Both tests had to pass for inclusion.

## Test 1 — Prospective Public Data Availability
Could a competent analyst, working only from publicly available material at the time of screening, write a defensible strategy analysis of this entity? The test is **prospective** (would-the-data-suffice, not did-the-AI-handle-it). It is **independent of any AI output** — no AI generation had been run when the screen was applied. The screen is therefore symmetric: it does not advantage or disadvantage the AI condition.

Possible outcomes: `pass` / `fail` / `n_a`.

## Test 2 — Unit Coherence
Is this entity a coherent strategic unit (a firm or business unit with its own strategic agency), as opposed to a sub-organisation that inherits strategy from its parent? If it is a subsidiary without standalone strategy, or a government body acting on policy rather than competitive strategy, or a state-controlled entity with thin public reporting, the entity may pass Test 1 but still fail Test 2.

Possible outcomes: `pass` / `fail` / `n_a`.

## Three rules (recorded as `exclusion_reasons`)
- `rule_1_government_body` — public-sector / regulatory entities lacking a competitive-strategy unit
- `rule_2_subsidiary_no_standalone_strategy` — strategically subordinate organisations
- `rule_3_state_entity_thin_data` — state-controlled entities with insufficient public disclosure
- `test_1_data_insufficient` / `test_2_not_a_strategic_unit` — generic per-test failures
- `other` — flagged for individual review

## Conservative direction
The retained set is biased toward data-rich entities. This is the favourable case for AI grounding — if a persistent epistemic gap survives even on the data-rich side, the finding is stronger. The retention is therefore a methodological choice, not a sampling for effect.

## Anonymised classification record
`inclusion-records-public.jsonl` — one record per considered entity, UID-anonymised, with categorical fields only (no free-text `basis` that could leak company names). Researcher identifier replaced with `researcher_a`. All test outcomes and exclusion reasons are preserved.
