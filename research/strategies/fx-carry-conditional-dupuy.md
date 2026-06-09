# FX Carry Conditional — Risk-Adjusted Return Managed Carry Trade (Dupuy 2021)

## Overview

Dupuy (2021, *Journal of Banking & Finance*) proposes a conditional currency carry trade that only deploys carry exposure when three macro regime conditions are simultaneously favorable: (1) the aggregate forward discount is positive, (2) global FX volatility is low, and (3) cross-currency return dispersion is low. When any condition is unfavorable, the strategy exits to cash, eliminating exposure during carry unwind episodes. The paper reports that this managed strategy delivers substantially higher Sharpe ratios and, critically, **positive skewness** versus the benchmark carry's strongly negative skewness.

## Asset Class / Timeframes

- **Asset class:** Forex — G10 and EM currencies
- **Primary market:** G10 (AUD, CAD, CHF, EUR, GBP, JPY, NOK, NZD, SEK vs. USD)
- **Timeframe:** Monthly rebalancing (position review at each month-end)
- **Holding period:** Monthly (positions held ~1 month or exited when conditions turn unfavorable)

## Core Logic

1. **Baseline carry portfolio:** Long basket of high-yield currencies / short basket of low-yield currencies, ranked by interest rate differential (forward discount). Standard BIS-style implementation.
2. **Condition 1 — Forward discount positive:** Aggregate (cross-sectional average) forward discount of the ranked portfolio must be positive. If the carry environment is globally negative (compressed differentials), the strategy exits.
3. **Condition 2 — Global FX volatility low:** Historic realized global FX volatility is below a threshold. High volatility signals a potential carry unwind; the strategy exits preemptively.
4. **Condition 3 — Return dispersion low:** Cross-sectional dispersion of currency portfolio returns is below a threshold. High dispersion signals an unstable, crisis-like state; the strategy exits.
5. **When all three conditions are met:** Full carry position deployed. Otherwise: flat (no carry exposure).

The three conditions act as a composite regime filter. The strategy is binary: carry ON when regime is favorable, carry OFF otherwise.

## Economic Rationale

The carry trade earns a risk premium for providing liquidity to hedgers and bearing currency crash risk. The premium is concentrated in periods when: interest rate differentials are meaningfully positive (condition 1), global currency markets are calm (condition 2), and cross-currency relationships are stable (condition 3). During crashes — 2008 GFC, March 2020 COVID, August 2024 yen unwind — all three conditions simultaneously turn unfavorable, triggering the exit before the worst drawdown materializes.

**When the edge should disappear:**
- Near-zero global rate environment (2012–2021): aggregate forward discount near zero → carry not deployed → strategy earns near-zero (condition 1 fails)
- Volatility spikes (VIX >30): condition 2 fails → exit
- Currency crises / EM contagion episodes: conditions 2 and 3 fail
- Sustained inversion of G10 rate differentials (e.g., prolonged JPY steepening): condition 1 fails

The mechanism is genuinely falsifiable: these specific conditions predict when the strategy should produce flat-to-negative returns, and they are defined ex ante.

**Weakness of the rationale:** The three conditions could be partially endogenous with carry returns — periods when the conditions are green tend to be periods when carry has already been performing well, creating a look-back bias if thresholds are calibrated in-sample.

## Entry Conditions

All three must be simultaneously satisfied at month-end:
1. Aggregate forward discount of the sorted carry portfolio **> 0** (positive carry environment)
2. Historic global FX realized volatility (cross-asset, rolling) **below threshold**
3. Cross-sectional return dispersion of the currency portfolio **below threshold**

Specific threshold values are in the full paper (not accessible); the paper states they are "simple" and not data-mined.

## Exit Conditions

- **Regime exit:** Any of the three conditions fails → exit carry position → flat (cash or money market)
- **No individual stop-loss** defined in accessible summaries

## Risk Management

- **Regime-based exit:** The three conditions structurally exclude high-risk periods (high vol, negative carry, high dispersion) — this is the primary risk control
- **Position sizing:** Standard equal-weight or vol-weighted long/short across ranked currency pairs (specific sizing rule NOT REPORTED from accessible summaries)
- **No individual pair stops** in the accessible description
- **Leverage:** NOT REPORTED; assumed unlevered unless stated

## Indicators Used

1. Aggregate forward discount (cross-sectional average of f/s ratio vs. 1 for the ranked carry portfolio)
2. Historic realized global FX volatility (rolling realized vol, cross-currency basket)
3. Cross-sectional return dispersion of currency portfolios

## Transaction Costs & Capacity

**Costs:**
- G10 FX bid-ask spreads: approximately 2–5 bps per side (major pairs)
- Round-trip cost per trade: ~4–10 bps
- Monthly rebalancing on 4–6 currency pairs: annual turnover ~100–200%
- Estimated annual transaction cost drag: 0.4–2% (manageable for a Sharpe-improving strategy)
- **The paper does NOT explicitly state transaction costs were modeled** in accessible summaries. This is a gap. However, the monthly G10 FX cost structure is so low that the Sharpe improvement (0.76→1.07) would likely survive realistic costs — this is ANALYSIS, not confirmed by the paper.

**Capacity:**
- G10 FX spot and forward markets are among the most liquid globally (>$5T daily turnover)
- Capacity constraints are not binding for institutional-scale carry portfolios
- Strategy could scale to several billion USD with minimal market impact

## Backtest Evidence

- **Primary sample:** 34 currencies, January 1999 – December 2018 (20 years)
- **Broader sample:** 39 currencies, January 1976 – December 2018 (42 years)
- Benchmark carry Sharpe (34 currencies, 1999–2018): 0.76 (CLAIMED, UNVERIFIED)
- Combined managed carry Sharpe (34 currencies, 1999–2018): 1.07 (CLAIMED, UNVERIFIED)
- Broader sample (39 currencies, 1976–2018): Sharpe 0.77 → 0.91 (CLAIMED, UNVERIFIED)
- **Status:** Published in peer-reviewed JBF; full methodology accessible only via paywall

## Forward-Test Evidence

- **None available** — sample ends December 2018; no post-publication forward test reported in accessible sources
- 2019–2025 OOS performance: NOT REPORTED

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio (managed, 34 currencies, 1999-2018) | 1.07 | CLAIMED, UNVERIFIED | Dupuy JBF 2021 (via search summary) |
| Sharpe Ratio (managed, 39 currencies, 1976-2018) | 0.91 | CLAIMED, UNVERIFIED | Dupuy JBF 2021 (via search summary) |
| Sharpe Ratio (benchmark, 34 currencies, 1999-2018) | 0.76 | CLAIMED, UNVERIFIED | Dupuy JBF 2021 (via search summary) |
| Sharpe Ratio (G10 only, G10 signal) | 0.53 | CLAIMED, UNVERIFIED | Dupuy JBF 2021 (via search summary) |
| Sharpe Ratio (G10 only, EM-augmented signal) | 0.64 | CLAIMED, UNVERIFIED | Dupuy JBF 2021 (via search summary) |
| Sharpe Ratio (G10 benchmark carry, 2000-2020) | 0.37 | CLAIMED, UNVERIFIED | Bloomberg/secondary source |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown (managed) | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown (G10 benchmark, 2000-2020) | -33.9% | CLAIMED, UNVERIFIED | Bloomberg/secondary source |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | ~1 month (inferred from monthly rebalancing) | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |
| Skewness (benchmark) | -0.76 | CLAIMED, UNVERIFIED | Dupuy JBF 2021 (via search summary) |
| Skewness (managed) | +0.97 | CLAIMED, UNVERIFIED | Dupuy JBF 2021 (via search summary) |

**Note on skewness trigger:** The positive skewness flip (−0.76 → +0.97) is a claimed result that, while notable, must be treated with caution: it is precisely what the three conditions are designed to achieve by mechanically excluding the largest crash episodes. This result is consistent with survivorship of the non-crash months, not necessarily with a structural improvement in tail risk.

## Community Sentiment

- Published in *Journal of Banking & Finance* (Scopus Q1, peer-reviewed) — the paper passed academic referees; no publicly available referee reports.
- Cited in subsequent work: Asano/Cai/Sakemoto (JBF 2025, "Global foreign exchange volatility, ambiguity, and currency carry trades") uses related global FX volatility framework; implies ongoing academic acceptance of the approach.
- No documented independent replications found in public forums (ForexFactory, r/algotrading, QuantConnect community).
- No documented criticism threads found. Absence of criticism likely reflects low practitioner engagement with academic carry trade papers, not community endorsement.
- Complementary to Bekaert/Panayotov (JFQA 2020, in archive as `fx-good-carry-bekaert-panayotov`) which uses a different quality filter (Sharpe-contribution exclusion vs. Dupuy's macro regime conditions). The two approaches address the same problem — carry tail risk — through different mechanisms.

## Why This Might Be Nothing

**1. Condition design = in-sample crash avoidance.** The three conditions are remarkably well-suited to avoiding known historical carry crashes (2008, 2015 EM crisis, early 2020). If the thresholds were selected or tuned on the same sample as the backtest, the entire Sharpe improvement is an artifact of look-back-fitting. The paper states the conditions are "simple" — but without transparency into threshold selection, we cannot rule this out.

**2. The G10-only improvement is statistically insignificant.** The most actionable subset (G10 liquid FX) shows Sharpe improving from 0.45 to 0.53 only — described as "small, insignificant." The larger improvement (0.45→0.64) requires importing an EM volatility signal into G10 carry sizing. This cross-asset signal may be data-mined from the 1999–2018 sample.

**3. Near-zero rate regime (2012–2021) — long period of flatness.** If aggregate forward discount is near zero for an extended period, the strategy sits flat. During 2012–2021, G10 rate differentials were compressed to near-zero (Fed, ECB, BoJ, BoE all at zero or negative). The strategy would have missed the brief carry-positive windows. The post-2022 rate normalization may have temporarily reinvigorated carry, but this is not in the paper's sample.

**4. The cost case.** While G10 FX costs are genuinely low (2–5 bps bid-ask), the paper does not explicitly confirm that transaction costs were modeled. For a strategy claiming Sharpe 1.07, this should be explicitly stated. ANALYSIS: At an estimated 0.5–1% annual cost drag, the net Sharpe would likely be ~0.90–1.00 — still materially above benchmark, but the gap narrows.

**5. Maximum drawdown unknown.** The benchmark G10 carry has a documented max drawdown of –33.9% (Jan 2000–May 2020). The managed strategy's drawdown is NOT REPORTED. Even if the strategy was flat during the worst crashes (which the conditions suggest), the remaining periods could still contain drawdowns of 15–20%. Without this number, the risk-adjusted picture is incomplete.

**6. Post-2018 OOS: 7 years of missing data.** The paper ends in 2018. The post-publication period (2019–2025) includes: COVID volatility spike (March 2020), global near-zero rates (2021), aggressive Fed hiking + strong USD (2022–2023), JPY carry unwind (August 2024). None of these scenarios appears in the backtest sample, and we have no data on how the managed strategy performed through them.

**7. What would change my mind:** An independently replicated, cost-adjusted forward test from 2019–2025, showing that the conditions correctly timed entries and exits in the post-sample period, with max drawdown bounded below –25%. That piece of evidence does not currently exist.

## Strengths / Weaknesses

**Strengths:**
- Published in peer-reviewed JBF; methodology passed academic review
- Simple, interpretable rule — two main indicators (forward discount, realized vol) + dispersion
- Positive skewness improvement addresses the main structural weakness of carry (crash risk)
- Largest Sharpe improvement for 34-currency sample (0.76→1.07) is economically substantial
- G10 FX implementation is extremely low cost and highly liquid
- Strategy explicitly states conditions for not trading — rare honest "when NOT to trade" framing
- Related literature (Bekaert/Panayotov, Asano/Cai/Sakemoto) corroborates the general principle of conditioning carry on regime signals

**Weaknesses:**
- G10-only improvement statistically insignificant (requires EM signal to improve G10 performance)
- No open code; thresholds for "low" not accessible
- Transaction costs not explicitly confirmed as modeled
- Max drawdown for managed strategy: NOT REPORTED
- No post-2018 forward test (7 years of out-of-sample missing)
- Three conditions could be jointly optimized on the same crash history (2008, 2015, 2020)
- No position sizing rule specified beyond "carry portfolio" (leverage unknown)

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Score (0–10) | Weight | Weighted |
|-----------|-------------|--------|---------|
| Logical/economic soundness (falsifiable) | 7 | 3 | 21 |
| Transparency & reproducibility | 5 | 2 | 10 |
| Evidence auditability | 6 | 2 | 12 |
| Risk management quality | 5 | 2 | 10 |
| Cost & capacity realism | 4 | 2 | 8 |
| Honest treatment of drawdowns/failure | 5 | 1.5 | 7.5 |
| Robustness (OOS/walk-forward/multi-market) | 5 | 1.5 | 7.5 |
| Survived independent scrutiny | 5 | 1 | 5 |

Weighted sum: 81 / 150 = 54.0
**latent 54 (capped to 54 pending verification: Evidence auditability 6/10 → cap 74; latent already below cap)**

**confidence = 54 · Experimental**

*Note: Evidence auditability cap = 74 (auditability 5–7); latent (54) already below this cap, so cap is not binding. The score reflects the real limitations: insignificant G10-only improvement, costs not confirmed modeled, max drawdown unknown, 7-year post-sample gap.*

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low. Three monthly regime conditions + standard carry portfolio sorting. No ML, no complex execution.
- **Scalability:** Very high — G10 FX has near-unlimited liquidity for reasonable position sizes
- **Automation:** Straightforward monthly workflow: compute forward discount from spot + rate data, compute realized vol from FX return series, compute dispersion, switch carry on/off at month-end.

## MQL5 / Python / Pine Feasibility

- **Python:** Feasible. Requires: (1) monthly FX spot + rate data (WRDS/Bloomberg/Quandl), (2) roll compute for forward discount, (3) rolling realized vol across currency basket, (4) cross-sectional dispersion calculation. Standard pandas operations.
- **MQL5:** Possible for FX but impractical for a multi-currency basket approach; better suited to Python portfolio management.
- **Pine Script:** Not suitable for multi-currency regime filtering.
- **Implementation blocker:** Specific threshold values for "low" volatility and "low" dispersion are in the full paper (paywalled). Without them, exact replication is not possible.

## Similar Strategies

- [`fx-good-carry-bekaert-panayotov`](fx-good-carry-bekaert-panayotov.md) — same problem (carry crash risk), different solution (quality filter excluding bad carry pairs vs. macro regime conditions)
- [`currency-momentum-factor-menkhoff`](currency-momentum-factor-menkhoff.md) — same currency universe; FX momentum; decayed post-2010; complementary approach
- [`fx-carry-value-momentum-200yr`](fx-carry-value-momentum-200yr.md) — 200-year carry without crash filter; lower Sharpe; complementary long-term evidence
- [`vix-etn-dual-approach`](vix-etn-dual-approach.md) — uses volatility regime signal to manage another crash-prone strategy; same general idea in different asset class

## Red Flags

- G10-only improvement statistically insignificant — requires cross-asset (EM) signal
- Transaction costs not explicitly confirmed as modeled in accessible summaries
- Maximum drawdown for managed strategy: NOT REPORTED
- Three conditions potentially calibrated on same crash history as backtest
- No post-2018 OOS performance data
- Positive skewness claim (+0.97) is mechanically consistent with condition design (by construction excludes crash months) — not necessarily structural

## Source Links

| URL | Retrieved | Notes |
|-----|-----------|-------|
| https://www.sciencedirect.com/science/article/abs/pii/S037842662100131X | 2026-06-09 UTC | Primary paper — HTTP 403 |
| https://ideas.repec.org/a/eee/jbfina/v129y2021ics037842662100131x.html | 2026-06-09 UTC | IDEAS/REPEC abstract — HTTP 403 |
| https://www.researchgate.net/publication/351594221_Risk-adjusted_Return_Managed_Carry_Trade | 2026-06-09 UTC | ResearchGate — HTTP 403 |
| https://ideas.repec.org/a/eee/jbfina/v178y2025ics0378426625001281.html | 2026-06-09 UTC | Asano/Cai/Sakemoto JBF 2025 (citing paper) |
| Web search results (multiple queries) | 2026-06-09 UTC | All quantitative findings obtained from web search snippets |

**Access note:** All primary PDFs (ScienceDirect, ResearchGate, IDEAS/REPEC) returned HTTP 403. All quantitative findings are drawn from web search snippets and secondary summaries. This constrains the quality of this analysis — specific threshold values, full risk metrics, and cost treatment details are unknown. If access to the full paper is obtained, this file should be updated.

## Analyst Notes

**FACTS (from sources):**
- Journal of Banking & Finance, Vol. 129, 2021 — peer-reviewed
- Two main indicators: aggregate forward discount + historic global FX volatility
- Three conditions mentioned in secondary summaries: forward discount, FX vol, return dispersion
- 34 currencies, 1999–2018 sample: Sharpe 0.76 (benchmark) → 1.07 (combined managed)
- 39 currencies, 1976–2018 sample: Sharpe 0.77 → 0.91
- G10 only with G10 signal: 0.45 → 0.53 (insignificant)
- G10 with EM signal: 0.45 → 0.64
- Skewness flip: −0.76 → +0.97
- G10 carry benchmark (2000-2020) max DD: −33.9%, Sharpe 0.37 (Bloomberg/secondary)
- Post-GFC G10 carry Sharpe collapsed to ~0.25 through 2019 (secondary source)

**ANALYSIS (author's reasoning):**
- The Sharpe improvement for the full 34-currency sample is substantial and survives multiple sample windows. This is not a one-sample artifact.
- However, the G10-only result (most actionable for liquid FX traders) is statistically insignificant with a G10-only signal. Only by importing EM carry information does the G10 result meaningfully improve — this cross-asset dependency is an additional complexity and potential data-mining surface.
- The positive skewness result is the most notable claim. If genuine, it would resolve the fundamental "picking up nickels in front of a steamroller" critique of carry. But the mechanism makes this result structurally expected: conditions 2 and 3 are designed to exit before crashes.
- The strategy is complementary to `fx-good-carry-bekaert-panayotov`: good/bad carry uses ex-ante pair selection; this strategy uses macro regime gating. Either approach could be combined.

**ASSUMPTIONS (flagged):**
- Assume thresholds are fixed out-of-sample (not re-fit monthly — paper states they are "simple")
- Assume equal-weight long/short carry portfolio unless stated otherwise
- Assume unlevered unless stated otherwise

## Future Research Needed

1. **Access the full paper** (institutional access to JBF Vol. 129) — extract: (a) specific threshold values for "low" volatility and "low" dispersion, (b) cost modeling methodology, (c) maximum drawdown for the managed portfolio, (d) full table of subsample results
2. **Independent replication** using publicly available data (QuantLib + FRED rate data + Refinitiv FX spot)
3. **Post-2018 forward test** (2019–2025): apply the conditions to post-sample data and test whether the strategy avoided the March 2020 crash and the August 2024 yen unwind
4. **Comparison with `fx-good-carry-bekaert-panayotov`** on overlapping period to determine which crash filter is more effective
5. **Sensitivity analysis on thresholds**: are the results robust to ±1 standard deviation changes in the vol and dispersion thresholds?
