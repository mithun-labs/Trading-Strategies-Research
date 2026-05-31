# Currency Basis Momentum (Fan/Han/Li/Liu 2025)

## Overview

A systematic long-short currency strategy that trades on the difference in momentum
between first- and second-nearby forward contracts — the "basis momentum" (BM) signal —
applied to 48 currencies over monthly rebalancing. Published in European Financial
Management (April 2025). The paper's main contribution is decomposing the BM signal
into four components and showing it partially hedges carry and momentum drawdowns
during currency crises.

## Asset Class / Timeframes

- **Asset class:** Foreign exchange — 48 currencies (G10 + emerging markets, USD-based)
- **Timeframe:** Monthly rebalancing (signal formation: 1, 3, 6, 9, or 12 months; best = BM-3)
- **Sample period:** December 1986 – October 2020 (34 years)
- **Instruments:** Forward exchange rate contracts (first- and second-nearby)

## Core Logic

**BM Signal construction:**

`BM(t, f) = [Cumulative excess return of 1st-nearby forward over f months]`
`         − [Cumulative excess return of 2nd-nearby forward over f months]`

This is directly analogous to basis momentum in commodity futures markets (difference
in momentum between front-month and second-month contract), applied to currency
forward contracts.

**Portfolio construction:**
1. Sort all 48 currencies by BM(t, f) signal into equally-weighted quintile portfolios
2. Go long the top quintile (highest BM), short the bottom quintile (lowest BM)
3. Rebalance monthly
4. Transaction costs: bid-ask spread adjustments applied at each rebalance

**Best formation period:** BM-3 (3-month formation) — highest monthly return and Sharpe

## Economic Rationale

ANALYSIS: The BM signal exploits the term structure of currency forward premiums. If
forward premiums were flat across maturities (CIP-consistent), BM would generate zero
signal — it only exists because the premium paid in the first-nearby forward differs from
the second-nearby. The paper's decomposition shows this is partly a carry signal (forward
discount FD) and partly a momentum signal (excess return change EC), with two residual
terms (spot rate change SC and second-nearest forward discount FD2) adding statistical
but economically modest value.

**When the edge should disappear (falsifiability test):**
- If CIP deviations narrow toward zero (the post-2014 trend was toward normalization;
  post-COVID this has partially reversed)
- If arbitrageurs fully price the term structure information embedded in BM, arbitraging
  away the carry and momentum components
- If emerging market currency transaction costs rise or liquidity shrinks, making the
  50% of the universe that is EM impractical to trade
- If the rate cycle becomes unusually flat (all formation periods generate similar signals),
  reducing cross-currency sorting power

ANALYSIS: Since the paper shows carry (FD) and momentum (EC) are the dominant contributors,
BM's "distinct" rationale is weaker than initially claimed. If those two known factors stop
working (as documented post-publication for currency momentum — see `currency-momentum-factor-menkhoff.md`),
BM's edge would likely evaporate simultaneously.

## Entry Conditions

- At each month-end, compute BM(t, 3) for all 48 currencies
- Sort all currencies by their BM-3 signal value
- Establish long positions in the top quintile (9-10 currencies)
- Establish short positions in the bottom quintile (9-10 currencies)
- Equal-weight within each quintile

## Exit Conditions

- Monthly rebalancing; full portfolio reconstruction each period
- No explicit stop-loss defined; positions held for 1 month

## Risk Management

- Equal-weight within quintile (implicit diversification)
- Transaction costs modeled via bid-ask spreads (net-return targeting)
- No explicit stop-loss, no max-position sizing, no max-drawdown exit rule
- No regime filter; fully invested at all times (long and short)

## Indicators Used

- First-nearby currency forward contract cumulative excess return (f-month lookback)
- Second-nearby currency forward contract cumulative excess return (f-month lookback)
- BM signal = difference between the two
- Transaction cost adjustment: bid-ask spread on forward contracts

## Transaction Costs & Capacity

**Transaction costs:** Explicitly modeled via bid-ask spread adjustments at each rebalance.
The published paper confirms that BM strategies "yield statistically significant excess
returns and Sharpe ratios across formation periods of 1, 3, 6, 9 and 12 months after
considering transaction costs." This is a meaningful positive attribute; most FX strategy
papers skip cost modeling.

**Capacity concerns (ANALYSIS):**
- Strategy uses 48 currencies; ~50% are emerging market currencies with materially wider
  bid-ask spreads and lower liquidity than G10
- Bid-ask spread modeling captures explicit costs but NOT market impact — entering/exiting
  9-10 EM currency forward positions at month-end could move the market at any institutional size
- No capacity estimate provided in the paper
- A G10-only version would be higher-capacity but likely lower Sharpe (EM currencies
  contribute disproportionate excess return in FX factor strategies due to higher friction
  and lower arbitrage activity)
- The paper does not address capacity or optimal position sizing

## Backtest Evidence

**Status:** CLAIMED, UNVERIFIED — published peer-reviewed results but no open code;
full paper access blocked during research (HTTP 403 on QUB repository and Wiley)

**Full-sample (Dec 1986 – Oct 2020), BM-3:**
- Monthly excess return: 1.23%, t-statistic = 6.67 (CLAIMED)
- Annualized Sharpe: 0.52 (CLAIMED)

**Benchmark comparisons (CLAIMED):**
- Standalone carry Sharpe: ~0.63
- Standalone momentum Sharpe: ~0.79 (in this paper's dataset — lower than Menkhoff 2012's 0.95 because of different sample period and currency universe)
- Equal-weighted carry + momentum combination Sharpe: ~0.98
- BM Sharpe (0.52) is LOWER than both benchmarks on Sharpe alone

**The "lower volatility" claim:** BM's stated advantage is lower volatility (vs. carry)
and tail-risk hedging during crises, not higher absolute Sharpe. This is the paper's
genuine contribution — crisis period performance.

**Sub-period analysis (BM-3, CLAIMED):**

| Sub-period | Period | Monthly return | t-stat |
|------------|--------|---------------|--------|
| 1 | Dec 1986 – Dec 1998 | ~1.04% | ~4.62 |
| 2 | Jan 1999 – Dec 2010 | ~1.83% | NOT REPORTED |
| 3 | Jan 2011 – Oct 2020 | NOT REPORTED | NOT REPORTED |

Sub-period 3 returns are not confirmed in accessible search summaries — a significant gap.

**Crisis behavior (CLAIMED):** During the 1997–2002 Asian/EM crisis and 2008 GFC, the BM
strategy accumulates positive returns while carry and momentum decline. This is attributed
to the FD2 (second-nearest forward discount) component providing counter-cyclical exposure.

## Forward-Test Evidence

None. The paper's sample ends October 2020. No out-of-sample or post-publication test has
been published as of May 2026. The strategy has been in the public domain since March 2024
(SSRN upload) and April 2025 (EFM publication). A meaningful live/forward-test period of
approximately 1 year (2025-2026) exists but has not been studied.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 0.52 (BM-3, full sample) | CLAIMED, UNVERIFIED | Fan et al. EFM 2025 |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | 1.23%/mo × 12 = ~14.8% annualized nominal (ANALYSIS from stated monthly) | ANALYSIS only — do not treat as sourced value | — |
| Number of Trades | ~408 monthly rebalances × 18-20 currency positions (ANALYSIS) | ANALYSIS estimate | — |
| Average Trade Duration | 1 month (by construction) | CLAIMED, UNVERIFIED | Fan et al. EFM 2025 |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% (long-short, always invested) | CLAIMED, UNVERIFIED | Fan et al. EFM 2025 |

**NOTE on Annual Return:** The 14.8% figure in the table is ANALYSIS, not sourced. It is
listed only to flag that the CAGR was not reported. Do not use it for ranking or comparison.

**On the "Sharpe 0.52 vs 1.05" discrepancy in the queue note:** Evidence from multiple
search result snippets consistently reports BM-3 Sharpe = 0.52 for the published EFM
version. The 1.05 figure may originate from: (a) the preliminary EFMA 2023 conference
paper ("Decomposing Basis-Momentum in Currency Market") which uses a different sample
or specification; or (b) the equal-weighted BM+carry+momentum triple-combination portfolio.
I cannot confirm which interpretation is correct without full-paper access. The 0.52 figure
is from the published, peer-reviewed EFM 2025 paper and is the canonical reference.

## Community Sentiment

Very new paper (EFM April 2025). No independent community critique, blog analysis,
or replication attempt found as of May 2026. The paper has been discussed only in the
context of its own abstract and journal listing. No practitioner blog (Quantpedia,
Alpha Architect, CXOA) has reviewed it. No critique found on SSRN comment sections.

ANALYSIS: The absence of community scrutiny is itself a risk factor — it means no
independent party has yet attempted replication or stress-tested the strategy.

## Why This Might Be Nothing

**1. Most likely benign explanation — decomposition reveals no independent mechanism:**
The paper explicitly shows that carry (FD) and momentum (EC) are the dominant BM
contributors. Since a standalone BM strategy has LOWER Sharpe (0.52) than both carry
(0.63) and momentum (0.79) and the equal-weighted carry+momentum combo (0.98), the BM
signal is best interpreted as a linear re-weighting of known factors. The "incremental
information" from SC and FD2 is statistically significant but economically small —
adding it doesn't beat the simpler carry+momentum combination on Sharpe. BM's only
genuine advantage is crisis-period performance, which has not been tested out-of-sample.

**2. Publication effect — the strongest base-rate argument:**
Currency momentum (the dominant BM component) was published in 2012 (Menkhoff et al.
JFE) with in-sample Sharpe 0.95. By 2022, an independent peer-reviewed paper
(Hutchinson et al. IRFA) documented that post-publication Sharpe fell to −0.32. BM
was SSRN-posted in March 2024 and EFM-published in April 2025. Arbitrageurs reading
FX factor research have been active since 2012. If momentum decayed, BM (which is
~50% momentum component) will decay with it. The publication effect is the single
highest-probability explanation.

**3. Cost case:**
Bid-ask spreads are modeled but market impact is not. For EM currencies (50% of the
universe), entering 9-10 forward positions at month-end at any institutional size will
move the market. A true cost-inclusive test for sizes > $10M would likely erode the
1.23%/month gross edge materially. In practice, this strategy may only be executable
in sub-$10M notional sizes.

**4. Regime dependence:**
The sample (1986–2020) covers the post-Bretton Woods managed float era, three decades
of declining rate volatility, the GFC, and the 2010s low-rate regime. The strategy has
NOT been tested against:
- The 2022 rate shock (largest global central bank tightening in 40 years)
- The 2023-2025 high-rate, high-volatility FX environment
- The post-COVID divergence in EM vs. G10 carry dynamics

Sub-period 3 (2011–2020) returns are not confirmed — if they declined sharply vs. sub-period 2 (1.83%/mo), that is pre-publication evidence of decay already underway.

**5. Sample/overfitting:**
Five formation periods tested (BM-1, BM-3, BM-6, BM-9, BM-12); BM-3 is selected as
"best." This is a 5-way selection within the same dataset. With 5 choices, the probability
of the best variant outperforming by chance is non-trivial. The paper does not perform
a multiple-testing correction for the formation period selection.

**6. What single piece of evidence would most change my mind:**
An independent replication of BM-3 using post-October 2020 data (ideally 2020–2026,
incorporating the rate shock and dollar cycle), showing Sharpe remains near 0.52 with
realistic bid-ask spreads and no look-back bias. I do not have this, and it has not
been published.

## Strengths / Weaknesses

**Strengths:**
- Peer-reviewed (EFM is a legitimate European finance journal)
- Transaction costs explicitly modeled — unusual and positive
- 34-year sample (1986–2020) — substantial history
- Sub-period analysis shows stability across different macro regimes (though sub-period 3 unconfirmed)
- Crisis-period hedge property is genuinely novel: positive returns when carry and momentum crash
- Decomposition adds theoretical transparency (4 components identified)
- Clear, mechanical construction: quintile sort, equal-weight, monthly rebalance

**Weaknesses:**
- BM Sharpe (0.52) is LOWER than carry alone (0.63) and momentum alone (0.79)
- No OOS test; sub-periods are in-sample splits, not walk-forward
- Sample ends October 2020 — excludes most recent 5+ years
- No open code; full paper requires journal access
- Dominant components (carry + momentum) are known to have decayed post-publication
- 48-currency universe includes illiquid EM pairs; market impact unmodeled
- No max drawdown reported; recovery factor, win rate, and other key metrics absent
- Published 2025; no independent replication exists
- Multiple formation period testing without correction (5 alternatives, best selected)

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Wt | Score | Rationale |
|-----------|-----|-------|-----------|
| Logical/economic soundness (falsifiable) | 3 | 5 | Forward term structure rationale has genuine basis (analogous to commodity BM); conditions when edge disappears are partially specified (CIP normalization, arbitrage of carry+momentum components); weakened by decomposition showing no truly independent mechanism |
| Transparency & reproducibility | 2 | 5 | Quintile sort rules, formation periods, and cost methodology described in published paper; no open code; full paper access requires journal subscription (blocked during research) |
| Evidence auditability | 2 | 5 | Peer-reviewed EFM (solid journal, not top-5); methodology documented in paper; no open code; no independent replication; methodology reproducible in principle but not verified line-by-line |
| Risk management quality | 2 | 2 | No stop-loss; no position sizing beyond equal-weight quintile; no max-loss rule; long/short equal-weight is the only structural risk control |
| Cost & capacity realism | 2 | 5 | Bid-ask spreads explicitly modeled (strong positive); market impact not addressed; EM capacity concern unaddressed; G10-only would be higher capacity but untested |
| Honest treatment of drawdowns/failure | 1.5 | 3 | Sub-period analysis shows stability in sub-periods 1 and 2; sub-period 3 not confirmed; max drawdown not reported; no explicit failure regime discussion |
| Robustness (OOS/walk-forward/multi-market) | 1.5 | 2 | Sub-period analysis within sample only; no walk-forward; no second market test; sample ends Oct 2020 missing 5+ years of post-publication data |
| Survived independent scrutiny | 1 | 2 | Only peer-review scrutiny (EFM); no independent academic replication; no practitioner review; no community discussion found as of May 2026 |

**Weighted sum:**
(5×3) + (5×2) + (5×2) + (2×2) + (5×2) + (3×1.5) + (2×1.5) + (2×1)
= 15 + 10 + 10 + 4 + 10 + 4.5 + 3 + 2 = **58.5**

**Latent score:** round(58.5 / 150 × 100) = **39**

**Verification cap:** Evidence auditability = 5 → cap = 74

**Confidence (capped):** min(39, 74) = **39**

**→ latent 39 (capped to 39; evidence auditability 5/10; no OOS test; BM Sharpe 0.52 below both carry and momentum benchmarks; dominant components are known decayed factors)**

**Band:** LOW CONFIDENCE (0–39)

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Moderate. Requires access to forward exchange rate data for 48 currencies,
  including both first-nearby and second-nearby contract prices. This data is available from
  Bloomberg or Barclays (the source used in the paper) but not free.
- **Scalability:** Limited above ~$10–20M notional in EM currencies; G10-only version more
  scalable but untested.
- **Automation:** Mechanically definable once data is acquired. Monthly rebalancing is low
  frequency. The signal calculation is straightforward. Data acquisition is the main barrier.

## MQL5 / Python / Pine Feasibility

- **Python:** Feasible if forward rate data is available (Barclays, Bloomberg, or Refinitiv
  feeds). Core signal: `BM = rolling_return(fwd_1, f) - rolling_return(fwd_2, f)`; quintile
  sort is trivial. Estimated: ~200-300 lines with data infrastructure.
- **MQL5:** Impractical — requires tick data for 48 currency forward contracts and custom
  data feed integration; MT4/MT5 do not natively support forward vs. spot decomposition.
- **Pine Script:** Not feasible — multi-currency portfolio construction is not supported
  natively; only single-instrument scripts.

## Similar Strategies

- `currency-momentum-factor-menkhoff` — Dominant component of BM (EC term); see `currency-momentum-factor-menkhoff.md`; Sharpe 0.95 in-sample, −0.32 OOS post-publication
- `fx-carry-value-momentum-200yr` — BM carry component (FD term); see `fx-carry-value-momentum-200yr.md`; carry Sharpe ~0.39 over 200 years
- `fx-mean-reversion-futures-monthly` — Monthly FX futures strategy on same market; linear variant Sharpe 0.12; see `fx-mean-reversion-futures-monthly.md`

## Red Flags

- **BM Sharpe (0.52) LOWER than both carry and momentum benchmarks** — the "advantage"
  is crisis-period behavior, not risk-adjusted return; this weakens the core case
- **Dominant components are known decayed factors** — carry and momentum both show
  declining performance post-publication in independent studies
- **No OOS period tested** — sub-periods are in-sample splits; post-Oct 2020 performance unknown
- **Sub-period 3 returns not confirmed** — if declining vs. sub-period 2, pre-publication
  decay may already be documented within the paper
- **Multiple formation period testing without correction** — BM-3 selected as best among 5
- **Conflicting Sharpe metrics across paper versions** (0.52 vs. 1.05 reported in earlier queue note)
- **EM currency market impact unmodeled** — strategy likely only viable at small scale
- **No max drawdown reported** — prevents proper risk assessment

## Source Links

| URL | Retrieved | Notes |
|-----|-----------|-------|
| https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4925173 | 2026-05-31 | SSRN 4925173 — HTTP 403 |
| https://onlinelibrary.wiley.com/doi/full/10.1111/eufm.12555 | 2026-05-31 | EFM published paper — HTTP 403 |
| https://pure.qub.ac.uk/files/636563351/Euro_Fin_Management_-_2025_-_Fan_-_Understanding_the_Performance_of_Currency_Basis_Momentum.pdf | 2026-05-31 | QUB repository PDF — HTTP 403 |
| https://www.efmaefm.org/0EFMAMEETINGS/EFMA%20ANNUAL%20MEETINGS/2023-UK/papers/EFMA%202023_stage-4455_question-Full%20Paper_id-136.pdf | 2026-05-31 | EFMA 2023 conference paper (earlier version) — HTTP 403 |

## Analyst Notes

**FACTS (from search result snippets, EFM 2025 published paper):**
- Paper published European Financial Management, April 2025, DOI 10.1111/eufm.12555
- Authors: Minyou Fan (QUB), Xing Han, Ang Li, Jiadong Liu
- Sample: 48 currencies, Dec 1986 – Oct 2020
- BM-3 monthly return: 1.23%, t-stat: 6.67
- BM-3 annualized Sharpe: 0.52
- Transaction costs modeled: bid-ask spreads
- BM decomposed into 4 components: FD (carry), EC (momentum), SC (spot change), FD2
- Carry (FD) and momentum (EC) are dominant contributors
- Crisis-period hedge property claimed for 1997-2002 and 2008 periods
- Sub-period 2 (1999-2010) monthly return ~1.83% for BM-3
- Sub-period 3 (2011-2020) returns NOT confirmed in accessible snippets

**ANALYSIS:**
- BM Sharpe of 0.52 is below both carry (0.63) and momentum (0.79) benchmarks, suggesting BM is not superior on a purely risk-adjusted basis — the value is in the crisis hedge, which is an unconfirmed OOS claim
- The decomposition result implies BM is largely a carry + momentum factor with marginal additions; the "independent" BM strategy has lower Sharpe than the simpler combination
- With currency momentum showing documented post-publication OOS decay (−0.32 Sharpe, Hutchinson 2022), the most probable outcome for BM post-publication is similar decay in the EC component
- The Sharpe divergence (0.52 vs. 0.79 for momentum vs. 0.52 for BM) suggests that the FD2 and SC components add diversification value (lower volatility) but not return enhancement
- Annualizing the 1.23%/month: ~14.8% nominal annual return; but volatility to support Sharpe 0.52 would imply annual volatility ~28.5% (ANALYSIS: 14.8% / 0.52 = 28.5%)
- This is high volatility for a currency factor strategy, consistent with EM exposure

**ASSUMPTIONS:**
- Assumed carry Sharpe (0.63) and momentum Sharpe (0.79) are from this paper's specific dataset (Dec 1986 – Oct 2020, 48 currencies); these differ from Menkhoff 2012 figures (different sample)
- Assumed bid-ask spread modeling covers explicit transaction costs but not market impact

## Future Research Needed

1. Access full paper to verify sub-period 3 (2011–2020) performance — if declining, pre-publication decay is already documented
2. Test BM-3 on post-October 2020 data (2020–2026) to assess publication-effect decay
3. G10-only variant: test whether the strategy survives when EM currencies (high costs, low liquidity) are excluded
4. Compare BM to simple equal-weighted carry+momentum combination: if Sharpe 0.52 (BM) < 0.98 (carry+momentum combo), why implement BM at all?
5. Check for independent replication: this paper was published April 2025; by 2026-2027, replication attempts should appear
6. Resolve the Sharpe 0.52 vs 1.05 discrepancy — access the EFMA 2023 conference paper to understand how the earlier version differs from the final EFM publication
