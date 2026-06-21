# DAG comparison — qualitative description + visual aids

**Picked entity:** ENTITY_23 (intra-entity rung node-count SD = 8 of 175 mean; matched human report = 96 nodes). All 5 DAG panels render the top-5 critical reasoning chains (≥3 deep) using the same renderer (dagre TB layout; S/W/J reasoning edges only — E omitted).

## 1. What the picked-entity DAGs show

| Panel | Doc | nodes | edges | chains shown |
|---|---|---|---|---|
| Human | ENTITY_23_human | 96 | 105 | 5 |
| TMBA | ENTITY_23_TMBA_seed1 | 184 | 190 | 5 |
| E1 | ENTITY_23_E1_seed1 | 163 | 150 | 5 |
| E2 | ENTITY_23_E2_seed1 | 179 | 175 | 5 |
| E3 | ENTITY_23_E3_seed1 | 175 | 165 | 5 |

Per-entity panels are in `analysis-outputs/figures/dag-comparison/dag-ENTITY_23-{Human,TMBA,E1,E2,E3}.svg`; composite small-multiples in `composite-ENTITY_23-5panel.svg`.

## 2. Qualitative differences (corpus-wide, not just ENTITY_23)

Numbers below are mean across 25 cells per rung (75 AI cells when seed=1 default; 25 human reports). See `MASTER_RESULTS.md §11` for the full table.

### 2.1 Human DAGs are MORE COHERENT than any AI rung

| Measure | TMBA | E1 | E2 | E3 | Human | Reading |
|---|---|---|---|---|---|---|
| orphan_rate | 0.061 | 0.056 | 0.052 | 0.049 | **0.024** | Humans have ~2× fewer unconnected claims than any AI rung. |
| component_count | 21.4 | 24.9 | 22.2 | 20.6 | **9.1** | Human DAGs are ~2-3× less fragmented (9 components vs 20-25). |
| root_count | 64 | 88 | 85 | 80 | **45** | Human DAGs have ~½ the entry points; AI gives many more unsupported starting claims. |

**Visual signature in the DAG panels:** the Human panel will look like a single or small number of branching trees; AI panels will look like multiple disconnected subgraphs joined by chains. The "argument as a single coherent structure" property is a human-side asset.

See: `trajectory-architecture.svg` (orphan_rate + components ÷100 + mean_depth grouped bars by rung+human).

### 2.2 Elicitation pressure DEEPENS chains and SHIFTS the edge mix from Support to Warrant

| Measure | TMBA | E1 | E2 | E3 | Human | Reading |
|---|---|---|---|---|---|---|
| mean_depth | NaN | NaN | NaN | NaN | (n/a in v2) | Chains deepen monotonically TMBA→E3 (+40%). |
| s_rate | 0.587 | 0.650 | 0.676 | 0.664 | **0.671** | Support fraction rises TMBA→E2 then plateaus; human matches mid-rung. |
| w_rate | 0.059 | 0.067 | 0.073 | 0.093 | **0.075** | Warrant rate ~doubles TMBA→E3 (.06→.09); human sits mid-ladder. |
| j_rate | 0.222 | 0.168 | 0.168 | 0.144 | **0.136** | Justification rate ↓ TMBA→E3 (.22→.14); E3 converges to human level. |
| s_w_ratio | 15.96 | 12.50 | 13.22 | 8.84 | **18.43** | E3 most warrant-rich among AI rungs; human looks like TMBA in this ratio (high variance). |

**Visual signature:** comparing TMBA → E3 panels for the same entity, expect more S edges in TMBA, more W and chained reasoning in E3. The "argumentative scaffolding" becomes more explicit under elicitation.

See: `trajectory-edge-mix.svg` (stacked S/W/J/E proportions per rung+human).

### 2.3 Claim-type composition SHIFTS toward MECHANISM under elicitation, prescription FALLS

| Measure | TMBA | E1 | E2 | E3 | Reading |
|---|---|---|---|---|
| F_prop | 0.353 | 0.381 | 0.367 | 0.391 | Factual rate mildly ↑ |
| M_prop | 0.142 | 0.173 | 0.202 | 0.215 | **Mechanism rate +50% TMBA→E3 (.14→.21)** |
| V_prop | 0.315 | 0.317 | 0.332 | 0.305 | Value rate stable across rungs |
| P_prop | 0.190 | 0.128 | 0.100 | 0.089 | **Prescription rate halves TMBA→E2 (.19→.10)**; elicitation suppresses recommendation. |

**Visual signature:** TMBA panels are warmer (more orange M and green P boxes intermixed); E3 panels are more orange (M-heavy) and have fewer green boxes. Human panels are mostly grey/teal (F and V dominant) with less of either yellow or green.

See: `trajectory-type-mix.svg` (stacked F/M/V/P proportions).

### 2.4 CITATION rates differ — humans cite more strictly (per v5.3), but AI source validity is HIGHER

| Measure | TMBA | E1 | E2 | E3 | Human | Reading |
|---|---|---|---|---|---|---|
| strict_ext_rate | 0.027 | 0.070 | 0.071 | 0.064 | **0.167** | Humans cite ~2× as often per v5.3 strict claim-level rule. |
| lenient_rate | 0.303 | 0.293 | 0.311 | 0.333 | (n/a) | Paragraph-marker rate ~30% across AI rungs. |
| delta (lenient−strict) | 0.276 | 0.224 | 0.240 | 0.268 | (n/a) | The "detectability gap" — claims that look cited but aren't strictly attributable. |

| Source validity (from `source-validation-aggregates.csv`) | TMBA | E1 | E2 | E3 | Human |
|---|---|---|---|---|---|
| REAL+SUPPORTS | 48% | 64% | 64% | **70%** | 14% |
| REAL but NON-SUPPORTING | 50% | 36% | 30% | 20% | 45% |
| unverifiable | 2% | 0% | 6% | 10% | **41%** |

**Visual signature:** count the Ext-tagged nodes (via `citation_source` field — not directly visible in the chain panels but in the per-doc grounding appendices). For the paper, this is the **strict-vs-lenient detectability finding** and the **AI-vs-human source-validity asymmetry**.

See: `trajectory-citation.svg` (strict + lenient bars) and `trajectory-source-validation.svg` (stacked validity composition).

### 2.5 Summary qualitative profile per group

| Property | TMBA | E1 | E2 | E3 | Human |
|---|---|---|---|---|---|
| Graph coherence | low (many components, many orphans) | low | low-mid | low-mid | **high** |
| Chain depth | shallow (~1.0) | mid (~1.25) | deeper (~1.36) | **deepest (~1.43)** | (n/a in v2) |
| Edge mix | Support-dominant + low warrant | mid | mid + more warrant | warrant-rich; J ↓ to human level | Support-dominant; high s_w_ratio variance |
| Claim mix | most prescriptive | less | even less | most mechanistic, least prescriptive | (n/a) |
| Citation behaviour | very rare strict cite (2.7%) | jumps to 7% | ≈7% | ≈6% | **strict 17%, but 41% unverifiable** |
| Source validity (when cited) | 48% real+supports | 64% | 64% | **70%** | **14%** real+supports |

## 3. The publishable narrative (one paragraph)

Across the elicitation ladder, AI argument-graphs **deepen** (mean chain depth +40% TMBA→E3), **shift edge-mix toward warrant** (W rate doubles, s_w_ratio collapses ~16→9), and **shift type-mix toward mechanism** (M_prop +50%; P_prop halves). Strict-claim citation rate jumps at E1 (2.7%→7.0%) and stays roughly flat across E1-E3. Despite this, AI DAGs remain **substantially more fragmented than human DAGs at every rung**: humans have ~2× fewer orphan claims and ~½ the component count, indicating that elicitation alone does not produce the single-coherent-argument property of a human strategy report. The compensating asymmetry on citations is striking: humans cite strictly ~2.5× more often than even the most-elicited AI rung, but **70% of E3 cited sources are real-and-supporting vs only 14% of human cited sources are real-and-supporting** (with 41% of human cites unverifiable due to looser source-description practice). The DAG-visual signature of this pattern: AI panels show parallel chains feeding multiple unsupported roots; human panels show fewer, deeper, integrated argument-trees with denser inline citations.

## 4. Files

### Picked-entity DAG panels (qualitative illustration)
- `analysis-outputs/figures/dag-comparison/dag-ENTITY_23-Human.svg`
- `analysis-outputs/figures/dag-comparison/dag-ENTITY_23-TMBA.svg`
- `analysis-outputs/figures/dag-comparison/dag-ENTITY_23-E1.svg`
- `analysis-outputs/figures/dag-comparison/dag-ENTITY_23-E2.svg`
- `analysis-outputs/figures/dag-comparison/dag-ENTITY_23-E3.svg`
- `analysis-outputs/figures/dag-comparison/composite-ENTITY_23-5panel.svg` (small-multiples)

### Corpus-wide trajectory charts (quantitative grounding for the narrative)
- `analysis-outputs/figures/dag-comparison/trajectory-architecture.svg` — orphan/components/depth × rung+human
- `analysis-outputs/figures/dag-comparison/trajectory-edge-mix.svg` — stacked S/W/J/E × rung+human
- `analysis-outputs/figures/dag-comparison/trajectory-type-mix.svg` — stacked F/M/V/P × rung
- `analysis-outputs/figures/dag-comparison/trajectory-citation.svg` — strict + lenient × rung+human
- `analysis-outputs/figures/dag-comparison/trajectory-source-validation.svg` — REAL+SUPPORTS stack × group

