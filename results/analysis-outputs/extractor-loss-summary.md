# Pass-1 extractor-loss audit (hedge + citation preservation)

Sampled n=611 (claim, source-paragraph) pairs from the canonical pipeline analyses. For each, the best-matching source sentence is selected; we compare hedge words (from v5.3 §3.3 Q1+Q2 named lists) and inline citation markers present in the sentence vs the atomised claim.

## Rates

| Loss type | Pairs with marker in source sentence | Pairs where ≥1 marker dropped | Drop rate | Marker-level survival |
|---|---|---|---|---|
| **Hedge words** (Q1+Q2 named list) | 16 | 6 | **37.5%** | 62.5% (10/16 occurrences) |
| **Citation markers** (inline) | 46 | 37 | **80.4%** | 15.4% (12/78 occurrences) |

## Per-group breakdown

| Group | n pairs | hedge drop rate | citation drop rate |
|---|---|---|---|
| tesla | 24 | 0.0% (0/2) | 100.0% (15/15) |
| rok | 50 | 0.0% (0/2) | 85.7% (6/7) |
| ai-corpus | 537 | 50.0% (6/12) | 66.7% (16/24) |

## Worked examples of dropped hedges (first 10)

- **ENTITY_34_TMBA_seed1** (pipeline qualifier=Q0)
  - SOURCE: A wave of consolidation within the automation industry is notable — Siemens acquired Digital Industries Software and Schneider Electric acquired AVEVA, which may further intensify competition.
  - CLAIM: Siemens acquired Digital Industries Software.
  - DROPPED: may
- **ENTITY_34_TMBA_seed1** (pipeline qualifier=Q0)
  - SOURCE: A wave of consolidation within the automation industry is notable — Siemens acquired Digital Industries Software and Schneider Electric acquired AVEVA, which may further intensify competition.
  - CLAIM: Schneider Electric acquired AVEVA.
  - DROPPED: may
- **ENTITY_38_E1_seed1** (pipeline qualifier=Q0)
  - SOURCE: Introduction and Strategic Situation Tesla, Inc. was founded in 2003 and spent the better part of two decades as the world's most admired disruptor: a company that redefined what an automobile could be, built a vertically integrated energy 
  - CLAIM: Tesla, Inc. was founded in 2003.
  - DROPPED: could
- **ENTITY_38_E1_seed1** (pipeline qualifier=Q0)
  - SOURCE: While projected global growth around 3.2% for 2024 and 2025 suggests a stable, albeit moderate, economic environment, persistent inflation and interest rate hikes in early 2024 continued to dampen consumer spending on discretionary items li
  - CLAIM: Persistent inflation and interest rate hikes in early 2024 dampened consumer spending on discretionary items like EVs, making Tesla's premium products more price-sensitive.
  - DROPPED: suggests
- **ENTITY_25_E1_seed1** (pipeline qualifier=Q0)
  - SOURCE: The $5.2 billion Nigeria fine and COMESA investigations suggest organisational gaps in regulatory risk management.
  - CLAIM: The $5.2 billion Nigeria fine was ultimately negotiated down.
  - DROPPED: suggest
- **ENTITY_33_E3_seed1** (pipeline qualifier=Q0)
  - SOURCE: SpaceX reportedly captured over 70% of launches in recent years, posing a significant threat to Rocket Lab's market share and pricing power.
  - CLAIM: SpaceX's dominant market share poses a significant threat to Rocket Lab's market share and pricing power.
  - DROPPED: reportedly

## Worked examples of dropped citation markers (first 10)

- **Tesla Human (MBA) — reliabilit** (pipeline citation=Ext)
  - SOURCE: Tesla Inc. (Tesla) was founded by a group of engineers, including Elon Musk, in 2003 (Isaacson, 2023) with the goal to ‘expedite the move from a mine-and-burn hydrocarbon economy towards a solar electric economy, which I believe to be the p
  - CLAIM: Tesla Inc. was founded by a group of engineers, including Elon Musk, in 2003.
  - DROPPED: (Isaacson, 2023) | (Musk, 2006)
- **Tesla Human (MBA) — reliabilit** (pipeline citation=Ext)
  - SOURCE: Tesla Inc. (Tesla) was founded by a group of engineers, including Elon Musk, in 2003 (Isaacson, 2023) with the goal to ‘expedite the move from a mine-and-burn hydrocarbon economy towards a solar electric economy, which I believe to be the p
  - CLAIM: Tesla's founding goal was to expedite the move from a hydrocarbon economy towards a solar electric economy as the primary sustainable solution.
  - DROPPED: (Isaacson, 2023) | (Musk, 2006)
- **Tesla Human (MBA) — reliabilit** (pipeline citation=Ext)
  - SOURCE: The firm's long- term strategy was to start at the top end of the car market and move down towards a mass- produced vehicle to achieve the scale required to turn a profit as an auto manufacturer. (Musk, 2006).
  - CLAIM: Tesla's long-term strategy was to start at the top end of the car market and move down towards a mass-produced vehicle to achieve the scale required to turn a profit as an auto manufacturer.
  - DROPPED: (Musk, 2006)
- **Tesla Human (MBA) — reliabilit** (pipeline citation=Ext)
  - SOURCE: In the United States, there is Republican opposition to EVs, with their leader, Donald Trump, saying owners should ‘Rot in hell’ (Economist, 2024).
  - CLAIM: In the United States, there is Republican opposition to EVs, with Donald Trump saying EV owners should 'Rot in hell'.
  - DROPPED: (Economist, 2024)
- **Tesla Human (MBA) — reliabilit** (pipeline citation=Ext)
  - SOURCE: Political Increased focus on climate change in recent years has spurred governments' investments in green technology through subsidies, tax credits, and R&D sponsorship as nations seek to hasten the green transition (Mangram, 2012).
  - CLAIM: Increased focus on climate change in recent years has spurred governments' investments in green technology through subsidies, tax credits, and R&D sponsorship.
  - DROPPED: (Mangram, 2012)
- **Tesla Human (MBA) — reliabilit** (pipeline citation=Ext)
  - SOURCE: A major piece of legislation in the United States in support of this is Joe Biden’s signature policy, The Inflation Reduction Act, which will contribute ‘$396bn of subsidies and tax credits over the course of a decade on renewable energy an
  - CLAIM: The Inflation Reduction Act will contribute $396bn of subsidies and tax credits over a decade on renewable energy and electric vehicles.
  - DROPPED: (Economist, 2022)
- **Tesla Human (MBA) — reliabilit** (pipeline citation=None)
  - SOURCE: This is a boon for firms such as Tesla, which have benefited from such provisions; Tesla reports $5bn of tax credits in its most recent financial statements (Tesla, 2024).
  - CLAIM: The Inflation Reduction Act is a boon for firms such as Tesla, which have benefited from its provisions.
  - DROPPED: (Tesla, 2024) | Tesla reports
- **Tesla Human (MBA) — reliabilit** (pipeline citation=Ext)
  - SOURCE: This is a boon for firms such as Tesla, which have benefited from such provisions; Tesla reports $5bn of tax credits in its most recent financial statements (Tesla, 2024).
  - CLAIM: Tesla reports $5bn of tax credits in its most recent financial statements.
  - DROPPED: (Tesla, 2024)
- **Tesla Human (MBA) — reliabilit** (pipeline citation=Ext)
  - SOURCE: Democratic voters purchase the majority of EVs, and Republicans remain highly sceptical (Economist, 2024).
  - CLAIM: Democratic voters purchase the majority of EVs in the US.
  - DROPPED: (Economist, 2024)
- **Tesla Human (MBA) — reliabilit** (pipeline citation=Ext)
  - SOURCE: Democratic voters purchase the majority of EVs, and Republicans remain highly sceptical (Economist, 2024).
  - CLAIM: Republican voters remain highly sceptical of EVs.
  - DROPPED: (Economist, 2024)

## Interpretation

A drop rate is "non-trivial" if a meaningful share of marker-bearing source sentences lose their marker on atomisation. Headline drop rates above are reported as **pair-level** (each sentence-with-marker counts once); marker-level survival is more granular.

Caveats on the audit itself:
- Best-matching-sentence heuristic can mis-identify the source sentence when the pipeline atomised across multiple sentences. In those cases the "source sentence" used for comparison may differ from what the pipeline actually summarised. The numbers above bias toward over-counting drops (the heuristic may bring in a sibling sentence whose marker was never meant to attach to the claim).
- The citation-marker patterns are a heuristic list, not exhaustive (e.g., footnote markers, in-text "Ref. 3" are not covered). Likely under-counts real citations.
- The hedge list mirrors v5.3 §3.3 exactly (named reference list); private/graduated hedge intuitions are not in scope per the codebook.

If hedge drop rate is high → qualifier-trajectory findings in the paper need a measured caveat or partial re-extraction.
If citation drop rate is high → grounding/citation findings need the same.
