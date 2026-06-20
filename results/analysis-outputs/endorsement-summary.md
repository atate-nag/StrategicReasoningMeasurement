# Endorsement-rate results (Task 2)

Source: `endorsement-sample-reconciled-final-completed.xlsx`
Sample: 124 rows (≈30 per dimension, stratified across 4 dimensions and ~300 AI corpus docs + Tesla anchor, seed=44).

## Headline

| Dimension | n | Human-orig | AI-orig | Reconciled-final | Divergent |
|---|---|---|---|---|---|
| type | 32 | 87.5% | 84.4% | **87.5%** | 5/32 |
| citation | 30 | 60.0% | 73.3% | **63.3%** | 7/30 |
| qualifier | 30 | 43.3% | 66.7% | **73.3%** | 15/30 |
| edge_type | 32 | 71.9% | 68.8% | **71.9%** | 7/32 |

Reconciled-final = the verdict after Adrian reconciled the 34 divergent rows against the AI endorser and two independent AI reconcilers (Sonnet 4.6 + Opus 4.7). For non-divergent rows the agreed verdict stands.

## Reconciler-vs-Adrian post-hoc alignment

After Adrian filled HUMAN_RECONCILED:
- Sonnet 4.6 reconciler agreed with Adrian's final call: **27/34**
- Opus 4.7 reconciler agreed with Adrian's final call: **32/34**
- Both reconcilers agreed with Adrian's final call: **27/34**

This is informative for the methodology paper: the gap between Sonnet and Opus reconciler-to-human alignment quantifies the "same-model" bias an AI judge inherits from the AI endorser.

## Rejection-bucket (B1/B2/B3) tagging

The RejectBuckets sheet in the source xlsx is currently un-filled (53 rows pending). Per CC_Noise_Measurement_Brief.md §47, bucket-firewall is the qualitative reconciliation device and is separate from the headline endorsement rate, so this does not block Task 3.

## Files

- `analysis-outputs/endorsement-rates.csv` — per-dimension counts + rates, original + reconciled
- `reports/coding-iteration/endorsement-sample-reconciled-final-completed.xlsx` — full audit trail: original verdicts, both AI reconcilers, Adrian's reconciliation
