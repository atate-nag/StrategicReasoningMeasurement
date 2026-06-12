# Pre-Committed Analysis Plan — Strategic Theatre Corpus

**Dated:** 2026-06-12, written before any cross-rung or human-vs-AI comparison was run; after corpus finalisation (300 canonical AI analyses across 75 cells × 4 rungs; 25 human reports) but before the confirmatory analysis below was executed. This is a characterising-study pre-registration: it fixes the measures, the aggregation, the reporting discipline, and the analysis order. It deliberately does NOT pre-set numerical thresholds or predict directions, because the study characterises the architecture of reasoning across conditions rather than testing a point hypothesis against a cutoff. The protection it provides is against selective reporting and post-hoc analysis choice, not against a predicted effect failing to reach a threshold.

## Measures examined (the complete confirmatory set)
No measure outside this set is computed for the confirmatory analysis:
1. Strict-Ext rate (claim-level attributable citation).
2. Strict/lenient citation delta (the detectability / apparent-vs-actual-grounding gap).
3. Node-type distribution (F / M / V / P).
4. Edge-type distribution (S / W / J / E).

Computed across the four rungs (TMBA / E1 / E2 / E3) and for human-vs-AI.

## Reporting discipline
All four measures are reported for all comparisons, whether or not each shows a pattern. No measure is dropped for being flat. Flat results are part of the finding.

## Aggregation
- Three seeds averaged within a cell (entity × rung); the entity is the replication unit.
- Seed-level spread shown alongside every cell/rung mean, so within-rung variation is always visible next to between-rung variation.
- No threshold for "real difference" is pre-set. The comparison is displayed (between-rung spread against within-rung spread) and characterised descriptively in light of the measured noise structure.

## Analysis order
The four measures above are computed and recorded FIRST, as the confirmatory pass, before any exploratory pattern-hunting. Exploratory analyses (unplanned slices, surprises) are run only after the confirmatory pass is written down, and are labelled exploratory in the paper, never presented as predicted.

## RQ-level procedures (commitments to run and report honestly, not predicted results)
- **RQ1 (consistency across the ladder):** for each measure, report rung means with seed spread; describe whether between-rung variation exceeds within-rung, measure by measure. Report all four whatever they show.
- **RQ2 (detectability):** report the strict/lenient delta per rung and per dimension; rank; describe the pattern. No pre-set "low-detectability" cutoff; gaps described as found.
- **RQ3 (the coincidence — keystone):** identify (a) which signatures vary least across the ladder and (b) which have the largest strict/lenient gap, each by the descriptive procedures above; then report whether they are the same signatures — reporting the overlap whatever it is, INCLUDING no relationship.

## Human-vs-AI
The same four measures applied identically to human and AI documents (the symmetry principle operationalised). Differences described, not tested against a cutoff.

## Pre-identified wide-variance rungs (flagged blind)
Two rungs are pre-identified, blind to results, as carrying wider variance for distinct reasons, and are therefore estimated with wider error bars:
- **E2** — coding-dense variance (measured pilot pipeline-noise floor ~3.7pp on strict-Ext; highest of the rungs).
- **TMBA** — generative-latitude variance (the minimal prompt produces the widest output-length spread; some cells 600–1500w, others 4000–6000w).
If either rung proves pivotal to a finding and its error bars are too wide to resolve, seed augmentation for that rung is a pre-identified (not post-hoc) move. "Pivotal" = the finding's direction depends on where that rung's mean sits within its current error band.

## Exclusions (pre-committed)
- Documents excluded only as: content-less stubs (<500 words, no analysis), or tagged duplicates / superseded generations.
- Short-but-complete documents (≥500 words with a real analysis) are INCLUDED.
- Any cell dropped for unrecoverable generation failure is documented by UID with reason.
- No exclusion is made on the basis of results.
- Final canonical corpus: 300 AI analyses (75 cells × 4 rungs), 25 human reports; 45 audit-only artefacts excluded with logged reasons.

## Note on length vs rate
Headline measures are RATES and PROPORTIONS (strict-Ext rate, strict/lenient delta), which are length-normalised. Output-length variance (notably in TMBA) is therefore expected to be largely orthogonal to the rate-based signal. If length proves to bear on a finding, it is handled transparently and reported.
