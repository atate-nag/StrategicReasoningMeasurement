# Pre-freeze development methodology — coding standard + generation prompt

**Date assembled:** 2026-06-20
**Scope:** documents the iterative methodology used to take (1) the v5.3 coding-prompt suite from working to frozen on 2026-06-08, and (2) the generation-prompt suite (TMBA / E1 / E2 / E3) from candidate to frozen on 2026-06-10. All numbers cited are pulled directly from the logs listed at the head of each section; nothing summarised from memory.

---

## A. Objective function optimised each iteration

### A.1 Coding-prompt freeze (v5.3)

**Source files:**
- `briefs/CC_Coding_Prompt_Freeze_Brief.md` §0, §1, §4–§6
- `provenance-pack/PART_A_freeze_artefacts/A5_failed_iterations.md` §"Regression gate"

**Objective** — the prompt was tuned to maximise *per-dimension agreement* between pipeline output and Adrian's hand-coding **only insofar as Adrian's hand-coding correctly applies v5.3**:

> "v5.3 is frozen and is the sole arbiter of correct coding — not Adrian's opinion, not CC's. We are tuning the *prompt* that makes the pipeline apply v5.3 faithfully. The target is agreement between pipeline output and Adrian's hand-coding, **but only insofar as Adrian's coding correctly applies v5.3**." — `CC_Coding_Prompt_Freeze_Brief.md:13–15`

**Reference** — the v5.3 codebook is the arbiter, NOT a coder:

> "Treat Adrian's coding as a **proxy for v5.3**, not as personal ground truth. Where Adrian and the AI disagree, the question is always 'what does v5.3 say,' resolved via the three buckets — not 'match Adrian.'" — `CC_Coding_Prompt_Freeze_Brief.md:127–130`

**No composite, no weighting** — each dimension is held to its own bar separately:

> "Agreement is computed and reported **per dimension, separately** … **No weighted composite score.** Each dimension is held to its own bar. A prompt change that lifts one dimension while regressing another does not 'net out' — the regression is a failure regardless of the gain elsewhere." — `CC_Coding_Prompt_Freeze_Brief.md:40–45`

**Three-bucket firewall** (the disagreement-routing rule that turns raw κ into a tunable signal):

> "(1) AI-error — the pipeline misapplied v5.3. → Fix the prompt. (2) Human-error — Adrian misapplied v5.3; the AI was right. → Change nothing … (3) Spec-ambiguity — both codings are defensible under v5.3 *as written*. → Change NOTHING now. Log it as a v5.4 candidate issue." — `CC_Coding_Prompt_Freeze_Brief.md:21–27`

### A.2 Generation-prompt freeze (TMBA / E1 / E2 / E3)

**Source files:**
- `provenance-pack/PART_B_generation_logging/B3_generation_freeze_template.md`
- `briefs/CC_Generation_Experiment_Brief.md` (referenced by B3)
- `briefs/CC_Generation_Experiment_Response.md`

**Objective** — the 4 prompts form a **literal-nesting elicitation ladder**: TMBA ⊂ E1 ⊂ E2 ⊂ E3. The ladder is the cross-rung variable; everything else (model, temperature, max_tokens, web search policy, max_uses) is held constant per `B3_generation_freeze_template.md:10–14, 17–33`.

**Reference for the symmetry control:** the human (student) condition. Each parameter held constant matches a student-condition feature:

> "Web access symmetry: confirmed. Students had unrestricted web access. Uniform `web_search_20260209 max_uses=8` [later raised to 50 uncapped] across all 4 rungs is the right symmetry choice. Length-limit symmetry: confirmed. Students had no length limit. `max_tokens: 16000` was originally chosen as non-binding for typical strategy doc length, then raised to `32000` on 2026-06-11" — `B3_generation_freeze_template.md:11–13`

The nesting-byte-verification gate is enforced by `scripts/generation/verify-nesting.ts` — `freeze-prompts.ts` will refuse to freeze if the literal nesting TMBA ⊂ E1 ⊂ E2 ⊂ E3 is broken (B3 §"Generated with").

---

## B. Per-iteration metric log (gradient trajectory)

### B.1 Coding-prompt iteration table (the rolled-back-iteration log)

**Primary source:** `briefs/Coding_Prompt_Freeze_Status_2026-06-06.md` §"Iteration history" (lines 52–66).

Table reproduced verbatim from that file (8 candidates; metrics shown are Δκ from `dev-baseline` on the accumulated corpus; 2-seed means where shown; single-run κ has ~0.06 SE at n≈118):

| Candidate | Change | Δκ_type | Δκ_cite | Δκ_qual | Verdict |
|---|---|---|---|---|---|
| `dev-baseline` | pre-fix Pass-1 (v5.3 already there) | 0.595 | 0.467 | 0.592 | baseline |
| `dev-atomic-v1` | + atomic splitting rule + N-to-N aligner | 0.657 | 0.420 | 0.566 | **adopted aligner** |
| `dev-cite-v1` | + 3-step citation test + worked examples | 0.630 | 0.436 | 0.567 | adopted |
| `dev-qual-v1` | + verbose Q1/Q2 expansion | -0.019 | +0.005 | -0.165 | **rolled back** (Q2 phantom) |
| `dev-qual-v2` | surgical Q1 patterns only | 0.621 | 0.437 | +0.045 | **adopted = gold candidate v1** |
| `dev-fine-v1` | + type rules D+E + cite end-of-paragraph + Int figures | -0.025 | +0.032 | +0.028 | rolled back (type regression) |
| `dev-fine-v2` | drop rule D, keep E + cite extras | -0.037 | +0.010 | -0.029 | rolled back (mixed) |
| `dev-qual-v3` | strong attribution-hedge rule (mechanical) | -0.012 | -0.001 | -0.150 | **rolled back** (Q1 over-fired 14→24) |
| `dev-qual-v4` | few-shot examples for hedge boundaries | -0.080 | +0.006 | +0.060 | **rolled back** (qual-gain doesn't justify type-loss) |

**Per-doc snapshot at gold-candidate (qual-v2)** — `Coding_Prompt_Freeze_Status_2026-06-06.md:22–46`:

| Doc | n | type κ | citation κ | qualifier κ | Note |
|---|---|---|---|---|---|
| Tesla | 118 | 0.803 | 0.877 | 0.663 | qualifier below 0.8 bar |
| MBG | 25 | 0.939 | undef (88% agree, skewed marginals) | 1.000 | unmeasurable cite |
| XOM | 25 | 0.829 | undef (72% agree, n=25) | 0.648 | adj. pending |

**Multi-seed stability** at gold candidate (Tesla, 3 seeds × qual-v2) — `A5_failed_iterations.md:14`:

> "Tesla type ±0.011, cite ±0.010, qual ±0.032 (3 seeds × Tesla on qual-v2). Any cross-doc Δ exceeding these bounds = regression."

**Overnight iterations 2026-06-07** (additional rounds attempted after qual-v2 was the gold candidate) — `briefs/Coding_Prompt_Freeze_Status_2026-06-07-overnight.md:11–34`:

| Attempt | Target | Result | Verdict |
|---|---|---|---|
| Pass-2 edge v1 (verbose rewrite + drift block) | edge_type κ | edge_type κ 0.637 → **0.142** (vocab collapse to 40×S + 1×W); edge_presence +0.045 | FAIL |
| Pass-2 edge v2 (surgical: proximity test only) | edge_type κ without v1 regression | edge_type κ 0.538 (-0.099); qualifier 0.598 (-0.065) | FAIL |
| Pass-1 cite-preservation (preserve inline `(Author, year)` in restated prop_text) | preserve markers without κ regression | Tesla Δ within ±1 SE → initially adopted; cross-doc revealed: MBG type -0.295 / XOM -0.239 / JARIR -0.087 / GEHC -0.188 | ROLLED BACK |

**Final κ at freeze (post-bucket-firewall correction)** — `briefs/Coding_Prompt_Freeze_Decision_2026-06-08.md:17–24`:

| Dim | Raw κ | Corrected κ (bucket firewall) | Bar | Status |
|---|---|---|---|---|
| Type | 0.787 (n=238) | **0.826** | ≥0.8 | ✓ cleared |
| Citation | 0.809 (n=238) | **0.845** | ≥0.8 | ✓ cleared |
| Qualifier | 0.664 (n=235) | 0.760 | ≥0.8 | -0.04 below; doc-specific ceiling on attribution-heavy Tesla |
| Edge presence | -0.519 (n=163) | 0.058 | — | moved negative→positive via correction |
| Edge type | 0.635 (n=41 matched) | 0.635 | — | small matched-pair n |

### B.2 Generation-prompt iteration log

**Source:** `B3_generation_freeze_template.md` §"Web search constraint — clarification" (lines 81–102), §"Truncation bug-fix" (lines 106–130).

Generation-prompt **prompt text** was frozen on 2026-06-10 without per-iteration tuning of content; the iterative work was on the **runtime parameters** (web search policy + max_tokens) that bracket the prompts:

| Decision | Pre-decision state | Post-decision state | Reason |
|---|---|---|---|
| Web search `max_uses` cap | 8 (inherited from `scripts/generate-batch1.ts`) | **50 (effectively uncapped)** — Adrian 2026-06-10 | Pilot showed max observed attempts = 49; cap=8 imposed a handicap students didn't face. Empirical re-pilot verified cap=2 enforces server-side regardless of attempt count. |
| Web-search metric logged | `web_search_uses` (legacy, ambiguous — counted ATTEMPTS not results) | dual: `usage.web_search_results` (load-bearing) + `usage.web_search_attempts` (diagnostic) | Pilot 2026-06-10 seed=1 produced attempt counts 18/29/49/31 misread as results. Re-pilot seed=2 corrected. |
| `max_tokens` per turn | 16000 (non-binding for typical strategy doc length) | **32000** — Adrian 2026-06-11 | 11 stub rungs (<500w) detected post-generation in 2 distinct failure modes: `max_tokens` truncation (3 TMBA) and `pause_turn` truncation (8 across E1/E2/E3). Both condition-correlated; rescues both. |

**Affected cells for the max_tokens fix (10 cells regenerated as full tight-window batches at 32K)** — `B3_generation_freeze_template.md:126`:

> "ENTITY_02_seed1, ENTITY_04_seed2, ENTITY_05_seed1, ENTITY_15_seed3, ENTITY_17_seed1, ENTITY_18_seed2, ENTITY_22_seed1, ENTITY_22_seed2, ENTITY_34_seed3, ENTITY_36_seed2 … Original batches tagged `superseded_by='truncation_bug_fix_2026-06-11'`. ENTITY_05_seed1 needed one further standalone retry (E3 stubbed again at 32K via early `pause_turn`); ENTITY_19_seed3 also required a retry for a TMBA stub the diagnostic missed (was 438w; below the 500w inclusion line but above the 200w diagnostic threshold). After all rescues: 75/75 cells × 4 rungs = 300 canonical AI docs, zero stubs."

---

## C. Freeze/gate thresholds — set before iterating

### C.1 Coding-prompt thresholds

**Source:** `briefs/CC_Coding_Prompt_Freeze_Brief.md` §6 (lines 152–159) — bars defined BEFORE iteration began (brief dated 2026-06-05; first iteration logged 2026-06-06).

> "The coding prompt is **frozen** when, per Adrian's judgement:
> - Phase A: document-level segmentation agreement clears its threshold, AND
> - Phase B: each dimension's claim-level agreement clears its bar, AND
> - the latest candidate shows no material regression across the accumulated corpus, AND
> - remaining disagreements are predominantly bucket 2 (human) or bucket 3 (v5.4-ambiguity), not bucket 1 (AI-error)."

**Per-dimension bars** were set by Adrian, in advance, at **κ ≥ 0.8**:
- Source: `Coding_Prompt_Freeze_Status_2026-06-06.md:22–26` (the per-dim table headers literally read "Bar (0.8)").
- The bar was binary per dimension — no weighting, no composite (per §1 of the brief).

**Regression gate** (numeric):

> "A candidate prompt is adopted only if it improves κ on the targeted dimension WITHOUT regression beyond multi-seed noise (±1 SE) on any other dimension OR document." — `A5_failed_iterations.md:11–12`, quoting `CC_Coding_Prompt_Freeze_Brief.md` §5.

**Cross-doc test corpus** specified upfront — `A5_failed_iterations.md:16`:

> "Tesla (full doc, n=117) + MBG/XOM/JARIR/GEHC (25-claim samples each). 5 docs total."

**Phase ordering** (Phase A gates Phase B) — `CC_Coding_Prompt_Freeze_Brief.md:56–94`. Pre-stated before any iteration ran.

### C.2 Generation-prompt thresholds

**Source:** `B3_generation_freeze_template.md` §"Pre-freeze checklist" (lines 213–223), §"Immutability rule" (lines 181–189).

Gates set *before* freeze (pre-freeze checklist):
1. All 5 prompt files present (`MBA_brief.txt`, `TMBA.txt`, `E1.txt`, `E2.txt`, `E3.txt`)
2. `verify-nesting.ts` passes (TMBA ⊂ E1 ⊂ E2 ⊂ E3 byte-for-byte) ← **hard gate**, enforced by `freeze-prompts.ts`
3. Web-access + length-limit symmetry verified with WBS team
4. Decisions confirmed: temperature 0.7, max_tokens 16000 (later raised), seed count 3
5. Matched-pair company list finalised + inclusion records written

**Immutability rule (post-freeze)**:

> "Any modification to any prompt file → new version + fresh corpus generation. … Don't tweak E3 verification directive in response to looking at early E3 outputs." — `B3_generation_freeze_template.md:182–187`

**Inclusion line** (set in advance, applied post-generation) — `B3_generation_freeze_template.md:126`:

> "11 stub rungs (<500 words) caused by per-turn ceiling truncation in two distinct failure modes … After all rescues: 75/75 cells × 4 rungs = 300 canonical AI docs, zero stubs."

The 500-word inclusion line was the predefined filter; the 200-word diagnostic threshold caught only the egregious stubs and one (438w ENTITY_19_seed3) escaped the diagnostic but was below the inclusion line.

---

## D. Version history — candidate ↔ freeze events

### D.1 Pipeline lineage

**Source:** `provenance-pack/PART_A_freeze_artefacts/A1_freeze_manifest.md:74–84`. Reproduced verbatim:

| Date | Event |
|---|---|
| 2026-05-23 | v5.1 provisional freeze (rolled forward) |
| Earlier | v5.2 errata applied (`briefs/Codebook_v5_2_Errata_for_Freeze.md`) |
| 2026-06-04 (approx) | v5.3 codebook freeze (`Codebook_v5_3_Spec.md` + `Codebook_v5_3_Errata_for_Freeze.md`) |
| 2026-06-05 | Pass-1b claim-level citation attribution shipped |
| 2026-06-06 | qual-v2 prompt expansion adopted after multi-doc test |
| 2026-06-07 (overnight) | 3 Pass-2 edge iterations + 1 Pass-1 cite-preservation iteration attempted; all rolled back (see `A5_failed_iterations.md`) |
| 2026-06-07 | ID-collision bug discovered; instrument rebuilt with semantic alignment |
| 2026-06-08 | Freeze decision finalised; provenance pack initiated (`briefs/Coding_Prompt_Freeze_Decision_2026-06-08.md`) |
| 2026-06-09 | A1 freeze manifest published |
| 2026-06-10 | Generation prompts frozen — git tag `generation-prompts-freeze-2026-06-10` |
| 2026-06-11 | Generation `max_tokens` raised 16000 → 32000 (per-turn ceiling only; bytes-identical prompts; 10 cells regenerated) |

### D.2 v5.3 → v5.4 candidate map

**Source:** `provenance-pack/PART_A_freeze_artefacts/A6_v5_4_candidate_log.md` consolidates 6 candidates derived from B3 (spec-ambiguity) reconciliations; `briefs/v5_4_Candidate_Log.md` lists the underlying 18 B3 reconciliation cases.

| v5.4 candidate (from A6) | What drove it | Memory pointer | v5.3 status |
|---|---|---|---|
| #1 Meta-claim exclusion | edge-batch findings, [[project_v5_3_candidates]] item #1 | [[project_v5_3_candidates]] | Logged; not patched |
| #2 §4.4 E-rule clarification | 15-Tesla-edge B3 cluster (`A6_v5_4_candidate_log.md:44`) | [[project_v5_4_E_rule_clarification]] | Logged; 3 Pass-2 prompt iterations tried + rolled back per A5 |
| #3 Multi-source compound atomicity | §2.2 atomicity gap with §3.2 single-source | [[project_v5_4_multi_citation_atomicity]] | Logged |
| #4 "Signs of X" as Q0 | refined view 2026-06-07 — verb-of-assertion test | [[project_v5_4_signs_of_q0]] | Logged; qual-v2 prompt's "signs of" inclusion queued for v5.4 rev |
| #5 P→P edges | v5.3 §4 vocab doesn't fit | [[project_v5_4_p_to_p_edges]] | Logged; no-edge convention preferred |
| #6 Same-source co-referential atoms | when atomic split → N atoms attributed to same source | [[project_v5_4_same_source_co_referential]] | Logged; no-edge convention |

`v5_4_Candidate_Log.md` (compiled 2026-06-06) breaks the underlying 18 B3 cases by dimension: citation:4, type:6, edge_presence:1, edge_type:2, qualifier:5 — each case lists the specific hand/pipeline disagreement, Adrian's adjudicator note, and a `_TBD_` field for the v5.4 clarification text.

### D.3 Generation-prompt freeze ID

**Source:** `B3_generation_freeze_template.md:23, 39–44`.

| File | SHA256 |
|---|---|
| `generation-prompts/MBA_brief.txt` | `b97ff22112b172b304fc25311b21bbcc8b4777e1d465b4e9c6e244dc1d186afb` |
| `generation-prompts/TMBA.txt` | `b97ff22112b172b304fc25311b21bbcc8b4777e1d465b4e9c6e244dc1d186afb` (= MBA_brief — verbatim student brief, confirms symmetry control) |
| `generation-prompts/E1.txt` | `a156a64df985f4ddbdce660c1fd408cabceea445b790fa151c2b496a6f90daec` |
| `generation-prompts/E2.txt` | `6ad8ef2dcd88e63c58d525895304fce98184366d765d04a10f5eab6f4f87b9d1` |
| `generation-prompts/E3.txt` | `8cf529b0aff3ae4654362526d7a0bf76020cbc9c4fbdf7e18806d271d9320822` |

Tag: `generation-prompts-freeze-2026-06-10`.

### D.4 Coding-prompt freeze hashes

**Source:** `A1_freeze_manifest.md:18–24`.

| File | SHA256 | Role |
|---|---|---|
| `src/lib/reasonqa/prompts/pass1.ts` | `bedb879aba09ca2146739bc3962899745699b25c893dd1856fccc45388f4361b` | Pass-1 node extraction + classification — qual-v2 baseline |
| `src/lib/reasonqa/prompts/pass2.ts` | `9b92e22c2871747b63ebd35afc9addae6be6b2246157cbdfbdd379679303fbcc` | Pass-2 edges — original baseline; all 4 edge iterations rolled back |
| `src/lib/reasonqa/prompts/appendices.ts` | `5967534abe5e0a2286f858209c0dfeaafb7ca9efc1b9b06fc3104f812405930d` | Atomic-split rule + type guidance |

Branch: `coding-prompt-freeze` · Commit: `ef8f8b65deaae0f899099314f9b37eab38b6e7ea` (`A1_freeze_manifest.md:14–15`).

---

## E. Rollback events — specific metric regressions

**Primary source:** `A5_failed_iterations.md` (full audit trail with per-iteration tables, mechanism post-mortems, and architectural finding).

### E.1 Pass-2 edge prompt v1 (rolled back 2026-06-07 overnight)

- **Goal:** address B1 cluster of E-overdraw cases (§4.4 "removing wouldn't weaken" test violations)
- **Change:** extensive rewrite of edge-type definitions in `pass2.ts` + new "DRIFT PATTERNS TO AVOID" block enumerating ~12 patterns
- **Regression triggered:** Tesla `edge_type κ` 0.637 → **0.142** (Δ = **−0.495**). Mechanism: vocabulary collapse — pipeline produced 40× S and 1× W only; J/W/E vocabulary lost. `edge_presence κ` +0.045 (marginal positive).
- **Post-mortem:** "the model anchored on the heavy guidance against E and collapsed to defaulting all edges to S. Demonstrates that Pass-2 edge prompts cannot be tightened on E without producing collateral effects on other edge types." (`A5_failed_iterations.md:37`)

### E.2 Pass-2 edge prompt v2 — surgical (rolled back 2026-06-07 overnight)

- **Goal:** same as v1 but minimal change — only add a single test paragraph about topical proximity
- **Change:** one paragraph added to E definition; everything else unchanged
- **Regression triggered:** Tesla `edge_type κ` 0.637 → 0.538 (Δ = −0.099, exceeded 1 SE of 0.598–0.659 multi-seed range); `qualifier κ` 0.663 → 0.598 (Δ = −0.065, likely alignment variance). `edge_presence κ` +0.045.
- **Verdict:** edge_type still regressed beyond 1 SE; qualifier showed unexplained drop → fail. (`A5_failed_iterations.md:50–57`)

### E.3 Pass-2 edge-v3 (rolled back 2026-06-07 morning)

- **Goal:** tighter E definition only — require active restatement/refinement, NOT mere topical proximity
- **Change:** ONE change: E definition expanded to require positive content relation; other edge types untouched
- **Regression triggered (Tesla, 2-seed mean):** `type κ` 0.788 → 0.738 (Δ = −0.050, exceeds 1 SE of 0.011); `citation κ` 0.885 → 0.856 (Δ = −0.029); `qualifier κ` 0.567 → 0.463 (Δ = **−0.104, >3 SE**)
- **Verdict:** all node dimensions regressed; qualifier most severely. Edge κ not directly measured (script 12 lacked `--with-edges` flag on this run) but node regression alone failed the gate. (`A5_failed_iterations.md:70–77`)

### E.4 Pass-1 citation preservation (initially adopted, then rolled back 2026-06-07)

- **Goal:** preserve inline parenthetical citations in pipeline's restated `prop_text` (Adrian's MBG/XOM observation: pipeline strips citations from claim text)
- **Change:** one-sentence addition to the "text:" field instruction in Pass-1 prompt
- **Initial Tesla result:** all Δ within ±1 SE — appeared safe → adopted
- **Cross-doc regression triggered** (the gate that broke the adoption — `A5_failed_iterations.md:94–101`):

| Doc | Dim | Baseline κ | After cite-preserve κ | Δ |
|---|---|---|---|---|
| MBG | type | 0.939 | 0.644 | **−0.295** |
| XOM | type | 0.829 | 0.590 | **−0.239** |
| JARIR | type | 0.943 | 0.856 | −0.087 |
| GEHC | type | 0.877 | 0.689 | **−0.188** |

- **Mechanism:** "citation markers in pipeline's `prop_text` shift the embedding fingerprint of the claim → semantic-aligner pairs different hand↔pipeline propositions → measured κ changes via pair-selection effect, not via classification change. The PIPELINE'S CLASSIFICATION decisions were likely unchanged, but the κ measurement is downstream of paired observations, so any alignment shift propagates to κ. By the regression-gate rule, measured κ is what matters." (`A5_failed_iterations.md:103–104`)
- **Verdict:** ROLLED BACK. The fix is genuinely needed for paper-facing claim renders but cannot live in the pipeline's `prop_text` — see [[project_v5_4_extractor_hedge_preservation]] for the paper-side mitigation (re-extract source paragraph alongside the atomised claim; see also `results/appendices/grounding/` in the transparency repo).

### E.5 Earlier Pass-1 rollbacks (logged in `Coding_Prompt_Freeze_Status_2026-06-06.md`)

| Iteration | Change | Regression triggered |
|---|---|---|
| `dev-qual-v1` | verbose Q1/Q2 expansion | qualifier Δκ −0.165 (Q2 phantom — model over-fired Q2) |
| `dev-fine-v1` | type rules D+E + cite end-of-paragraph + Int figures | type Δκ −0.025 (any type regression = fail per per-dim bar rule) |
| `dev-fine-v2` | drop rule D, keep E + cite extras | mixed Δs; type Δκ −0.037 |
| `dev-qual-v3` | strong attribution-hedge rule (mechanical) | qualifier Δκ −0.150 (Q1 over-fired 14→24); type Δκ −0.012 |
| `dev-qual-v4` | few-shot examples for hedge boundaries | type Δκ −0.080 vs qualifier Δκ +0.060 — qual gain didn't justify type loss per "no composite" rule |
| `qual-v2-int` (Int patterns for Appendix/Section/fig) | (mid-iteration attempt logged in 06-06 status) | qualifier Δκ −0.155 mean across 2 seeds → rolled back. "Third confirmation that any Pass-1b prompt expansion regresses an unrelated dimension via attention dilution." (`Coding_Prompt_Freeze_Status_2026-06-06.md:46`) |

### E.6 Architectural finding from the rollback pattern

**Source:** `A5_failed_iterations.md:108–128`. The four overnight rollbacks share a common mechanism:

> "Pass-2 prompt change → marginal Pass-2 output change → embedding shift on Pass-1 prop_text → different paired observations chosen by semantic-aligner → different κ on Pass-1 dimensions"

> "Implication: edge-tightening cannot be done at the v5.3 prompt-iteration level without breaking the regression gate. The mechanical fix paths are: (1) 2-stage Pass-2 redesign — separate edge-presence decision from edge-type; (2) Post-processing E-filter — deterministic, not prompt-dependent; (3) Move to v5.4 prompt rev with codebook clarifications baked in. Both architectural paths (1, 2) deferred to post-freeze work. Path 3 is the documented direction."

### E.7 Generation-prompt rollbacks

Generation prompts had **no content rollbacks** — the freeze process iterated on runtime parameters (web search, max_tokens), not on prompt content. The two rollback-equivalent events:

| Event | Trigger | Rollback / mitigation |
|---|---|---|
| Web search `max_uses=8` → 50 | Pilot 2026-06-10 showed model hitting cap repeatedly (max attempts = 49); cap inflated cost via wasted output tokens on blocked attempts (`B3_generation_freeze_template.md:91–94`) | Cap raised to 50 (effectively uncapped). Affected: all subsequent generation. Pilot seed=1 retained for audit; canonical seed=2 generations under new policy. |
| `max_tokens=16000` per turn truncated 11 stubs out of 300 cells | Post-generation diagnostic identified 3 TMBA `max_tokens` stubs + 8 `pause_turn` stubs across E1/E2/E3 (`B3_generation_freeze_template.md:106–115`) | Per-turn ceiling raised to 32000 (no prompt change, no design change). 10 cells regenerated; original cells tagged `superseded_by='truncation_bug_fix_2026-06-11'`. ENTITY_05_seed1 needed one further retry; ENTITY_19_seed3 retried for a 438w TMBA stub below the 500w inclusion line. Final: 75/75 cells × 4 rungs = 300 canonical AI docs, zero stubs. |

---

## Source-file index (everything cited)

| File | Role |
|---|---|
| `briefs/CC_Coding_Prompt_Freeze_Brief.md` | The pre-iteration brief defining objective, bars, three-bucket firewall, regression gate, phase ordering, no-composite rule |
| `briefs/CC_Coding_Prompt_Freeze_Addendum.md` | Addendum (detectability/strict-lenient delta framing — referenced in 06-07 overnight log) |
| `briefs/Coding_Prompt_Freeze_Status_2026-06-06.md` | Per-iteration metric log up through `dev-qual-v2` adoption (Iteration history table) |
| `briefs/Coding_Prompt_Freeze_Status_2026-06-07-overnight.md` | Overnight iteration log (3 Pass-2 edge + 1 Pass-1 cite-preserve, all rolled back) |
| `briefs/Coding_Prompt_Freeze_Decision_2026-06-08.md` | Final freeze decision + corrected-κ table + v5.4 candidate enumeration |
| `briefs/v5_4_Candidate_Log.md` | 18 B3 reconciliation cases by dimension (citation:4, type:6, edge_presence:1, edge_type:2, qualifier:5) |
| `briefs/Codebook_v5_3_Spec.md` | The v5.3 codebook — the arbiter of correctness |
| `briefs/Codebook_v5_3_Errata_for_Freeze.md` + `Codebook_v5_2_Errata_for_Freeze.md` | Codebook errata applied pre-freeze |
| `provenance-pack/PART_A_freeze_artefacts/A1_freeze_manifest.md` | Frozen prompt + support file hashes, pipeline lineage, immutability rule |
| `provenance-pack/PART_A_freeze_artefacts/A5_failed_iterations.md` | The rolled-back-iteration log with per-iteration tables, mechanism post-mortems, architectural finding |
| `provenance-pack/PART_A_freeze_artefacts/A6_v5_4_candidate_log.md` | Consolidated v5.4 codebook candidate log (6 candidates) |
| `provenance-pack/PART_B_generation_logging/B3_generation_freeze_template.md` | Generation prompts freeze manifest (hashes, symmetry verifications, web-search policy decisions, truncation bug-fix) |
| `scripts/verify-codebook-freeze.ts` | Codebook freeze verification |
| `scripts/generation/freeze-prompts.ts` | Generation-prompt freezer (refuses to freeze unless byte-nesting passes) |
