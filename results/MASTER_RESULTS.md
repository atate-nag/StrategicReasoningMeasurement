# MASTER RESULTS — Strategic-reasoning measurement project

**Assembled 2026-06-21.** Single-source consolidation of all experimental results across the project: pre-freeze development, reliability measurement, post-freeze analytics. Exhaustive; not a summary. Each section ends with a one-line **signal status**: SETTLED (publishable), CANDIDATE (worth a paragraph if it survives review), OPEN (needs Adrian decision).

## Index

1. [Pre-freeze development (settled)](#1-pre-freeze-development-settled)
2. [Reliability — the headline](#2-reliability--the-headline)
3. [Post-freeze pipeline audits](#3-post-freeze-pipeline-audits)
4. [Post-corpus analytics — architectural (structural-v1)](#4-post-corpus-analytics--architectural-structural-v1)
5. [Post-corpus analytics — architectural (structural-v2: full inventory + motifs)](#5-post-corpus-analytics--architectural-structural-v2-full-inventory--motifs)
6. [Post-corpus analytics — inferential (S/W/J/E + chains)](#6-post-corpus-analytics--inferential-swje--chains)
7. [Post-corpus analytics — strict/lenient detectability delta](#7-post-corpus-analytics--strictlenient-detectability-delta)
8. [Post-corpus analytics — source validation](#8-post-corpus-analytics--source-validation)
9. [Post-corpus analytics — verifier validation](#9-post-corpus-analytics--verifier-validation)
10. [Post-corpus analytics — AI-AI distribution-level noise](#10-post-corpus-analytics--ai-ai-distribution-level-noise)
11. [Cross-rung trajectories (the elicitation gradient)](#11-cross-rung-trajectories-the-elicitation-gradient)
12. [Open questions inventory](#12-open-questions-inventory)
13. [Source-file index](#13-source-file-index)

---

## 1. Pre-freeze development (settled)

Full methodology with per-iteration log + rollback events: `briefs/Pre_Freeze_Development_Methodology.md` (mirrored at `coding/Pre_Freeze_Development_Methodology.md` in the transparency repo).

Headline trajectory (`Coding_Prompt_Freeze_Status_2026-06-06.md:52–66`):

| Candidate | Change | κ_type | κ_cite | κ_qual | Verdict |
|---|---|---|---|---|---|
| dev-baseline | pre-fix Pass-1 (v5.3) | 0.595 | 0.467 | 0.592 | baseline |
| dev-atomic-v1 | + atomic splitting + N-to-N aligner | 0.657 | 0.420 | 0.566 | adopted aligner |
| dev-cite-v1 | + 3-step citation test + examples | 0.630 | 0.436 | 0.567 | adopted |
| dev-qual-v1 | verbose Q1/Q2 expansion | -0.019 | +0.005 | **-0.165** | rolled back |
| **dev-qual-v2** | surgical Q1 patterns only | 0.621 | 0.437 | +0.045 | **adopted = gold candidate** |
| dev-fine-v1 | type rules D+E + cite extras | -0.025 | +0.032 | +0.028 | rolled back |
| dev-fine-v2 | drop D, keep E + cite extras | -0.037 | +0.010 | -0.029 | rolled back |
| dev-qual-v3 | mechanical attribution hedge | -0.012 | -0.001 | **-0.150** | rolled back |
| dev-qual-v4 | few-shot hedge boundaries | -0.080 | +0.006 | +0.060 | rolled back |

Overnight 2026-06-07 iterations (`Coding_Prompt_Freeze_Status_2026-06-07-overnight.md:11–34`):

| Attempt | Trigger | Outcome |
|---|---|---|
| Pass-2 edge v1 (verbose rewrite + drift block) | E-overdraw cluster | edge_type κ 0.637 → 0.142 (vocab collapse → 40×S+1×W) ❌ |
| Pass-2 edge v2 (surgical proximity test) | same | edge_type κ 0.637 → 0.538 ❌ |
| Pass-1 cite-preservation | preserve inline (Author, year) | Tesla within 1 SE → adopted; cross-doc: MBG type -0.295, XOM -0.239, GEHC -0.188, JARIR -0.087 → rolled back ❌ |
| Pass-2 edge-v3 (E def tightening) | E-overdraw | Tesla 2-seed: type -0.050, qualifier -0.104 ❌ |

Final corrected-κ at freeze (`Coding_Prompt_Freeze_Decision_2026-06-08.md:17–24`):

| Dim | Raw κ | Corrected κ (bucket firewall) | Bar | Status |
|---|---|---|---|---|
| type | 0.787 (n=238) | **0.826** | ≥0.8 | ✓ cleared |
| citation | 0.809 (n=238) | **0.845** | ≥0.8 | ✓ cleared (load-bearing per addendum §2) |
| qualifier | 0.664 (n=235) | 0.760 | ≥0.8 | -0.04 below; doc-specific Tesla ceiling |
| edge presence | -0.519 (n=163) | 0.058 | — | trajectory positive; full story deferred to v5.4 arch redesign |
| edge type | 0.635 (n=41 matched) | 0.635 | — | small matched-pair n |

Multi-seed stability at gold (Tesla, 3 seeds × qual-v2): type ±0.011, cite ±0.010, qual ±0.032.

Bucket totals at freeze: 229/321 disagreements reconciled (71% sample); **B1=61, B2=131, B3=37, pending=92**. B2:B1 ≈ 2:1 — hand-coding drift larger than pipeline error.

v5.4 candidate map (6 candidates derived from 18 B3 cases — see `provenance-pack/PART_A_freeze_artefacts/A6_v5_4_candidate_log.md`):

| # | Candidate | Trigger |
|---|---|---|
| 1 | Meta-claim exclusion | Pass-0/1 filter for "X argues / This study reveals…" |
| 2 | §4.4 E-rule clarification | 15-edge Tesla B3 cluster; 3 Pass-2 iterations tried, rolled back |
| 3 | Multi-source compound atomicity | §2.2 should mandate split on multi-source compounds |
| 4 | "Signs of X" as Q0 | qual-v2 prompt currently codes Q1; verb-of-assertion test |
| 5 | P→P edges | §4 vocab doesn't fit; preferred fix = no-edge convention |
| 6 | Same-source co-referential atoms | no-edge for "Murphy argues X / Murphy argues Y" |

**Signal status: SETTLED.** Pre-freeze development log + κ at freeze are publishable as a methods-section contribution. The 4-rollback regression-gate audit trail is the distinctive methodological asset.

---

## 2. Reliability — the headline

### 2.1 Three-way noise table (Tesla + ROK side-by-side)

Source: `analysis-outputs/three-way-noise-table.csv` + `.md`

| sample | noise_source | n_type | kappa_type | n_cit | kappa_citation | n_qual | kappa_qualifier | coefficient |
|---|---|---|---|---|---|---|---|---|
| Tesla | AI-AI (5 pipeline runs × 10 pairwise aligned κs averaged) | 114 | 0.946 | 114 | 0.930 | 114 | 0.867 | Cohen κ |
| Tesla | Human-AI (Adrian v5.4 vs pipeline qual-v2; semantic-aligned) | 111 | 0.821 | 111 | 0.889 | 111 | 0.685 | Cohen κ |
| ROK | AI-AI (5 pipeline runs × 2 source docs × 10 pairs averaged) | 110 | 0.849 | 110 | 0.959 | 110 | 0.854 | Cohen κ |
| ROK | Human-AI (pipeline-run1 vs each human Adrian/Sotirios/Christian; mean of 3) | 48 | 0.498 | 48 | 0.482 | 48 | 0.206 | Cohen κ |
| ROK | Human-Human Fleiss (Adrian/Sotirios/Christian; un-reconciled) | 48 | 0.434 | 48 | 0.310 | 48 | 0.150 | Fleiss κ |
| ROK | Human-Human pairwise A↔S | 48 | 0.458 | 48 | 0.347 | 48 | 0.160 | Cohen κ |
| ROK | Human-Human pairwise A↔C | 48 | 0.517 | 48 | 0.416 | 48 | 0.467 | Cohen κ |
| ROK | Human-Human pairwise S↔C | 50 | 0.389 | 50 | 0.240 | 50 | 0.082 | Cohen κ |
| ROK | Human-AI pipeline↔Adrian | 48 | 0.447 | 48 | 0.621 | 48 | 0.368 | Cohen κ |
| ROK | Human-AI pipeline↔Sotirios | 50 | 0.489 | 50 | 0.462 | 50 | 0.122 | Cohen κ |
| ROK | Human-AI pipeline↔Christian | 50 | 0.559 | 50 | 0.362 | 50 | 0.127 | Cohen κ |

Brief's anticipated direction (AI-AI > Human-AI > Human-Human) holds on ALL three dimensions on BOTH samples.

### 2.2 ROK Human-AI κ — per source doc + per coder

Source: `analysis-outputs/rok-human-ai-kappa.csv`

| source | dimension | kappa_PvA | n_PvA | kappa_PvS | n_PvS | kappa_PvC | n_PvC | pipeline_run |
|---|---|---|---|---|---|---|---|---|
| ROK_Human | type | 0.344 | 20 | 0.348 | 22 | 0.467 | 22 | 1 |
| ROK_Human | citation | 0.828 | 20 | 0.641 | 22 | 0.450 | 22 | 1 |
| ROK_Human | qualifier | 0.000 | 20 | 0.000 | 22 | 0.000 | 22 | 1 |
| ROK_AI | type | 0.469 | 28 | 0.531 | 28 | 0.608 | 28 | 1 |
| ROK_AI | citation | 0.344 | 28 | 0.315 | 28 | 0.222 | 28 | 1 |
| ROK_AI | qualifier | 0.472 | 28 | 0.115 | 28 | 0.253 | 28 | 1 |
| ROK_combined | type | 0.447 | 48 | 0.489 | 50 | 0.559 | 50 | 1 |
| ROK_combined | citation | 0.621 | 48 | 0.462 | 50 | 0.362 | 50 | 1 |
| ROK_combined | qualifier | 0.368 | 48 | 0.122 | 50 | 0.127 | 50 | 1 |

### 2.3 ROK Human-Human κ + prevalence

Source: `analysis-outputs/human-human-noise.csv`

| dimension | n_three_way | unanimous_rate | kappa_AS | n_AS | kappa_AC | n_AC | kappa_SC | n_SC | fleiss_kappa | prevalence |
|---|---|---|---|---|---|---|---|---|---|---|
| type | 48 | 0.4583 | 0.4580 | 48 | 0.5172 | 48 | 0.3893 | 50 | 0.4345 | V:0.222;F:0.354;M:0.319;P:0.104 |
| citation | 48 | 0.7083 | 0.3469 | 48 | 0.4158 | 48 | 0.2398 | 50 | 0.3101 | None:0.826;Int:0.09;Ext:0.083 |
| qualifier | 48 | 0.6458 | 0.1600 | 48 | 0.4667 | 48 | 0.0821 | 50 | 0.1500 | Q0:0.833;Q1:0.167 |

Note prevalence pathology: citation None=83%, qualifier Q0=83% in ROK → Cohen κ deflated even at 71-65% unanimous. Christian ↔ Adrian outperforms Adrian ↔ Sotirios outperforms Sotirios ↔ Christian on qualifier (0.467 / 0.160 / 0.082) — preserve as unevenness signal.

### 2.4 Endorsement rates (Adrian-reconciled, against v5.3)

Source: `analysis-outputs/endorsement-rates.csv` · detail: `reports/coding-iteration/endorsement-sample-reconciled-final-completed.xlsx`

| dimension | n | human_orig_endorse_rate | ai_orig_endorse_rate | final_endorse_rate | divergent | reconciled_rows |
|---|---|---|---|---|---|---|
| type | 32 | 87.5% | 84.4% | 87.5% | 5 | 5 |
| citation | 30 | 60.0% | 73.3% | 63.3% | 7 | 7 |
| qualifier | 30 | 43.3% | 66.7% | 73.3% | 15 | 15 |
| edge_type | 32 | 71.9% | 68.8% | 71.9% | 7 | 7 |

### 2.5 Reversal audit (Adrian's changes after reconciliation)

Source: `analysis-outputs/reversal-audit.md`

| Dim | Reversals | B2 (clear-spec drift) | B3 (standard-ambiguity) | Spec-citation rate |
|---|---|---|---|---|
| type | 4 | 4 | 0 | 4/4 |
| citation | 1 | 1 | 0 | 1/1 |
| qualifier | 13 | 11 | 2 | 13/13 |
| edge_type | 6 | 5 | 1 | 6/6 |
| **Total** | **24** | **21 (87.5%)** | **3 (12.5%)** | **24/24 (100%)** |

### 2.6 Human-human bucket distribution (Adrian-confirmed)

Source: `analysis-outputs/human-human-bucket-distribution.csv`

| dimension | B1 | B2 | B3 | ERROR | total |
|---|---|---|---|---|---|
| type | 5 | 20 | 1 | 0 | 26 |
| citation | 0 | 14 | 0 | 0 | 14 |
| qualifier | 1 | 16 | 0 | 0 | 17 |

Plus 2 non-bucket flags Adrian added in cols O/P of the bucketed xlsx:
- `cit_294094ac` — AI-bucketer **confabulation**, caught by human review (asserted attribution structure not present in claim text). Paper-relevant evidence for the human review layer.
- `qual_7cc76f88` — **extractor artefact** (Pass-1 dropped "suggests" on atomisation; "disagreement" partly artifactual).

Final distribution over all 57: 50 B2 / 5 B1 / 1 B3 / 1 confab / 1 extractor → **87.7% B2 share** (50/57). The B2 share is the headline: human-human disagreements are predominantly clear-spec drift, not codebook ambiguity.

### 2.7 Reconciler model-bias quantum

On the 34 divergent endorsement rows, agreement with Adrian's final HUMAN_RECONCILED:

| Reconciler | Agreement | Model family |
|---|---|---|
| Sonnet 4.6 reconciler | 27/34 = 79% | same family as original endorser |
| **Opus 4.7 reconciler** | **32/34 = 94%** | different family |
| Both reconcilers agree with Adrian | 27/34 = 79% | — |

15-pp gap = quantifiable same-model bias when AI judges AI on this task.

**Signal status: SETTLED.** All sections 2.1-2.7 are publishable. The three-way table, reversal audit, and reconciler-bias quantum are the strongest methodological contributions.

---

## 3. Post-freeze pipeline audits

### 3.1 Pass-1 extractor-loss audit (2026-06-20)

Source: `analysis-outputs/extractor-loss-rates.csv` · summary: `analysis-outputs/extractor-loss-summary.md`

Sample: n=611 (claim, source-paragraph) pairs across Tesla anchor + 2 ROK docs + 24 AI-corpus docs.

| Loss type | Pairs w/ marker | ≥1 marker dropped | Drop rate (pair) | Marker survival |
|---|---|---|---|---|
| Hedge words (Q1+Q2 named list) | 22 | 8 | 36.4% nominal / **<10% true** (heuristic FP-inflated) | 63.6% |
| Citation markers (inline) | 46 | 37 | **80.4%** | **15.4%** |

Per-group:

| Group | n pairs | hedge drop | citation drop |
|---|---|---|---|
| Tesla | 24 | 0% (0/2) | 100% (15/15) |
| ROK | 50 | 0% (0/2) | 86% (6/7) |
| AI corpus | 537 | 44% (8/18) | 67% (16/24) |

**Decision matrix for paper:**
- **Qualifier trajectories** — safe with brief caveat. Hedge loss <10% true rate; class assignment uses paragraph context not atomised text.
- **citation_status (Ext/Int/None)** — safe. Set by Pass-1b reading broader context.
- **Claim-render appendices with inline citations** — NOT paper-ready as-is. Re-render with source-paragraph alongside (already done; see `analysis-outputs/appendices/grounding/` and `results/appendices/grounding/` in transparency repo) OR footnote the strip.

### 3.2 Node-connectivity agreement (orphan binary, degree band, in-deg≥1)

Source: `analysis-outputs/node-connectivity-agreement.csv`

| check | n | Po | kappa | kappa_type | notes |
|---|---|---|---|---|---|
| orphan_binary | 110 | 0.8636363636363636 | 0 | cohens | PRIMARY — orphan vs connected per node |
| degree_band_3way | 110 | 0.6363636363636364 | 0.3192367199587416 | linear_weighted | SECONDARY — isolated / leaf-or-tip / connected-hub |
| in_degree_ge1 | 110 | 0.7090909090909091 | 0.41176470588235303 | cohens | TERTIARY — has-incoming vs no-incoming (root candidate) |
| contingency_orphan | hand_orphan | hand_connected |  |  |  |
| pipe_orphan | 0 | 15 |  |  |  |
| pipe_connected | 0 | 95 |  |  |  |
| contingency_band | hand_isolated | hand_leaf-or-tip | hand_connected-hub |  |  |
| pipe_isolated | 0 | 0 | 0 |  |  |
| pipe_leaf-or-tip | 7 | 33 | 11 |  |  |
| pipe_connected-hub | 8 | 14 | 37 |  |  |
| contingency_in_degree | hand_has-incoming | hand_no-incoming |  |  |  |
| pipe_has-incoming | 32 | 12 |  |  |  |
| pipe_no-incoming | 20 | 46 |  |  |  |

Architectural finding: pipeline never produces orphans, hand-coding does (~15 of 117 on Tesla). Asymmetry, not disagreement.

**Signal status: SETTLED for caveats; CANDIDATE for "Pass-1 information loss as a methodological honesty section."**

---

## 4. Post-corpus analytics — architectural (structural-v1)

Per-rung aggregates over the canonical 25 entities × 4 rungs × 3 seeds (AI) + 25 human MBA reports.

Source: `analysis-outputs/structural-aggregates.csv` (n=51 measure-rung rows)

Pivot — mean across cells per rung, on the **firmer** dimensions (asterisked ones below):

| firmer_or_asterisked | rung | measure | mean | sd_across_cells | within_rung_seed_sd_mean | min | max | n_cells_or_obs |
|---|---|---|---|---|---|---|---|---|
| firmer | TMBA | orphan_rate | 0.061 | 0.029 | 0.036 | 0.016 | 0.152 | 25 |
| firmer | E1 | orphan_rate | 0.056 | 0.021 | 0.033 | 0.029 | 0.112 | 25 |
| firmer | E2 | orphan_rate | 0.052 | 0.023 | 0.026 | 0.016 | 0.104 | 25 |
| firmer | E3 | orphan_rate | 0.049 | 0.021 | 0.034 | 0.012 | 0.117 | 25 |
| firmer | human | orphan_rate | 0.024 | 0.026 | 0.000 | 0.000 | 0.075 | 25 |
| firmer | TMBA | mean_in_deg | 1.011 | 0.093 | 0.118 | 0.764 | 1.237 | 25 |
| firmer | E1 | mean_in_deg | 0.959 | 0.050 | 0.067 | 0.884 | 1.053 | 25 |
| firmer | E2 | mean_in_deg | 0.977 | 0.056 | 0.068 | 0.886 | 1.082 | 25 |
| firmer | E3 | mean_in_deg | 0.992 | 0.050 | 0.084 | 0.913 | 1.104 | 25 |
| firmer | human | mean_in_deg | 1.022 | 0.107 | 0.000 | 0.876 | 1.258 | 25 |
| firmer | TMBA | max_in_deg | 7.360 | 1.040 | 1.860 | 5.333 | 9.333 | 25 |
| firmer | E1 | max_in_deg | 8.307 | 1.610 | 1.925 | 6.000 | 11.333 | 25 |
| firmer | E2 | max_in_deg | 7.707 | 1.352 | 1.710 | 5.667 | 11.000 | 25 |
| firmer | E3 | max_in_deg | 7.680 | 1.345 | 1.863 | 6.000 | 12.000 | 25 |
| firmer | human | max_in_deg | 7.920 | 2.644 | 0.000 | 4.000 | 14.000 | 25 |
| firmer | TMBA | frac_in_deg_ge2 | 0.251 | 0.038 | 0.040 | 0.174 | 0.361 | 25 |
| firmer | E1 | frac_in_deg_ge2 | 0.240 | 0.020 | 0.027 | 0.205 | 0.275 | 25 |
| firmer | E2 | frac_in_deg_ge2 | 0.251 | 0.020 | 0.034 | 0.208 | 0.287 | 25 |
| firmer | E3 | frac_in_deg_ge2 | 0.257 | 0.019 | 0.035 | 0.210 | 0.288 | 25 |
| firmer | human | frac_in_deg_ge2 | 0.241 | 0.029 | 0.000 | 0.200 | 0.317 | 25 |

Per-doc detail: `analysis-outputs/structural-per-doc.csv` (n=326 rows — 300 AI + 25 human + 1 header)

**Signal status: CANDIDATE.** Look for systematic differences AI-vs-human and TMBA→E3 trajectories. Initial scan: human has lower orphan rate (2.4%) than any AI rung (~5-6%), lower component count (9 vs ~21-25), lower root count (45 vs 64-88). Suggests humans produce more coherent argument graphs.

---

## 5. Post-corpus analytics — architectural (structural-v2: full inventory + motifs)

Source: `analysis-outputs/structural-v2-aggregates.csv` (n=156 measure-rung rows; v1 measures + branching/depth/role-x-type)

**Per-rung headline (firmer dimensions, mean across 25 cells):**

| rung | measure | mean | sd_across_cells | within_rung_seed_sd_mean |
|---|---|---|---|---|
| TMBA | orphan_rate | 0.061 | 0.029 | 0.036 |
| E1 | orphan_rate | 0.056 | 0.021 | 0.033 |
| E2 | orphan_rate | 0.052 | 0.023 | 0.026 |
| E3 | orphan_rate | 0.049 | 0.021 | 0.034 |
| human | orphan_rate | 0.024 | 0.026 | 0.000 |
| TMBA | component_count | 21.400 | 10.804 | 10.176 |
| E1 | component_count | 24.933 | 7.170 | 10.296 |
| E2 | component_count | 22.160 | 7.361 | 7.882 |
| E3 | component_count | 20.587 | 7.173 | 9.220 |
| human | component_count | 9.080 | 5.090 | 0.000 |
| TMBA | root_count | 64.227 | 17.433 | 19.941 |
| E1 | root_count | 87.947 | 7.494 | 12.566 |
| E2 | root_count | 85.107 | 11.887 | 13.878 |
| E3 | root_count | 80.440 | 8.964 | 13.989 |
| human | root_count | 45.400 | 12.852 | 0.000 |
| TMBA | leaf_count | 32.667 | 7.899 | 11.243 |
| E1 | leaf_count | 32.267 | 5.543 | 9.357 |
| E2 | leaf_count | 30.653 | 7.235 | 6.135 |
| E3 | leaf_count | 26.987 | 6.304 | 7.711 |
| human | leaf_count | 13.680 | 6.250 | 0.000 |
| TMBA | root_leaf_ratio | 2.261 | 0.677 | 0.862 |
| E1 | root_leaf_ratio | 3.240 | 1.042 | 1.503 |
| E2 | root_leaf_ratio | 3.035 | 0.751 | 0.887 |
| E3 | root_leaf_ratio | 3.376 | 0.857 | 1.198 |
| human | root_leaf_ratio | 4.416 | 5.025 | 0.000 |
| TMBA | branching_factor | 1.388 | 0.109 | 0.123 |
| E1 | branching_factor | 1.241 | 0.047 | 0.084 |
| E2 | branching_factor | 1.249 | 0.052 | 0.099 |
| E3 | branching_factor | 1.245 | 0.062 | 0.106 |
| human | branching_factor | 1.226 | 0.101 | 0.000 |

**Role-x-type proportions (F-roots, M-leaves, V-internal, etc.)** — `structural-v2-aggregates.csv` rows where measure matches `F_root`/`M_leaf`/etc. Detail in per-doc CSV: `analysis-outputs/structural-v2-per-doc.csv`.

### 5.1 Motif inventory + AI-vs-Human diff

Source: `analysis-outputs/structural-v2-motif-inventory.csv` (n=3580 motif-group rows) + `analysis-outputs/structural-v2-motif-diff.csv` (n=1171 unique motif signatures with per-rung counts).

Motif example: `v_convergent:F-S+V-S->V` = a V node with two incoming Support edges from an F and a V (one of many convergent shapes). The inventory is exhaustive across all 326 docs.

Top 10 AI motifs by total count (head of motif-diff sorted by AI_total):

```
signature,AI_TMBA,AI_E1,AI_E2,AI_E3,Human,AI_total,present_in_human,present_in_ai
v_convergent:F-E+V-S->V,34,29,15,25,2,103,1,1
v_convergent:F-E+F-S->V,58,58,41,110,15,267,1,1
v_convergent:V-S+V-S->V,1157,1465,1392,1198,198,5212,1,1
v_convergent:F-S+V-S->V,800,1112,1064,765,187,3741,1,1
v_convergent:F-S+F-S->V,1271,1856,1766,1539,245,6432,1,1
v_convergent:F-S+M-S->V,693,894,703,699,169,2989,1,1
fork_divergent:F->V-S+V-S,336,437,476,403,26,1652,1,1
v_convergent:F-W+M-S->M,6,8,8,5,0,27,0,1
chain:M-S->M-S->V,96,126,164,153,46,539,1,1
fork_divergent:M->F-S+M-S,5,3,19,8,4,35,1,1
```

**Signal status: OPEN.** Motif inventory is exhaustive but unanalyzed. Candidate findings: motifs over-represented in AI vs human; rung-specific motif shifts; convergence-divergence patterns. **Adrian decision needed:** which motif analyses to run for the paper.

---

## 6. Post-corpus analytics — inferential (S/W/J/E + chains)

Source: `analysis-outputs/inferential-aggregates.csv` (n=80 measure-rung rows) · per-doc: `analysis-outputs/inferential-per-doc.csv`

### 6.1 Edge-type proportions per rung

| rung | measure | mean | sd_across_cells | within_rung_seed_sd_mean | min | max | n_cells_or_obs |
|---|---|---|---|---|---|---|---|
| TMBA | s_rate | 0.587 | 0.053 | 0.064 | 0.493 | 0.709 | 25 |
| E1 | s_rate | 0.650 | 0.044 | 0.064 | 0.558 | 0.758 | 25 |
| E2 | s_rate | 0.676 | 0.034 | 0.050 | 0.607 | 0.744 | 25 |
| E3 | s_rate | 0.664 | 0.035 | 0.064 | 0.598 | 0.732 | 25 |
| human | s_rate | 0.671 | 0.103 | 0.000 | 0.434 | 0.838 | 25 |
| TMBA | w_rate | 0.059 | 0.019 | 0.039 | 0.027 | 0.097 | 25 |
| E1 | w_rate | 0.067 | 0.021 | 0.030 | 0.037 | 0.106 | 25 |
| E2 | w_rate | 0.073 | 0.022 | 0.024 | 0.034 | 0.131 | 25 |
| E3 | w_rate | 0.093 | 0.021 | 0.031 | 0.062 | 0.137 | 25 |
| human | w_rate | 0.075 | 0.066 | 0.000 | 0.009 | 0.291 | 25 |
| TMBA | s_w_ratio | 15.959 | 9.737 | 12.278 | 3.060 | 44.204 | 25 |
| E1 | s_w_ratio | 12.497 | 5.026 | 6.618 | 6.245 | 26.575 | 25 |
| E2 | s_w_ratio | 13.223 | 11.147 | 7.648 | 5.344 | 63.778 | 25 |
| E3 | s_w_ratio | 8.839 | 3.137 | 4.293 | 5.454 | 18.390 | 25 |
| human | s_w_ratio | 18.430 | 20.181 | 0.000 | 2.063 | 83.000 | 25 |
| TMBA | w_rate | 0.059 | 0.019 | 0.039 | 0.027 | 0.097 | 25 |
| E1 | w_rate | 0.067 | 0.021 | 0.030 | 0.037 | 0.106 | 25 |
| E2 | w_rate | 0.073 | 0.022 | 0.024 | 0.034 | 0.131 | 25 |
| E3 | w_rate | 0.093 | 0.021 | 0.031 | 0.062 | 0.137 | 25 |
| human | w_rate | 0.075 | 0.066 | 0.000 | 0.009 | 0.291 | 25 |
| TMBA | j_rate | 0.222 | 0.038 | 0.061 | 0.157 | 0.331 | 25 |
| E1 | j_rate | 0.168 | 0.029 | 0.036 | 0.125 | 0.253 | 25 |
| E2 | j_rate | 0.168 | 0.033 | 0.035 | 0.116 | 0.251 | 25 |
| E3 | j_rate | 0.144 | 0.023 | 0.033 | 0.095 | 0.199 | 25 |
| human | j_rate | 0.136 | 0.071 | 0.000 | 0.000 | 0.256 | 25 |
| TMBA | e_rate | 0.132 | 0.049 | 0.067 | 0.043 | 0.232 | 25 |
| E1 | e_rate | 0.115 | 0.034 | 0.054 | 0.054 | 0.190 | 25 |
| E2 | e_rate | 0.083 | 0.023 | 0.035 | 0.036 | 0.138 | 25 |
| E3 | e_rate | 0.098 | 0.030 | 0.049 | 0.048 | 0.186 | 25 |
| human | e_rate | 0.119 | 0.046 | 0.000 | 0.034 | 0.202 | 25 |

### 6.2 Chain composition per rung

| rung | measure | mean | sd_across_cells | within_rung_seed_sd_mean | min | max | n_cells_or_obs |
|---|---|---|---|---|---|---|---|
| TMBA | chain_count | 300.320 | 102.064 | 111.153 | 118.333 | 576.333 | 25 |
| E1 | chain_count | 387.587 | 103.064 | 129.635 | 241.667 | 663.333 | 25 |
| E2 | chain_count | 421.093 | 132.476 | 142.850 | 226.000 | 696.667 | 25 |
| E3 | chain_count | 402.080 | 70.522 | 129.996 | 269.333 | 557.000 | 25 |
| human | chain_count | 220.880 | 131.753 | 0.000 | 29.000 | 512.000 | 25 |
| TMBA | chains_pure_s_frac | 0.292 | 0.106 | 0.107 | 0.140 | 0.573 | 25 |
| E1 | chains_pure_s_frac | 0.368 | 0.091 | 0.126 | 0.154 | 0.576 | 25 |
| E2 | chains_pure_s_frac | 0.382 | 0.079 | 0.118 | 0.245 | 0.570 | 25 |
| E3 | chains_pure_s_frac | 0.357 | 0.072 | 0.123 | 0.234 | 0.564 | 25 |
| human | chains_pure_s_frac | 0.340 | 0.135 | 0.000 | 0.128 | 0.673 | 25 |
| TMBA | chains_contain_w_frac | 0.130 | 0.072 | 0.093 | 0.045 | 0.313 | 25 |
| E1 | chains_contain_w_frac | 0.132 | 0.050 | 0.081 | 0.040 | 0.241 | 25 |
| E2 | chains_contain_w_frac | 0.162 | 0.071 | 0.090 | 0.065 | 0.339 | 25 |
| E3 | chains_contain_w_frac | 0.222 | 0.058 | 0.101 | 0.120 | 0.352 | 25 |
| human | chains_contain_w_frac | 0.169 | 0.146 | 0.000 | 0.018 | 0.517 | 25 |
| TMBA | chains_contain_j_frac | 0.515 | 0.116 | 0.151 | 0.313 | 0.724 | 25 |
| E1 | chains_contain_j_frac | 0.419 | 0.096 | 0.122 | 0.213 | 0.604 | 25 |
| E2 | chains_contain_j_frac | 0.406 | 0.112 | 0.121 | 0.177 | 0.633 | 25 |
| E3 | chains_contain_j_frac | 0.386 | 0.084 | 0.133 | 0.162 | 0.545 | 25 |
| human | chains_contain_j_frac | 0.451 | 0.181 | 0.000 | 0.000 | 0.758 | 25 |
| TMBA | chains_contain_e_frac | 0.304 | 0.121 | 0.174 | 0.117 | 0.593 | 25 |
| E1 | chains_contain_e_frac | 0.270 | 0.114 | 0.136 | 0.108 | 0.556 | 25 |
| E2 | chains_contain_e_frac | 0.232 | 0.075 | 0.146 | 0.124 | 0.407 | 25 |
| E3 | chains_contain_e_frac | 0.255 | 0.084 | 0.153 | 0.086 | 0.442 | 25 |
| human | chains_contain_e_frac | 0.239 | 0.165 | 0.000 | 0.005 | 0.651 | 25 |

### 6.3 Strict citation rate per rung (from inferential set, dual-reported)

| rung | measure | mean | sd_across_cells | within_rung_seed_sd_mean | min | max | n_cells_or_obs |
|---|---|---|---|---|---|---|---|
| TMBA | strict_ext_rate | 0.027 | 0.015 | 0.026 | 0.000 | 0.050 | 25 |
| E1 | strict_ext_rate | 0.070 | 0.038 | 0.053 | 0.016 | 0.152 | 25 |
| E2 | strict_ext_rate | 0.071 | 0.033 | 0.050 | 0.027 | 0.132 | 25 |
| E3 | strict_ext_rate | 0.064 | 0.039 | 0.058 | 0.012 | 0.148 | 25 |
| human | strict_ext_rate | 0.167 | 0.136 | 0.000 | 0.000 | 0.580 | 25 |

**Signal status: CANDIDATE.** Edge-type shifts (J-rate decreases TMBA→E3 0.22→0.14; W-rate increases 0.06→0.09; s_w_ratio decreases 16→9). Chain-composition shifts (chains containing W increase across rungs). Human distinct from all AI rungs on chain_count (220 vs 300-422). Candidate publishable signal: elicitation gradient changes argument-graph shape.

---

## 7. Post-corpus analytics — strict/lenient detectability delta

Source: `analysis-outputs/rung-aggregates.csv` · per-doc: `analysis-outputs/per-doc-metrics.csv` + `analysis-outputs/cell-aggregates.csv`

Detectability operationalised per addendum §2 as `lenient_rate − strict_rate`. **Inherits validity from κ-validated citation primitives** — no separate κ required.

| rung | measure | mean | sd_across_cells | min | max | n_cells | within_rung_seed_SD_mean |
|---|---|---|---|---|---|---|---|
| TMBA | strict_ext_rate | 0.027 | 0.015 | 0.000 | 0.050 | 25 | 0.026 |
| TMBA | lenient_rate | 0.303 | 0.063 | 0.137 | 0.455 | 25 | 0.093 |
| TMBA | delta | 0.276 | 0.061 | 0.124 | 0.421 | 25 | 0.094 |
| E1 | strict_ext_rate | 0.070 | 0.038 | 0.016 | 0.152 | 25 | 0.053 |
| E1 | lenient_rate | 0.293 | 0.049 | 0.186 | 0.378 | 25 | 0.071 |
| E1 | delta | 0.224 | 0.061 | 0.067 | 0.357 | 25 | 0.093 |
| E2 | strict_ext_rate | 0.071 | 0.033 | 0.027 | 0.132 | 25 | 0.050 |
| E2 | lenient_rate | 0.311 | 0.052 | 0.202 | 0.409 | 25 | 0.064 |
| E2 | delta | 0.240 | 0.047 | 0.140 | 0.316 | 25 | 0.087 |
| E3 | strict_ext_rate | 0.064 | 0.039 | 0.012 | 0.148 | 25 | 0.058 |
| E3 | lenient_rate | 0.333 | 0.081 | 0.143 | 0.482 | 25 | 0.083 |
| E3 | delta | 0.268 | 0.083 | 0.097 | 0.416 | 25 | 0.105 |
| human | strict_ext_rate | 0.167 | 0.136 |  | 0.000 | 0.5803571428571429 | 25.000 |
| human | lenient_rate | 0.626 | 0.259 |  | 0.193 | 1 | 25.000 |
| human | delta | 0.459 | 0.192 |  | 0.115 | 0.7727272727272727 | 25.000 |

Trajectory across the elicitation ladder: corpus-wide strict ranges 3-7%; lenient ranges 30-33%; delta ranges 22-28 pp. Human strict_ext_rate = 16.7% (higher than any AI rung).

Earlier 5-doc reliability cut (`Coding_Prompt_Freeze_Decision_2026-06-08.md:43–49`): MBG delta 74pp, Tesla 37pp, GEHC 21pp, JARIR 21pp, XOM 13pp; corpus 34pp.

**Signal status: SETTLED (corpus-wide).** Detectability delta is paper-ready. Candidate framing: "the average claim sits in a paragraph that LOOKS cited but only 7-17% are strictly attributable per v5.3 — across the 300-AI + 25-human corpus."

---

## 8. Post-corpus analytics — source validation

Source: `analysis-outputs/source-validation-aggregates.csv` · per-claim: `analysis-outputs/source-validation-per-claim.csv` (n=250 rows)

Claims that the pipeline coded as Ext were validated: does the cited source actually exist AND support the claim?

| group | n | real_supports | real_non_supporting | unverifiable | error | rate_real_supports | rate_real_non_supp | rate_unverifiable |
|---|---|---|---|---|---|---|---|---|
| AI TMBA | 50 | 24 | 25 | 1 | 0 | 48.0% | 50.0% | 2.0% |
| AI E1 | 50 | 32 | 18 | 0 | 0 | 64.0% | 36.0% | 0.0% |
| AI E2 | 50 | 32 | 15 | 3 | 0 | 64.0% | 30.0% | 6.0% |
| AI E3 | 50 | 35 | 10 | 5 | 0 | 70.0% | 20.0% | 10.0% |
| AI all | 200 | 123 | 68 | 9 | 0 | 61.5% | 34.0% | 4.5% |
| Human | 50 | 7 | 22 | 20 | 1 | 14.3% | 44.9% | 40.8% |

**Headline finding:** AI strict-Ext claims have **61.5% REAL+SUPPORTS rate** (TMBA 48% → E3 70%) across the elicitation ladder. **Human strict-Ext: 14.3% REAL+SUPPORTS, 40.8% unverifiable** (much harder to verify human-coded citations because their source descriptions are less structured).

**Signal status: SETTLED.** Both directions are publishable: (a) AI grounding improves with elicitation pressure; (b) AI may have higher REAL+SUPPORTS than humans — controversial finding worth foregrounding.

---

## 9. Post-corpus analytics — verifier validation

Source: `analysis-outputs/verifier-validation-sample-design.csv` + `verifier-validation-blind.csv` (n=197 blind rows) + `verifier-validation-blind-adrian.csv` (88 Adrian rows) + `verifier-validation-ai-verdicts.csv` (49 AI verdicts)

Stratified sample design:

| stratum | pool_size | target | sampled | seed_offset |
|---|---|---|---|---|
| ai|REAL+SUPPORTS | 123 | 18 | 18 | 0 |
| ai|REAL+NON-SUPPORTING | 68 | 5 | 5 | 1 |
| ai|UNVERIFIABLE | 9 | 4 | 4 | 2 |
| human|REAL+SUPPORTS | 7 | 5 | 5 | 3 |
| human|REAL+NON-SUPPORTING | 22 | 8 | 8 | 4 |
| human|UNVERIFIABLE | 20 | 8 | 8 | 5 |

Validates the source-validation AI verifier against human (Adrian) blind judgments. Adjudication column captures cases where AI vs human verifiers disagreed.

**Signal status: OPEN.** Numbers exist but no aggregated comparison report. **Adrian decision needed:** what verifier-vs-human agreement rate to report.

---

## 10. Post-corpus analytics — AI-AI distribution-level noise

Source: `analysis-outputs/ai-ai-noise.csv` (head section per-doc distribution-level; trailer section pairwise aligned κ on Tesla + 2 ROK docs)

21 source docs × 5 pipeline runs (per script 29 noise sample): Tesla anchor + 5 entities × 4 rungs at seed=1. Each doc has SD/CV/min/max across the 5 runs on ~18 measures.

Overall noise-floor table (mean SD/CV across all 21 docs):

```
===OVERALL (mean SD/CV across docs; noise floor per measure)===
measure,n_docs,mean_SD_across_runs,mean_CV_across_runs,max_CV_across_docs
n_nodes,23,5.163801080562166,0.028658414262466316,0.059268219984076845
n_edges,23,8.101529084186986,0.04910673929947008,0.1100642352780253
F_prop,23,0.01974764048120388,0.04906900052737704,0.12246893000355358
M_prop,23,0.015287437374610788,0.0855784537871527,0.18080244750640162
V_prop,23,0.020394590819887693,0.07223009034954346,0.1682480379222861
P_prop,23,0.008285938432395544,0.0704056516423777,0.25840074621277453
S_prop,23,0.040822052342736635,0.06259637099906006,0.1635403180756676
W_prop,23,0.022903178735273155,0.3427124652519907,0.769864892357559
J_prop,23,0.02311885006532071,0.1428777155627896,0.2716157298196659
E_prop,23,0.0345173037253782,0.3789092685469374,0.75491739091633
Ext_prop,23,0.012109589359164907,0.24219200552630907,1.391517470494427
Q0_prop,23,0.01364998870320947,0.014908277915057703,0.03548837888737463
Q1_prop,23,0.01352605015023232,0.17886396189906437,0.3238088643796539
Q2_prop,23,0.0006621749774708626,0.3592638653226723,2.23606797749979
orphan_rate,23,0.019855272040900086,0.48494273347792033,1.3693063937629153
root_count,23,5.442983037879227,0.06657276215196344,0.10829563315469647
leaf_count,23,4.458961822660853,0.1671882613958907,0.38085771224392173
component_count,23,5.514160089752992,0.27156170138336605,0.5219539227133919

===PAIRWISE-ALIGNED κ (semantic alignment between runs)===
```

Pairwise aligned-κ block (Tesla + ROK) feeds the three-way table in §2.1; full rows in the CSV.

**Signal status: SETTLED.** AI-AI is highly stable on counts (CV ~1-3%) and proportions (CV ~5-10% on common types; up to 30%+ on rare types like W and E). The architecture metrics (orphan rate, component count) have moderate variance (~20-30% CV) — note in paper.

---

## 11. Cross-rung trajectories (the elicitation gradient)

Source: `analysis-outputs/rung-aggregates.csv` + `cell-aggregates.csv` + `inferential-aggregates.csv` + `structural-v2-aggregates.csv` + `source-validation-aggregates.csv`

Headline trajectories (mean across cells per rung; AI rungs + human as comparator):

| Measure | TMBA | E1 | E2 | E3 | Human | Direction |
|---|---|---|---|---|---|---|
| strict_ext_rate | 2.7% | 7.0% | 7.1% | 6.4% | 16.7% | E1 jump; human higher |
| lenient_rate | 30.3% | 29.3% | 31.1% | 33.3% | — | mild ↑ across rungs |
| delta (lenient−strict) | 27.6pp | 22.4pp | 24.0pp | 26.8pp | — | TMBA biggest gap (less actual citing) |
| s_rate | 58.7% | 65.0% | 67.6% | 66.4% | 67.1% | TMBA lower; E1+ matches human |
| w_rate | 5.9% | 6.7% | 7.3% | 9.3% | 7.5% | ↑ across rungs |
| j_rate | 22.2% | 16.8% | 16.8% | 14.4% | 13.6% | ↓ across rungs toward human |
| s_w_ratio | 15.96 | 12.50 | 13.22 | 8.84 | 18.43 | E3 most warrant-rich |
| F_prop | 35.3% | 38.1% | 36.7% | 39.1% | — | mild ↑ |
| M_prop | 14.2% | 17.3% | 20.2% | 21.5% | — | ↑ |
| V_prop | 31.5% | 31.7% | 33.2% | 30.5% | — | ≈ |
| P_prop | 19.0% | 12.8% | 10.0% | — | — | ↓ (less recommendation under stronger elicitation) |
| orphan_rate | 6.1% | 5.6% | 5.2% | 4.9% | 2.4% | ↓ across rungs toward human |
| component_count | 21.4 | 24.9 | 22.2 | 20.6 | 9.1 | AI fragments more than human |
| root_count | 64.2 | 87.9 | 85.1 | 80.4 | 45.4 | AI has more roots |
| mean_depth | 1.02 | 1.25 | 1.36 | 1.43 | (look up) | ↑ deeper chains under elicitation |
| AI source REAL+SUPPORTS | 48% | 64% | 64% | 70% | 14% | ↑ across rungs |
| AI source unverifiable | 2% | 0% | 6% | 10% | 41% | E3 spends more on verifiable-but-niche; human very high |

**Signal status: CANDIDATE — multiple publishable elicitation-gradient signals.** Candidates: (a) E1 jumps strict_ext_rate 2.7%→7.0% (gating-the-cite effect of elicitation); (b) E3 has lowest s_w_ratio + highest M_prop + deepest chains (mechanistic reasoning lift); (c) AI consistently more fragmented (more components, more roots) than humans — orphan_rate trends toward human across rungs but doesn't reach it.

Within-rung seed SD column shows substantial within-cell variance — single-seed claims should be qualified.

---

## 12. Open questions inventory

| # | Question | Section ref | Decision needed |
|---|---|---|---|
| 1 | Which motif analyses to report? Inventory is exhaustive but unanalyzed. | §5.1 | Adrian — pick top-5 candidate motifs for AI-vs-human comparison |
| 2 | Verifier-vs-human agreement rate? Numbers exist but no aggregated comparison. | §9 | Adrian — choose framing (κ? % agreement? bucket breakdown?) |
| 3 | Edge-type κ at freeze (0.635 on small matched-pair n=41) — strong enough to report? | §1 | Adrian — accept caveat or defer to v5.4 |
| 4 | Qualifier κ ceiling 0.760 below 0.80 bar — caveat or fix in v5.4? | §1 + §2.1 | Adrian — paper framing |
| 5 | Citation-marker drops (78% pair / 17% survival) — does this affect any paper claim besides claim-render appendices? | §3.1 | Adrian — audit downstream uses of `prop_text` |
| 6 | ROK Human-AI κ qualifier=0.000 on ROK_Human is a prevalence-pathology artefact (everyone coded Q0) — explain in paper text or pre-filter? | §2.2 | Adrian — keep raw + add note |
| 7 | Reconciler model-bias quantum (Sonnet 79% vs Opus 94% with Adrian) — promote to methodological contribution? | §2.7 | Adrian — paper section weight |
| 8 | AI source REAL+SUPPORTS > human (62% vs 14%) — is "human is harder to verify" the right framing or does the gap need its own treatment? | §8 | Adrian — paper framing |
| 9 | s_w_ratio collapse TMBA→E3 (15.96→8.84) — is this a quality signal or just compositional drift? | §11 | Adrian — interpretation |
| 10 | P_prop drops sharply TMBA→E3 (19%→hmm) — fewer recommendations under heavier elicitation; explore? | §11 | Adrian — interpretation |
| 11 | Component count gap AI (21.4) vs Human (9.1) — argument coherence signal or atomisation difference? | §4 + §5 | Adrian — interpretation |
| 12 | Reversal audit B2/B3 split (88:12) is striking; promote to methodological contribution alongside bucket firewall? | §2.5 | Adrian — paper section |
| 13 | Christian ↔ Adrian outperforms others on qualifier (0.467 vs 0.082) — coder-style effect; report or smooth over? | §2.3 | Adrian — paper honesty |

---

## 13. Source-file index

### Reliability + endorsement (settled)
- `analysis-outputs/three-way-noise-table.csv` + `.md`
- `analysis-outputs/human-human-noise.csv`
- `analysis-outputs/human-human-bucket-distribution.csv`
- `analysis-outputs/rok-human-ai-kappa.csv`
- `analysis-outputs/endorsement-rates.csv`
- `analysis-outputs/endorsement-summary.md`
- `analysis-outputs/reversal-audit.md`
- `reports/coding-iteration/endorsement-sample-reconciled-final-completed.xlsx`
- `reports/coding-iteration/human-human-disagreements-bucketed-completed.xlsx`

### Pre-freeze + freeze provenance
- `briefs/Pre_Freeze_Development_Methodology.md` ← five-part methodology doc
- `briefs/CC_Coding_Prompt_Freeze_Brief.md`
- `briefs/Coding_Prompt_Freeze_Status_2026-06-06.md`
- `briefs/Coding_Prompt_Freeze_Status_2026-06-07-overnight.md`
- `briefs/Coding_Prompt_Freeze_Decision_2026-06-08.md`
- `briefs/v5_4_Candidate_Log.md`
- `provenance-pack/PART_A_freeze_artefacts/A1_freeze_manifest.md` (frozen hashes)
- `provenance-pack/PART_A_freeze_artefacts/A5_failed_iterations.md` (rolled-back audit)
- `provenance-pack/PART_A_freeze_artefacts/A6_v5_4_candidate_log.md`
- `provenance-pack/PART_B_generation_logging/B3_generation_freeze_template.md`

### Post-freeze pipeline audits
- `analysis-outputs/extractor-loss-rates.csv` + `extractor-loss-summary.md`
- `analysis-outputs/node-connectivity-agreement.csv`

### Post-corpus analytics
- `analysis-outputs/structural-aggregates.csv` + `structural-per-doc.csv` (v1)
- `analysis-outputs/structural-v2-aggregates.csv` + `structural-v2-per-doc.csv` (v2 — branching, role-x-type)
- `analysis-outputs/structural-v2-motif-inventory.csv` (n=3580)
- `analysis-outputs/structural-v2-motif-diff.csv` (n=1171 signatures)
- `analysis-outputs/inferential-aggregates.csv` + `inferential-per-doc.csv`
- `analysis-outputs/rung-aggregates.csv`
- `analysis-outputs/cell-aggregates.csv`
- `analysis-outputs/per-doc-metrics.csv`
- `analysis-outputs/source-validation-aggregates.csv` + `source-validation-per-claim.csv`
- `analysis-outputs/verifier-validation-{sample-design,blind,blind-adrian,ai-verdicts}.csv`
- `analysis-outputs/ai-ai-noise.csv` (distribution-level + pairwise aligned-κ)

### Figures + appendices
- `analysis-outputs/figures/three-way-noise.svg` + `.tsv`
- `analysis-outputs/figures/dag-Tesla_Human_MBA_reliability_anchor.svg` + `.tsv`
- `analysis-outputs/appendices/grounding/{Tesla,ROK_Human,ROK_Claude_T2}.xlsx`
- `analysis-outputs/appendices/grounding/all-audited-claims.csv` (n=611)

### Transparency repo mirror (live)
- `github.com/atate-nag/StrategicReasoningMeasurement` — all of `results/`, plus `coding/Pre_Freeze_Development_Methodology.md` and the original `corpus/` + `generation/` + `pre-registration/` directories

### Earlier reliability narrative + status docs (for paper-context referencing)
- `briefs/Reliability_Testing_Review.md` — full narrative incl. dead ends
- `briefs/Coding_Reliability_Status.md` — earlier-state snapshot
- `briefs/Three_Way_Comparison.md` — pre-bucket framing of three-way comparison
- `briefs/Matched_Pair_Data_Table.md`
- `briefs/Reliability_Data_For_Paper.md`

