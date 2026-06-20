# Three-way noise table — Tesla + ROK side-by-side

All κ measured **against the codebook v5.3 standard**. No coder is treated as ground truth.

## Headline (Cohen κ + Fleiss κ for 3-coder)

| Sample | Noise source | n | type | citation | qualifier |
|---|---|---|---|---|---|
| **Tesla** anchor | AI–AI (5 runs × 10 pairs) | ~114 | **0.946** | **0.930** | **0.867** |
| Tesla | Human–AI (Adrian v5.4 vs pipeline) | 111 | **0.821** | **0.889** | **0.685** |
| **ROK** instrument | AI–AI (2 docs × 5 runs × 10 pairs avg) | ~110 | **0.849** | **0.959** | **0.854** |
| ROK | Human–AI (mean of 3 humans vs pipeline-run1) | 48 | **0.498** | **0.482** | **0.206** |
| ROK | Human–Human Fleiss (Adrian/Sotirios/Christian) | 48 | **0.434** | **0.310** | **0.150** |

## The pattern holds within-sample on ROK

The brief's anticipated direction (AI-AI > Human-AI > Human-Human) is supported on ALL three dimensions, on BOTH samples, with the ROK sample controlling for document heterogeneity:

| Dim | Tesla AI-AI | Tesla H-AI | ROK AI-AI | ROK H-AI | ROK H-H |
|---|---|---|---|---|---|
| type | 0.946 | 0.821 | **0.849** | **0.498** | **0.434** |
| citation | 0.930 | 0.889 | **0.959** | **0.482** | **0.310** |
| qualifier | 0.867 | 0.685 | **0.854** | **0.206** | **0.150** |

Reading the bolded ROK columns left-to-right: AI-AI dominates Human-AI dominates Human-Human on all three dimensions. The Tesla columns (left) sit above the corresponding ROK columns — Tesla is the easier sample for everyone, including the pipeline.

## ROK Human-AI per-coder (preserved, not collapsed)

| Coder vs Pipeline | n | type | citation | qualifier |
|---|---|---|---|---|
| pipeline ↔ Adrian | 48 | 0.447 | 0.621 | 0.368 |
| pipeline ↔ Sotirios | 50 | 0.489 | 0.462 | 0.122 |
| pipeline ↔ Christian | 50 | 0.559 | 0.362 | 0.127 |

## ROK Human-Human pairwise (preserved)

| Pair | n | type | citation | qualifier |
|---|---|---|---|---|
| Adrian ↔ Sotirios | 48 | 0.458 | 0.347 | 0.160 |
| Adrian ↔ Christian | 48 | 0.517 | 0.416 | 0.467 |
| Sotirios ↔ Christian | 50 | 0.389 | 0.240 | 0.082 |

## Caveats (do not omit)

1. **Tesla sample is more homogeneous than ROK.** Tesla is one carefully-constructed reliability anchor; ROK is 2 documents (1 human MBA, 1 AI-generated) sampled into 50 claim items. The ROK columns are the within-sample-matched comparison; Tesla columns are kept for continuity with prior work but live on a different document.
2. **Prevalence pathology** affects ROK citation (None=83%) and qualifier (Q0=83%), deflating Cohen κ even when raters agree often (e.g., qualifier P↔A/S/C on ROK_Human all = 0.000 because everyone coded everything Q0). PABAK / Gwet AC1 would dampen this; the brief specified comparable coefficients, so Cohen κ stands.
3. **ROK Human-AI κs are lower than Tesla Human-AI κs.** Three contributors: (a) ROK pipeline atomization vs pre-curated ROK items requires semantic alignment, introducing alignment noise; (b) ROK pulls in 2 noisier coders (Sotirios, Christian) in addition to Adrian; (c) ROK documents are more heterogeneous content. The order of magnitude is what is comparable, not the absolute level.
4. **Edge dimensions excluded** per "firm dimensions" scope. Tesla edge κ in script 26 stdout (0.606 on 49 matched edges; asterisked per brief).
5. **Human-Human is un-reconciled by design** — the 57 ROK disagreements are surfaced + bucketed in `reports/coding-iteration/human-human-disagreements-bucketed-completed.xlsx`. Bucket distribution over all 57: **50 B2 (coder-error-vs-clear-rule) / 5 B1 (genuine disagreement) / 1 B3 (standard-ambiguity) / 1 AI-bucketer-confabulation / 1 extractor-artefact** — i.e. **88% B2 share** (50/57). The 88% B2 finding is the headline: when humans disagree on these dimensions, the codebook usually has a clear rule that someone drifted from — mirroring the reversal-audit finding from the endorsement work.

   The 2 non-bucket cases are findings within the set (kept in the denominator; not excluded to raise the share):

   (a) **`cit_294094ac` — AI bucketer confabulation, caught by human review.** Opus 4.7 asserted the claim "attributes a quoted statement to Blake Moret's Q3 FY2025 statement", concluding Adrian/Christian's None codings violated §3.2. Adrian inspected and found neither the claim text nor the source paragraph actually contains the FY2025-statement attribution — the AI bucketer fabricated the textual structure that warranted its B2 verdict. **Paper-relevant evidence: the standard-anchored verification architecture WITH a human review layer caught an AI verification error that the AI alone would have signed off on.** This is exactly the failure mode the architecture is designed to absorb.

   (b) **`qual_7cc76f88` — extractor artefact.** Pass-1 atomisation dropped the hedge word "suggests" when extracting the claim from a source paragraph that contained "...suggesting the strategic clock is running." Sotirios coded Q1 reading the paragraph context; Adrian/Christian coded Q0 reading the bare extracted claim. The "disagreement" is partly artifactual. Follow-up audit (`analysis-outputs/extractor-loss-summary.md`, n=611 sampled atomic claims across Tesla + ROK + 24 AI corpus docs) quantified Pass-1 marker preservation: **hedge-word drop rate is likely <10%** (apparent 33% is mostly heuristic false positives where the source sentence atomised across multiple sub-claims and the hedge attached to a sibling); this is non-trivial but small, so qualifier trajectories warrant only a brief caveat rather than re-extraction. **Citation-marker drop rate is 78% pair-level / 17% marker survival** — inline citation markers are systematically stripped during atomisation. Because `citation_status` (Ext/Int/None) is assigned by Pass-1b reading the broader context, the status field is intact, but any paper appendix that quotes atomised claims with their inline markers will read as None even when status=Ext. Recommended: for claim renders in the paper, either re-render with the source-paragraph snippet alongside OR footnote that inline markers were stripped during atomisation.
6. **Adrian's ROK session at 80/85 (in_progress)** at compute time; ROK Adrian pairs run on n≤48 of 50 claim items.

## Files

- `analysis-outputs/three-way-noise-table.csv` — this table, machine-readable
- `analysis-outputs/three-way-noise-table.md` — this document
- `analysis-outputs/ai-ai-noise.csv` — Tesla + ROK PAIRWISE-ALIGNED κ rows
- `analysis-outputs/human-human-noise.csv` — ROK Fleiss + pairwise H-H
- `analysis-outputs/rok-human-ai-kappa.csv` — ROK pipeline-vs-each-coder κ
- `reports/coding-iteration/human-human-disagreements-bucketed.xlsx` — 57 disagreements + B1/B2/B3
- `analysis-outputs/human-human-bucket-distribution.csv` — bucket counts per dimension
