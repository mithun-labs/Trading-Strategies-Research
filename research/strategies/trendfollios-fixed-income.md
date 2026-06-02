# TrendFolios® Fixed Income Momentum/Trend Strategy

## Overview

A bi-weekly-rebalanced active fixed income portfolio that applies combined momentum (line-based)
and trend-following (curve-based) signals to a diversified universe of fixed income ETFs
spanning inflation protection, duration, credit quality, and currency exposures. Position sizing
uses inverse-volatility weighting. Benchmarked to the Bloomberg U.S. Aggregate Bond Index.

Source: Lu, Rojas, Yeung & Convery (2025). "TrendFolios®: A Portfolio Construction Framework
for Utilizing Momentum and Trend-Following In a Multi-Asset Portfolio." arXiv 2506.09330.
Published June 11, 2025. Not peer-reviewed. TrendFolios® is a registered trademark,
suggesting this is a live commercial strategy presented on arXiv.

---

## Asset Class / Timeframes

- **Asset class:** Fixed Income (ETF-based)
- **ETF universe (risk factors covered):**
  - Inflation protection (e.g., TIPS ETFs such as TIP, SCHP)
  - Duration (e.g., Treasury ETFs: SHY, IEF, TLT)
  - Credit quality (e.g., IG corporates: LQD; high yield: HYG/JNK likely)
  - Currency exposures (e.g., international bonds: BNDX, BWX, EMB likely)
  - *Specific ETFs not disclosed in publicly available summaries*
- **Benchmark:** Bloomberg U.S. Aggregate Bond Index
- **Rebalancing frequency:** Every 2 weeks (bi-weekly)
- **Backtest/track period:** Inception ~2001 (22+ years) through December 2023

---

## Core Logic

**Signal computation (bi-weekly):**
1. **Line-based momentum signal:** Temporal integration of relative returns and price ratios
   across multiple calendar-based timeframes (daily to annual). The "line" appears to track
   how the ETF return deviates from its benchmark return at different horizons.
2. **Curve-based trend-following signal:** Uses tracking error, spread measures, and relative
   return signals derived from price ratios. The "curve" appears to capture the curvature of
   the return path (acceleration/deceleration of trend).
3. **Composite signal:** Combines the two signal components (specific weights not disclosed).

**Portfolio construction:**
- For each fixed income risk factor, calculate its attractiveness score from the composite signal.
- Weight each factor inversely proportional to its volatility (lower-vol factors get higher weight).
- Rebalance the portfolio every 2 weeks to the new signal-weighted, volatility-adjusted allocations.

*Note: "Line-based" and "curve-based" are the authors' proprietary terminology. Specific indicator
parameters, exact ETF list, and weighting formulas are not fully disclosed in available summaries.*

---

## Economic Rationale

Fixed income momentum has a documented academic foundation:
- **Jostova et al. (2013, RFS):** Corporate bond momentum is significant at 6-month horizon;
  peer-reviewed. Explains momentum as slow information diffusion and credit analyst herding.
- **Houweling & Van Zundert (2017, FAJ):** Factor investing in investment-grade corporate bonds.
- **Asness, Moskowitz & Pedersen (2013, JF):** Momentum premium exists across 8 asset classes
  including bonds; institutional investors underreact to information.

**Rationale for TrendFolios approach:**
- Inflation/duration regime shifts take months to reverse → trend-following captures these
  multi-month cycles (e.g., rising rate environment; credit spread cycles).
- Inverse vol weighting reduces concentration in the most volatile sectors (high yield, EM).
- Bi-weekly rebalancing captures intermediate-term trends without excessive transaction costs.

**When the edge should disappear (falsifiability):**
- During sudden, large monetary policy reversals (e.g., pivots in Fed policy direction) where
  the momentum signal lags the reversal — exactly the risk that materialized in 2022's rapid
  rate hikes (3-year Sharpe = 0.04).
- In flat yield curve / zero-rate environments where fixed income factor differentiation collapses.
- If all fixed income sectors move uniformly (e.g., systemic liquidity crisis — March 2020),
  relative momentum between sectors is meaningless.
- As the strategy becomes more widely known and signals are front-run by other momentum traders.

---

## Entry Conditions

- Compute composite (momentum + trend) score for each ETF risk factor bi-weekly.
- Allocate proportional to signal strength × inverse volatility weight.
- Higher-scoring factors receive larger allocation; factors with negative/weak signals are
  underweighted or avoided (specific threshold not disclosed).

---

## Exit Conditions

- Continuous relative allocation shifts with each bi-weekly rebalance.
- No hard exit rule described; exposure remains in fixed income universe at all times.

---

## Risk Management

- **Inverse volatility weighting:** Position sized inversely to each factor's realized vol.
  Reduces concentration in high yield / EM debt relative to Treasuries/TIPS.
- **Bi-weekly rebalancing discipline:** Prevents excessive drift from signals.
- **Benchmark-relative orientation:** Strategy is tracked vs. Bloomberg US Agg →
  significant duration mismatch is monitored via tracking error.
- **No hard stop loss or maximum drawdown trigger** described in available summaries.
- **No leverage specified** — but volatility of 15–19% is higher than pure AGG (~6%),
  suggesting either leveraged ETF exposure or significant allocation to high-vol fixed income.

---

## Indicators Used

- Past X-day to annual returns for each ETF vs. benchmark (multiple timeframes)
- Tracking error relative to Bloomberg US Agg
- Credit spreads and relative spread signals
- Realized volatility per ETF (for inverse-vol position sizing)

---

## Transaction Costs & Capacity

- **Costs:** Bi-weekly rebalancing in ETFs incurs 26 rebalancing events per year. For liquid
  fixed income ETFs (AGG, LQD, TIP), bid-ask spread is typically 0.01–0.05%. Total annual
  trading costs estimated at < 0.5% for liquid ETFs, potentially higher for HYG or EMB.
- **Source claims net returns:** Performance figures described as "composite net return"
  suggest management fees and transaction costs are deducted. However, the exact fee
  structure and cost assumptions are not disclosed.
- **Capacity:** ETF universe implies large capacity for institutional implementations (top
  fixed income ETFs have >$10B+ AUM). No capacity analysis provided.
- **High volatility concern:** 17–19% annualized composite volatility is unusually high for
  a pure fixed income strategy. This either reflects leverage, inclusion of high-yield and EM
  debt, or the composite metrics represent the full multi-asset TrendFolios strategy rather
  than only the fixed income component. ANALYSIS: If the 17% vol is for the composite
  multi-asset strategy rather than the fixed income sub-strategy, the performance attribution
  to the "fixed income strategy" needs reinterpretation.

---

## Backtest / Track Record Evidence

- **Period:** ~2001–2023 (22+ years; arXiv paper uses "inception" without specifying start date)
- **Status:** `CLAIMED, UNVERIFIED` — arXiv preprint; no peer review; no open code.
  The "TrendFolios®" trademark and GIPS-style reporting conventions suggest this may
  represent live performance composite data rather than a pure backtest. However, the
  distinction cannot be confirmed without full paper access.
- **3-year Sharpe = 0.04 (2021–2023):** Explicitly reported. This period includes the
  2022 rate shock — the most severe bond bear market in modern history (Bloomberg US Agg −13.0%
  in 2022). The near-zero Sharpe is the most important stress-test data point available.

---

## Forward-Test Evidence

- Not separately reported (performance data runs through Dec 2023 — end of the study period).
- Status: `NOT REPORTED`

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 0.65 (1Y); 0.04 (3Y); 0.54 (5Y); 0.33 (10Y); 0.33 (since inception) | CLAIMED, UNVERIFIED | TrendFolios arXiv 2506.09330 |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | 12.09% (1Y); 0.73% (3Y); 10.22% (5Y); 5.22% (10Y); 5.79% (since inception) | CLAIMED, UNVERIFIED | TrendFolios arXiv 2506.09330 |
| Annual Return | See CAGR row; per-year breakdown NOT REPORTED | NOT REPORTED | — |
| Number of Trades | ~26 rebalancing events/year (biweekly; implied by rules) | NOT REPORTED (estimated) | — |
| Average Trade Duration | ~2 weeks (implied by biweekly rebalancing) | NOT REPORTED (implied) | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% (always in fixed income ETFs; no cash option described) | NOT REPORTED (implied) | — |

**Benchmark excess returns (CLAIMED, UNVERIFIED) vs. Bloomberg US Aggregate Bond Index:**
- 1Y: +10.01%; 3Y: +6.46%; 5Y: +11.59%; 10Y: +5.90%; Since inception: +5.89% (annualized)

**Annualized volatility:**
- 1Y: 18.59%; 3Y: 17.41%; 5Y: 19.03%; 10Y: 15.82%; Since inception: 17.49%

**Information ratio:** 0.66 (1Y); 0.45 (3Y); 0.67 (5Y); 0.39 (10Y); 0.34 (since inception)

**Note on volatility:** 15–19% annualized vol is inconsistent with a pure fixed income strategy
(Bloomberg US Agg ~3–6% vol). This level is consistent with a multi-asset composite or with
significant high-yield/EM debt and/or leverage exposure. Cannot clarify without full paper access.

---

## Community Sentiment

- No academic community discussion found. arXiv preprint published June 2025; too new for
  peer community reaction.
- No practitioner forum discussion found in available searches.
- The TrendFolios® trademark suggests this is a commercial strategy presentation on arXiv
  rather than a traditional academic research paper. Authors' institutional affiliations
  are not stated in publicly available summaries.
- The "Duration Rotation in U.S. Treasury Fixed-Income ETFs" paper (MDPI FinTech 2025) is
  in the same space and is peer-reviewed; it finds evidence for a "median duration" rotation
  strategy in US Treasuries, providing independent confirmation that duration-based fixed
  income factor rotation can add value.

---

## Why This Might Be Nothing

**Steelman of the skeptical case (written before scoring):**

1. **The 3-year Sharpe of 0.04 is the most important data point.** The period 2021–2023
   included the fastest Fed rate hiking cycle since the 1980s. A momentum/trend strategy
   in fixed income that was long duration assets heading into 2022 would have suffered
   severe drawdowns. A 3-year Sharpe of 0.04 suggests the strategy barely outperformed
   cash over this stress period — exactly when investors would most want active management
   to add value. The "since inception" excess return may be dominated by regime-specific
   conditions in pre-2020 low-rate era.

2. **The volatility of 17–19% is structurally inconsistent with fixed income.** This is
   equity-portfolio-level volatility. If the composite includes high yield, leveraged loans,
   or EM bonds with leverage, the strategy is bearing equity-correlated credit risk and
   calling it "fixed income momentum." During risk-off events (March 2020, March 2023 SVB
   crisis), these exposures would move adversely simultaneously.

3. **Vague, proprietary signals cannot be independently verified or reproduced.** "Line-based"
   and "curve-based" are not industry-standard terms. Without the full paper's appendix
   (paywalled or access-restricted despite arXiv status), the signal cannot be replicated.
   A strategy with non-reproducible signals cannot be validated independently.

4. **"6.48% annual excess return over Bloomberg US Agg" is partly a 2022 artifact.**
   Bloomberg US Agg returned approximately −13.0% in 2022. Any strategy that avoided
   long-duration Treasuries in 2022 would show large excess return vs. AGG. The "since
   inception" excess return figure may be dominated by this one-year avoidance of the worst
   bond crash in decades — not a repeatable skill.

5. **Not peer-reviewed; no open code; proprietary trademark.** The paper appears to be a
   commercial pitch on arXiv rather than a rigorous academic contribution. The arXiv venue
   is being used for marketing, not peer-reviewed research.

6. **No maximum drawdown reported.** Given the 17–19% volatility, the maximum drawdown is
   likely in the 25–40% range for the worst period (2022 would be the candidate). Without
   this figure, the risk profile is incomplete and cannot be assessed.

**Single most decisive missing piece:** Maximum drawdown over the 2022 rate shock period,
plus the specific ETF universe and signal indicator formulas to enable independent replication.
These would confirm or refute whether the strategy genuinely managed rate risk or was lucky
in its timing.

---

## Strengths / Weaknesses

**Strengths:**
- Academic backing for fixed income momentum exists in peer-reviewed literature.
- Inverse volatility weighting is a well-studied, validated technique.
- 22+ year track record claimed (if live performance, not pure backtest).
- Bi-weekly rebalancing balances responsiveness with low turnover.
- Diversified ETF universe (inflation, duration, credit, currency) provides natural hedging.
- Information ratio of 0.34–0.67 (depending on period) is positive and indicates persistent
  benchmark outperformance (if CLAIMED metrics are accurate).

**Weaknesses:**
- Not peer-reviewed; arXiv preprint only.
- "Line-based" and "curve-based" signals are proprietary and non-reproducible.
- 3-year Sharpe 0.04 = near-zero performance during the key stress test (2022 rate shock).
- Volatility 17–19% is inconsistent with pure fixed income strategy — unclear what risk
  is actually being taken.
- Max drawdown NOT REPORTED.
- No OOS split or walk-forward test described.
- Appears to be a commercial strategy seeking clients/AUM via arXiv, not academic research.
- ETF universe not disclosed; cannot audit position concentration.

---

## Confidence Score

| Dimension | Raw (0–10) | Weight | Weighted |
|---|---|---|---|
| Logical / economic soundness (falsifiable) | 5 | 3 | 15 |
| Transparency & reproducibility | 3 | 2 | 6 |
| Evidence auditability | 2 | 2 | 4 |
| Risk management quality | 6 | 2 | 12 |
| Cost & capacity realism | 5 | 2 | 10 |
| Honest treatment of drawdowns / failure | 4 | 1.5 | 6 |
| Robustness evidence (OOS / walk-forward) | 4 | 1.5 | 6 |
| Survived independent scrutiny | 1 | 1 | 1 |
| **Total** | | **15** | **60** |

`latent_score = round(60 / 150 × 100) = 40`

**Evidence auditability = 2 → cap at 59**
`confidence = min(40, 59) = **40** (Experimental)`

Recorded as: `latent 40 (capped to 40 pending verification: arXiv preprint only; proprietary signals; 3Y Sharpe 0.04; volatility 17-19% inconsistent with pure fixed income; max drawdown not reported; no peer review)`

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Moderate. Bi-weekly rebalancing across ETF universe is automatable;
  but the specific signal formulas are not disclosed.
- **Scalability:** High — ETF-based, liquid instruments.
- **Automation:** Cannot automate without full signal specification from the paper.

---

## MQL5 / Python / Pine Script Feasibility

- **Python:** Basic momentum-on-ETFs implementation is straightforward (returns + vol);
  but cannot replicate "line-based" and "curve-based" proprietary signals without paper access.
- **Pine Script:** Implementable as generic momentum on fixed income ETFs (not tradeable
  on TradingView for most fixed income instruments at scale).
- **MQL5:** Some brokers offer fixed income ETF CFDs; limited universe.

---

## Similar Strategies

- `gold-cross-asset-regimes` — Related concept: asset momentum for a single fixed income
  instrument (IEF) as a filter; different mechanism.
- `hmm-rl-regime-taa` — Multi-asset rotation including TLT (long-duration Treasury ETF);
  similar fixed income exposure in a different framework.
- `vix-cmf-ml-walkforward` — VIX CMF (a fixed income adjacent instrument) with ML signals.

---

## Red Flags

- `INSUFFICIENT VERIFICATION` — arXiv preprint only; no peer review; proprietary signals.
- `VAGUE METHODOLOGY` — "Line-based" and "curve-based" are non-standard; cannot reproduce.
- `3-YEAR SHARPE 0.04` — Near-zero performance during 2022 rate shock stress test.
- `VOLATILITY INCONSISTENCY` — 17-19% vol inconsistent with stated fixed income focus.
- `COMMERCIAL PITCH` — TrendFolios® trademark suggests marketing use of arXiv rather than
  genuine academic research contribution.

---

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| Lu, Rojas, Yeung, Convery (2025). "TrendFolios®: A Portfolio Construction Framework..." arXiv 2506.09330 (https://arxiv.org/abs/2506.09330) | 2026-06-02 | Primary source; arXiv abstract accessible; full paper also on arXiv but blocked |
| Web search summary — TrendFolios performance metrics | 2026-06-02 | Sharpe ratios, CAGR, excess returns, IR extracted from search summaries |
| Jostova et al. (2013, Review of Financial Studies) — Corporate bond momentum | Background | Academic support for fixed income momentum |
| Houweling & Van Zundert (2017, FAJ) — Factor investing in IG bonds | Background | Academic support for fixed income factor strategies |
| MDPI FinTech 5(2):29 — "Duration Rotation in US Treasury Fixed-Income ETFs" (https://doi.org/10.3390/fintech5020029) | 2026-06-02 | Related peer-reviewed paper on treasury duration rotation |

---

## Analyst Notes

**FACTS (from sources):**
- Strategy uses combined line-based momentum + curve-based trend signals on fixed income ETFs
- Bi-weekly rebalancing; inverse volatility position sizing
- Performance through Dec 2023: Sharpe 0.33 (since inception), 0.04 (3Y), 0.65 (1Y)
- Annualized excess returns vs. Bloomberg US Agg: 5.89% since inception, 6.46% (3Y), 10.01% (1Y)
- Annualized volatility 15-19% (consistently across periods)

**ANALYSIS (my reasoning):**
- The 3-year Sharpe of 0.04 is the most informative data point: during the 2022 rate shock
  (worst bond bear market in history), the strategy's excess return claim (6.46% vs. AGG)
  implies it outperformed a −13% AGG year — but the absolute return of 0.73% annualized over
  3 years and Sharpe of 0.04 means it barely made money in absolute terms. The strategy
  survived but didn't thrive.
- The 17–19% volatility anomaly: assuming Bloomberg US Agg has ~5% annual vol, TrendFolios'
  17–19% is 3–4× higher. This is consistent with a strategy that either:
  (a) Concentrates in high-yield/EM bonds with much higher vol, or
  (b) Reports composite multi-asset metrics rather than just fixed income
  Either way, the risk profile is significantly higher than "fixed income" implies.
- Implied Calmar: If max DD is ~25–30% (assuming from 17–19% vol and 0.33 Sharpe), and CAGR
  is ~5.79% since inception, then Calmar ≈ 0.19–0.23 — below industry thresholds for quality
  fixed income management. (This is ANALYSIS, not a fact from sources.)

**ASSUMPTIONS (flagged):**
- Assumes the performance data represents the fixed income sub-strategy, not the full composite.
- Assumes "TrendFolios® Fixed Income" uses ETFs (not individual bonds or derivatives).
- Assumes transaction costs are appropriately captured in net returns.

---

## Future Research Needed

1. Obtain full arXiv paper to identify specific ETF universe, exact signal formulas, and
   whether the 17–19% volatility is for the fixed income sub-strategy or the composite.
2. Check whether there's a GIPS-compliant performance record or firm track record behind
   TrendFolios® (the trademark suggests a live strategy).
3. Evaluate the related peer-reviewed "Duration Rotation in US Treasury ETFs" paper (MDPI
   FinTech) as a cleaner, peer-reviewed alternative for fixed income duration momentum.
4. Compare to simpler fixed income momentum strategies (e.g., hold the best-performing of
   TLT/IEF/SHY/TIP over trailing 3-6 months) as a baseline for the claimed excess returns.
5. Obtain 2024–2026 live performance to assess how the strategy has fared since the paper's
   data cutoff.
