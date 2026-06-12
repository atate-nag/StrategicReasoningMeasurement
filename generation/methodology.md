# Generation Methodology

The Strategic Theatre corpus is a set of AI-generated strategy case studies produced under four elicitation conditions (TMBA / E1 / E2 / E3) varying in the **bindingness of demand** for sourcing, connection, and explicit warrant. The design lets researchers characterise how a single model's reasoning architecture responds across an elicitation gradient.

## Conditions (the bindingness ladder)

The four conditions share the same student-facing MBA brief (the TMBA text) but vary in the bindingness of an additional instruction layered on top:

| Rung | Instruction | Definition |
|---|---|---|
| **TMBA** | (none beyond brief) | The student brief alone — minimal scaffolding. |
| **E1** | named | Names the pillars (sourcing, connection, warrant) without elaborating them. |
| **E2** | specified | Specifies each pillar (what counts as sourced; what counts as connected; what counts as a warrant). |
| **E3** | enforced | Specifies + adds an in-prompt verification directive: produce the analysis, then verify each pillar was satisfied; revise where not. Single-pass. |

E3's verification is enforcement of the instruction within a single pass, not a cognitive technique like chain-of-thought. The ladder dimension is bindingness of demand, not reasoning depth.

## Other generation parameters (frozen at the freeze date)

| Parameter | Value |
|---|---|
| Model | claude-sonnet-4-6 |
| Provider routing | Direct Anthropic SDK (synthetic seed; no human-sensitive content sent) |
| Temperature | 0.7 |
| max_tokens (per turn) | 32000 |
| Web search tool | `web_search_20260209`, `max_uses=50` (effectively uncapped) |
| Number of seeds per (entity × rung) | 3 |
| Tight-window protocol | All 4 rungs for one `(entity × seed)` generated back-to-back within minutes, to hold web-state constant across the rung comparison |

## max_tokens — scoped per-turn-ceiling bug-fix on 2026-06-11

The initial value was 16000. After generation completed for all 75 cells, a diagnostic identified 11 stub rungs (<500 words) caused by per-turn truncation in two condition-correlated failure modes (a `max_tokens` mode and an Anthropic-server-side `pause_turn` mode triggered by very large input context). The per-turn ceiling was raised from 16000 to 32000 — a runtime parameter change only, with no change to prompt content, web-search policy, ladder definition, or any other design element. Ten affected cells were regenerated as full tight-window batches at the new ceiling; their original versions are tagged-out and excluded from the canonical analysis corpus. Full audit log in `../corpus/audit-log.md`.

## Unbounded web search

The original generation used `max_uses=8` as a precaution. Diagnostic showed students writing the original briefs had **unrestricted web access** for the analogue exercise; the AI-side cap therefore introduced a slight asymmetry between the human and AI conditions. The cap was raised to 50 (effectively uncapped — the maximum observed search-attempts count was 49). This preserves the human↔AI symmetry on web-access affordance, without altering the prompts.

## Single-model design

All four rungs use one model (`claude-sonnet-4-6`). The variable under study is the elicitation, not the model. Multi-model comparison is out of scope for this corpus; the ladder's bindingness dimension is meaningful only when the underlying generator is held constant.

## Generation freeze

The TMBA/E1/E2/E3 prompts and the MBA brief were hashed and frozen at the start of generation. Their hashes are preserved in `../coding/HASHES.txt` (alongside the coding-prompt hashes — same file format, both prove immutability). Each generation event logs the prompt hash that was used; any drift surfaces as a hash mismatch.

## Tight-window protocol

For each `(entity × seed)` cell, the 4 rungs are generated back-to-back in a single orchestration batch (a "tight-window batch"). The batch holds a shared `tight_window_batch_id` UUID across the 4 generations. This protocol controls for **web-state drift across rungs**: web content seen by one rung is the same content available to the next rung in the same cell, within minutes. Cross-cell drift (different days, different web state) is acceptable because cells are the replication unit.

## Inclusion of "Strategic Theatre" framing

The phrase "Strategic Theatre" refers to the elicitation-response gradient at the centre of the study, not to any rhetorical or evaluative judgement about the model's output. The corpus is designed to be theatre-free in the data: each report is what the model produced under the elicitation regime, no post-editing.
