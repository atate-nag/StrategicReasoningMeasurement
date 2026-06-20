# RESULTS — strategic-reasoning measurement (settled i-evidence)

Generated 2026-06-20 from canonical analysis-outputs. No prose.

## 1. Three-way noise table

Source: `analysis-outputs/three-way-noise-table.csv`

| Sample | Noise source | n (type/cit/qual) | κ type | κ citation | κ qualifier | Coefficient |
|---|---|---|---|---|---|---|
| Tesla | AI-AI (5 pipeline runs × 10 pairwise aligned κs averaged) | 114/114/114 | 0.946 | 0.930 | 0.867 | Cohen κ |
| Tesla | Human-AI (Adrian v5.4 vs pipeline qual-v2; semantic-aligned) | 111/111/111 | 0.821 | 0.889 | 0.685 | Cohen κ |
| ROK | AI-AI (5 pipeline runs × 2 source docs × 10 pairs averaged) | 110/110/110 | 0.849 | 0.959 | 0.854 | Cohen κ |
| ROK | Human-AI (pipeline-run1 vs each human Adrian/Sotirios/Christian; mean of 3) | 48/48/48 | 0.498 | 0.482 | 0.206 | Cohen κ |
| ROK | Human-Human Fleiss (Adrian/Sotirios/Christian; un-reconciled) | 48/48/48 | 0.434 | 0.310 | 0.150 | Fleiss κ |
| ROK | Human-Human pairwise A↔S | 48/48/48 | 0.458 | 0.347 | 0.160 | Cohen κ |
| ROK | Human-Human pairwise A↔C | 48/48/48 | 0.517 | 0.416 | 0.467 | Cohen κ |
| ROK | Human-Human pairwise S↔C | 50/50/50 | 0.389 | 0.240 | 0.082 | Cohen κ |
| ROK | Human-AI pipeline↔Adrian | 48/48/48 | 0.447 | 0.621 | 0.368 | Cohen κ |
| ROK | Human-AI pipeline↔Sotirios | 50/50/50 | 0.489 | 0.462 | 0.122 | Cohen κ |
| ROK | Human-AI pipeline↔Christian | 50/50/50 | 0.559 | 0.362 | 0.127 | Cohen κ |

Figure: `analysis-outputs/figures/three-way-noise.svg` · `.tsv`

## 2. Endorsement rates (Adrian-reconciled, against v5.3)

Source: `analysis-outputs/endorsement-rates.csv`  ·  detail: `reports/coding-iteration/endorsement-sample-reconciled-final-completed.xlsx`

| Dimension | n | Human-orig endorse | AI-orig endorse | Reconciled-final endorse | Divergent | Reconciled rows |
|---|---|---|---|---|---|---|
| type | 32 | 87.5% | 84.4% | **87.5%** | 5 | 5 |
| citation | 30 | 60.0% | 73.3% | **63.3%** | 7 | 7 |
| qualifier | 30 | 43.3% | 66.7% | **73.3%** | 15 | 15 |
| edge_type | 32 | 71.9% | 68.8% | **71.9%** | 7 | 7 |

## 3. Reversal audit (B2/B3 split on Adrian's endorsement reversals)

Source: `analysis-outputs/reversal-audit.md`

| Dimension | Reversals | B2 (clear-spec drift) | B3 (standard-ambiguity) |
|---|---|---|---|
| type | 4 | 4 | 0 |
| citation | 1 | 1 | 0 |
| qualifier | 13 | 11 | 2 |
| edge_type | 6 | 5 | 1 |
| **TOTAL** | **24** | **21 (87.5%)** | **3 (12.5%)** |

Spec-citation rate: 24/24 = 100%.

## 4. Human-Human noise (ROK instrument, un-reconciled)

Source: `analysis-outputs/human-human-noise.csv`

| Dimension | n (3-way) | unanimous | κ(A,S) | κ(A,C) | κ(S,C) | Fleiss κ | Prevalence |
|---|---|---|---|---|---|---|---|
| type | 48 | 45.8% | 0.4580 | 0.5172 | 0.3893 | **0.4345** | V:0.222;F:0.354;M:0.319;P:0.104 |
| citation | 48 | 70.8% | 0.3469 | 0.4158 | 0.2398 | **0.3101** | None:0.826;Int:0.09;Ext:0.083 |
| qualifier | 48 | 64.6% | 0.1600 | 0.4667 | 0.0821 | **0.1500** | Q0:0.833;Q1:0.167 |

## 5. Human-Human disagreement buckets (Adrian-confirmed, n=57)

Source: `analysis-outputs/human-human-bucket-distribution.csv`  ·  detail: `reports/coding-iteration/human-human-disagreements-bucketed-completed.xlsx`

| Dimension | B1 | B2 | B3 | Total |
|---|---|---|---|---|
| type | 5 | 20 | 1 | 26 |
| citation | 0 | 14 | 0 | 14 |
| qualifier | 1 | 16 | 0 | 17 |
| **Plus 2 non-bucket flags** | | | | |
| `cit_294094ac` — AI-bucketer confabulation (caught by human review) | | | | 1 |
| `qual_7cc76f88` — extractor artefact (hedge dropped on atomisation) | | | | 1 |

B2 share over full 57 denominator: 50/57 = 87.7%.

## 6. Reconciler model-bias quantification

Source: `reports/coding-iteration/endorsement-sample-reconciled-final-completed.xlsx` (Reconciliation sheet)

On the 34 divergent rows, agreement with Adrian's final HUMAN_RECONCILED:

| Reconciler | Agreement with Adrian | Model family |
|---|---|---|
| Sonnet 4.6 reconciler | 27/34 = 79% | same family as original AI endorser |
| Opus 4.7 reconciler | **32/34 = 94%** | different family |
| Both reconcilers agree | 27/34 = 79% | — |

## 7. Pass-1 extractor-loss audit

Source: `analysis-outputs/extractor-loss-rates.csv` · summary: `analysis-outputs/extractor-loss-summary.md`

Sample: n=611 atomic claims across Tesla anchor + 2 ROK source docs + 24 AI-corpus docs.

| Loss type | Pairs with marker in source | ≥1 marker dropped | Drop rate (pair) | Marker survival |
|---|---|---|---|---|
| Hedge words (Q1+Q2 named list) | 22 | 8 | 36.4% nominal / **<10% true** (heuristic FP-inflated) | 63.6% |
| Citation markers (inline) | 46 | 37 | **80.4%** | **15.4%** |

| Group | n | hedge-drop rate | citation-drop rate |
|---|---|---|---|
| Tesla | 24 | 0% (0/2) | 100% (15/15) |
| ROK | 50 | 0% (0/2) | 86% (6/7) |
| AI corpus | 537 | 44% (8/18) | 67% (16/24) |

## 8. File index

### Analysis outputs
- `analysis-outputs/three-way-noise-table.csv` + `.md` — headline table
- `analysis-outputs/endorsement-rates.csv` — per-dim endorsement (orig + reconciled)
- `analysis-outputs/endorsement-summary.md` — endorsement narrative incl. reconciler-bias quantum
- `analysis-outputs/reversal-audit.md` — 24 reversals, B2/B3 per row
- `analysis-outputs/human-human-noise.csv` — Fleiss + pairwise κ + prevalence
- `analysis-outputs/human-human-bucket-distribution.csv` — B1/B2/B3 per dim
- `analysis-outputs/rok-human-ai-kappa.csv` — ROK Human-AI κ per source doc
- `analysis-outputs/ai-ai-noise.csv` — AI-AI noise (per-doc spread + Tesla+ROK PAIRWISE-ALIGNED κ rows)
- `analysis-outputs/extractor-loss-rates.csv` + `extractor-loss-summary.md` — Pass-1 marker preservation audit

### Filled coder spreadsheets
- `reports/coding-iteration/endorsement-sample-reconciled-final-completed.xlsx` (4 sheets: Endorsement, Reconciliation, RejectBuckets, Summary)
- `reports/coding-iteration/human-human-disagreements-bucketed-completed.xlsx` (Adrian-confirmed buckets + 2 methodological flags in cols O,P)

### Figures
- `analysis-outputs/figures/three-way-noise.svg` — publishable bar chart
- `analysis-outputs/figures/three-way-noise.tsv` — paper-ready tabular form

### Grounding appendices (claim + source-sentence + source-paragraph)
- `analysis-outputs/appendices/grounding/Tesla_reliability_anchor.xlsx`
- `analysis-outputs/appendices/grounding/ROK_Human.xlsx`
- `analysis-outputs/appendices/grounding/ROK_Claude_T2.xlsx`
- `analysis-outputs/appendices/grounding/all-audited-claims.csv` (n=611 audit set)

### Transparency-repo staging
- `transparency-staging/` — mirror of the above, ready for push to `github.com/atate-nag/StrategicReasoningMeasurement`
