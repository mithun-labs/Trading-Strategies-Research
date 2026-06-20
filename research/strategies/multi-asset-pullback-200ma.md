# Multi-Asset Pullback Strategy (200-MA Uptrend + Consecutive Down-Day Trigger)

## Overview

A short-term mean-reversion strategy applied across six major asset-class ETFs. Buys any ETF that is in a confirmed long-term uptrend (price > 200-day simple moving average) AND has declined for N consecutive days. The position is closed after exactly one trading day. The strategy is essentially a cross-asset ETF extension of Larry Connors' RSI(2) framework — the consecutive-down-days trigger is a discrete analog of an extreme oversold RSI(2) reading. Quantpedia discovered the specific parameterisation via a 600-backtest AI-assisted grid search.

**Source:** Quantpedia blog — "Testing an AI-Assisted Research Workflow for Multi-Asset Pullback Strategy Discovery" (Soňa Beluská, June 19, 2026)

---

## Asset Class / Timeframes

- **Instruments:** 6 liquid ETFs — SPY (US equities), EEM (emerging markets), IEF (7–10yr Treasuries), FXE (Euro FX), GLD (gold), DBC (broad commodities)
- **Timeframe:** Daily bars; 1-day holding period
- **Markets:** Multi-asset (equities, fixed income, FX, gold, commodities)
- **Data period:** February 2006 – December 2025 (≈20 years)

---

## Core Logic

1. **Trend filter:** For each ETF, check if closing price > 200-day simple moving average. If below, no trade — the asset is in a downtrend where pullbacks may represent informed selling rather than recoverable liquidity shocks.
2. **Pullback trigger:** Count the number of consecutive calendar days on which the ETF posted a negative daily return. Trigger fires when the count reaches N (tested: 2, 3, 4, 5; recommended: 3).
3. **Entry:** Buy at close on the trigger day.
4. **Exit:** Sell at close of the next trading day (exactly 1-day hold, regardless of outcome).
5. **Position sizing — dynamic equal-weight:** Allocate 100% ÷ N_active across all ETFs simultaneously triggering on that day. Capital is concentrated only on the day(s) a signal fires.
6. **Position sizing — vol-targeted variant:** Each ETF allocation = 10% ÷ realized_vol (20-day lookback), capped so total exposure ≤ 100%.

**Recommended specification** (per the source article): MA200 + 3-consecutive-down-days + dynamic equal-weight.

---

## Economic Rationale

The mechanism rests on the hypothesis that multi-day declines in uptrending assets are disproportionately driven by transient, non-informational selling pressure — forced de-risking by systematic models, month-end margin calls, leveraged-fund redemptions, or short-term liquidity shocks from ETF creation/redemption flows — rather than fundamental reassessment of value. In an asset that remains above its 200-day MA (a durable uptrend), the market-clearing direction of least resistance is recovery.

**When the edge should disappear (falsifiability):**
- When the 200-day MA is broken: the filter explicitly removes this regime. In bear markets, consecutive declines increasingly represent informed sellers, not liquidity shocks.
- When bid-ask spreads spike to crisis levels (VIX > 50): the gross reversion edge may be smaller than the round-trip cost.
- When forced-selling mechanics change structurally: if systematic funds reduce margin use or daily de-risking rules change, the source of recoverable supply disappears.
- In assets with idiosyncratic fundamental deterioration: the 200-MA filter only catches regime breaks at the asset level, not issue-specific news (e.g., FXE would not filter during a EUR zone crisis if it hadn't yet crossed below MA).

The rationale is plausible but **not exclusive to this parameterisation** — the Connors RSI(2) literature documents the same mechanism applied to stocks and ETFs dating to 1996. The 200-day MA as the trend filter is a practitioner convention without unique theoretical necessity.

---

## Entry Conditions

Long only.

```
for each ETF in {SPY, EEM, IEF, FXE, GLD, DBC}:
    if close > SMA(200):
        consecutive_down = count of consecutive days with close < prior_close
        if consecutive_down >= 3:
            open long at today's close
            N_active += 1

allocate 100% / N_active to each triggered position
```

Variant (vol-targeted): replace uniform size with `min(10% / realized_vol_20d, 100%)`.

---

## Exit Conditions

Close all positions at the close of the next trading day (1-day holding period). No stop loss within the hold window. Position exits unconditionally after 24 hours regardless of P&L.

---

## Risk Management

- **200-day MA filter:** Principal drawdown protection — prevents trading against bear-market trend
- **1-day maximum hold:** Implicit time-stop prevents losses from compounding beyond 1 day
- **Dynamic equal-weight:** Prevents over-concentration in any single asset on active days
- **Vol-targeting (optional):** Scales exposure inversely to recent realized volatility; reduces position size during volatile periods
- **No explicit stop loss** within the holding window (held regardless of intraday direction)
- **No leverage** in baseline specification; vol-targeted version may apply leverage if realized vol is low

---

## Indicators Used

- 200-day simple moving average (SMA200)
- Consecutive negative daily return count
- Optional: 20-day realized volatility (vol-targeted variant)

---

## Transaction Costs & Capacity

**Costs NOT modeled in the source analysis.** This is the most significant omission.

ANALYSIS (not in the source):
- The 3-day variant generates approximately 1,030 trades/year across 6 ETFs → ~172 trades/year per ETF.
- Bid-ask round-trip costs for these ETFs: SPY ~0.5bps, IEF ~1bp, GLD ~1bp, EEM ~2–3bps, FXE ~3–5bps, DBC ~5–8bps. Weighted average: ~2–3bps/round-trip.
- Total friction per year: 172 trades/ETF × 3bps × 6 ETFs = ~3% drag on invested capital. Applying the 21% invested-day ratio (capital deployed only 21% of days) reduces this to ~0.63% drag on total portfolio per year.
- Against gross CAGR ~8.5%, this leaves net CAGR ~7.9% — meaningful reduction but not fatal for liquid ETFs.
- For the vol-targeted variant, the higher turnover (2-day variant) at ~2,152 trades/year would approximately double the friction, reducing net CAGR from 6.80% to ~6.1%.
- **Capacity:** ETFs have high capacity; strategy size is not a constraint. The 6-ETF universe limits diversification but eliminates liquidity concerns.
- **Slippage:** 1-day hold with ETF entry/exit at close (MOC orders) adds minimal slippage for liquid ETFs.

CONCLUSION (ANALYSIS): The strategy likely survives realistic transaction costs on these ETFs, but the net Sharpe would be approximately 0.65–0.75 vs. the reported 0.78 gross. This is a better cost situation than most short-term strategies reviewed in this database.

---

## Backtest Evidence

Hypothetical backtest on 6 ETFs, February 2006 – December 2025 (~20 years). Source: EODHD.com daily data. Conducted by Quantpedia junior analyst using AI-assisted parameter grid search (600 backtests). No independent replication found.

---

## Forward-Test Evidence

NOT REPORTED. No live trading results or paper trading results cited.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 0.78 (3-day) / 0.95 (2-day) / 0.98 (vol-targeted 2-day) | CLAIMED, UNVERIFIED | Quantpedia/Beluska 2026 |
| Sortino Ratio | 0.86 (vol-targeted 2-day) | CLAIMED, UNVERIFIED | Quantpedia/Beluska 2026 |
| Calmar Ratio | 0.39 (3-day) / 0.36 (2-day) | CLAIMED, UNVERIFIED | Quantpedia/Beluska 2026 |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | 58.5% average (3-day); 55%+ per ETF | CLAIMED, UNVERIFIED | Quantpedia/Beluska 2026 |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | −13.69% (3-day) / −25.11% (2-day) / −17.12% (vol-targeted) | CLAIMED, UNVERIFIED | Quantpedia/Beluska 2026 |
| CAGR | ~8.5% (3-day) / 8.96% (2-day) / 6.80% (vol-targeted) | CLAIMED, UNVERIFIED | Quantpedia/Beluska 2026 |
| Annual Return | Same as CAGR above | CLAIMED, UNVERIFIED | Quantpedia/Beluska 2026 |
| Number of Trades | ~1,030/year (3-day); ~2,152/year (2-day) | CLAIMED, UNVERIFIED | Quantpedia/Beluska 2026 |
| Average Trade Duration | 1 day | CLAIMED, UNVERIFIED | Quantpedia/Beluska 2026 |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~21% of trading days (3-day) | CLAIMED, UNVERIFIED | Quantpedia/Beluska 2026 |

---

## Community Sentiment

No independent academic or community analysis found at time of research. The Quantpedia article is internally produced; no external discussion threads, forum commentary, or independent replication have been located.

The broader class of "mean reversion within uptrend" strategies has extensive practitioner discussion:
- Larry Connors' RSI(2) strategy (same mechanism, stock-level) shows documented performance decay after his 2008 book publication — a relevant cautionary analogue.
- TradingView open-source script for "ETF 3-Day Reversion Strategy" confirms the idea is in the public domain, though community performance discussion is mixed.

---

## Why This Might Be Nothing

**1. Massive multiple-testing problem (most likely benign explanation).**
600 backtests were run across a grid of MA lengths {50, 75, 100, 150, 200} × down-day counts {2, 3, 4, 5} × hold periods {1, 2, 3, 5, 7} × 6 assets = 600 combinations. The Deflated Sharpe Ratio (Bailey/Lopez de Prado) methodology penalises the Sharpe ratio substantially when many strategies were tested before selecting a winner. With 600 independent trials and T=5,000 daily observations, the expected maximum Sharpe from strategies with zero true edge can reach 0.5–0.7 purely from sampling noise. The reported 0.78 gross Sharpe for the "recommended" variant barely clears this noise floor. The "AI-assisted" workflow did not apply any multiple-testing correction before selecting the final strategy.

**2. The Connors RSI(2) decay precedent.**
This strategy is mechanically equivalent to the RSI(2) framework Larry Connors documented in his 2008 book "Short Term Trading Strategies That Work." Connors' RSI(2) on SPY and equity ETFs showed documented performance decay after the publication date as the strategy became widely known and crowded. A cross-asset extension in 2026, built on the same mechanism, faces the same publication effect. The underlying ETF microstructure (arbitrage desks, algorithmic market makers) has also become more efficient since 2008.

**3. Regime concentration in the 2021–2025 best sub-period.**
The sub-period analysis shows 2021–2025 as the strongest period (Sharpe 1.25). This is the most recent window and coincides with the period when the strategy was (implicitly) being developed. In a post-discovery period, mean reversion in uptrending ETFs benefited from the unusual post-COVID recovery trajectory (few sustained bear markets in the 6-ETF universe, with the 200-MA filter frequently engaged). This sub-period may be structurally favourable rather than representative.

**4. Transaction costs not modeled; ETF-specific costs remain unquantified.**
As computed above (ANALYSIS), estimated net-of-cost CAGR is approximately 7.9% for the 3-day variant. This is still positive, but the Sharpe ratio would decline from 0.78 to approximately 0.65–0.75. More importantly, the source article provides no confirmation of this calculation, so net viability is unconfirmed.

**5. Concentrated 6-ETF universe with limited diversification.**
Six ETFs means approximately 6 independent signals. Sub-period analysis is conducted on the same 6 ETFs used for parameter discovery. There is no test on held-out or alternative instruments. A truly out-of-sample test would apply the identified parameters (MA200 + 3 down days + 1-day hold) to a new, untested ETF universe.

**The single piece of evidence that would most change my view:** An independent replication on a held-out universe of ETFs (e.g., sector SPDRs, international equity ETFs) using only the single recommended specification (MA200 + 3-day + 1-day hold) — with no parameter re-optimisation — accompanied by realistic bid-ask costs. If this replication achieves Sharpe ≥ 0.5, the multiple-testing concern is substantially reduced.

---

## Strengths / Weaknesses

**Strengths:**
- Clear, fully codeable rules; no ambiguity in signal definition
- 20-year history across 6 asset classes providing cross-asset validation
- 200-day MA filter directly addresses the most important failure mode (bear markets)
- Economic rationale (liquidity shocks in uptrending assets) is externally supported by ETF flow literature
- 21% invested time creates "free capital" for alternative strategies; low opportunity cost
- Sub-period analysis shows consistently positive Sharpe across all four 5-year windows
- Transaction costs on the specific ETFs are very low relative to most short-term strategies reviewed here
- Sortino 0.86 (vol-targeted) suggests good downside management

**Weaknesses:**
- 600-test data mining with no multiple-testing correction is the dominant methodological flaw
- No independent replication or academic peer review
- Connors RSI(2) decay precedent: identical mechanism lost edge post-publication in 2008
- No forward-test or live trading results
- Only 6 ETFs; cross-asset generalization not validated on held-out instruments
- Sub-period analysis is post-hoc, not a genuine OOS test
- Costs not modeled (though ANALYSIS above suggests survival for these specific ETFs)
- Strategy fires on only 21% of days: compounding returns are lower in practice than CAGR figure suggests for short-duration trades

---

## Confidence Score

| Dimension | Score (0–10) | Weight | Weighted |
|-----------|-------------|--------|---------|
| Logical / economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 7 | 2 | 14 |
| Evidence auditability | 2 | 2 | 4 |
| Risk management quality | 4 | 2 | 8 |
| Cost & capacity realism | 2 | 2 | 4 |
| Honest treatment of drawdowns / failure | 6 | 1.5 | 9 |
| Robustness evidence (OOS / walk-forward / multi-market) | 3 | 1.5 | 4.5 |
| Survived independent scrutiny | 1 | 1 | 1 |
| **Total** | | **15** | **62.5** |

**latent_score** = round(62.5 / 150 × 100) = **42**

**Verification cap:** Evidence auditability = 2/10 ≤ 4 → cap at **59**. Since latent 42 < 59, the cap does not bind.

**confidence = 42 (Experimental)**

`latent 42 (cap 59 does not bind since latent < cap; pending verification: evidence auditability 2/10 — blog post with 600-test AI-assisted data mining, no peer review, no independent replication)`

**Rationale for key dimension scores:**
- *Soundness 6/10:* Mechanism (forced-selling reversion in uptrend) is plausible, supported by external ETF flow literature, and falsifiable (names key failure conditions). Downgraded because the parameter selection is data-mined rather than theory-derived.
- *Transparency 7/10:* Full rules described in prose; EODHD data source stated; TradingView script referenced. Not quite 10 because the AI-assisted grid search process is not fully documented.
- *Auditability 2/10:* Quantpedia internal blog post; junior analyst author; no academic paper, no independent replication, no peer review. The methodology is described but the analysis cannot be independently verified without EODHD access.
- *Risk management 4/10:* 200-day MA filter is a meaningful risk filter. 1-day hold is an implicit time-stop. Vol-targeting available. No intraday stop loss; no maximum daily loss rule; no regime-exit rule.
- *Cost & capacity 2/10:* Costs explicitly not modeled. ANALYSIS above estimates ~0.63% annual drag on total portfolio after adjusting for 21% invested rate — probably survivable but unconfirmed.
- *Drawdowns 6/10:* Max DD shown across all variants (−13.69% to −25.11%). Sub-period analysis shows all 4 windows positive. 4+ down-day degradation documented.
- *Robustness 3/10:* Sub-periods tested but post-hoc. 6 assets across different classes. Parameter sensitivity documented. No genuine OOS split (all from same grid search period).
- *Scrutiny 1/10:* No academic or community scrutiny found. TradingView script exists but no performance discussion.

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low. Simple SMA filter + consecutive return count. No proprietary data required.
- **Scalability:** High for ETFs; not capacity-constrained. But only 6 assets limits diversification.
- **Automation:** Straightforward. Daily close check on 6 tickers; MOC order on trigger; next-day close exit. Can be fully automated.

---

## MQL5 / Python / Pine Feasibility

- **Python:** Fully feasible. Pandas/yfinance can replicate the strategy in ~50 lines of code.
- **Pine Script (TradingView):** A referenced open-source script exists ("ETF 3-Day Reversion Strategy") — directly replicable.
- **MQL5:** Feasible but requires multi-symbol support (for portfolio of ETFs); uncommon use case for MT4/5 which typically handle FX/futures.

---

## Similar Strategies

- `intraday-momentum-spy` (score 50): different mechanism (intraday momentum, not mean reversion) but same underlying asset (SPY); different timeframe
- `fast-alphas-intraday-overlay` (score 55): mean reversion overlay on SPY, intraday 5-min bars vs. daily here; more similar in spirit but different implementation
- `maxing-out-reversal-max-stocks` (score 44): weekly reversal in individual stocks filtered by MAX; similar mean-reversion-in-uptrend spirit but different universe, timeframe, and mechanism
- `hmm-rl-regime-taa` (score 44): multi-asset ETF rotation, daily; different mechanism (regime-conditional asset allocation vs. short-term pullback)

---

## Red Flags

- **600-test grid search (data mining):** Without Deflated Sharpe Ratio or similar multiple-testing correction, the selected Sharpe 0.78 is likely inflated above the true expected edge.
- **No peer review or academic backing:** Quantpedia junior analyst blog post is the sole primary source.
- **Connors RSI(2) decay precedent:** Identical mechanism documented in 2008 subsequently lost edge in equity ETFs after publication. Same decay risk applies here.
- **Transaction costs omitted:** Sub-threshold risk (costs likely survivable for these ETFs) but not confirmed.
- **Sub-period analysis is post-hoc:** All analysis uses data also used for parameter selection; no genuine OOS test.

---

## Source Links

| URL | Retrieved | Used For |
|-----|-----------|---------|
| https://quantpedia.com/testing-an-ai-assisted-research-workflow-for-multi-asset-pullback-strategy-discovery/ | 2026-06-20 UTC | Primary source (strategy rules, performance metrics, methodology) |

---

## Analyst Notes

**FACTS (from source):**
- Strategy: price > SMA(200) AND N consecutive negative daily returns → buy at close; exit next-day close
- Universe: SPY, EEM, IEF, FXE, GLD, DBC (6 ETFs)
- Data: EODHD.com, Feb 2006–Dec 2025
- 3-day variant: Sharpe 0.78, CAGR ~8.5%, max DD −13.69%, win rate 58.5%, trade count ~1,030/year, invested 21% of days
- 2-day variant: Sharpe 0.95, CAGR 8.96%, max DD −25.11%
- Vol-targeted (10%) 2-day: Sharpe 0.98, CAGR 6.80%, max DD −17.12%, Sortino 0.86
- Grid search: 600 backtests; AI tools used: Claude 3 Opus + ChatGPT 5.5
- Sub-periods: 4 five-year windows, all positive Sharpe; 2021–2025 strongest (Sharpe 1.25)
- Academic references: None explicitly cited (Connors RSI(2) not cited but conceptually identical)
- Author: Soňa Beluská, Junior Quant Analyst, Quantpedia; Published: June 19, 2026

**ANALYSIS:**
- This strategy is mechanically equivalent to the Larry Connors RSI(2) framework applied to a multi-asset ETF universe. The consecutive-down-days trigger is a discrete analog of extreme RSI(2) oversold readings.
- The Deflated Sharpe Ratio correction for 600 trials on T≈5,000 observations would likely reduce the expected true Sharpe to approximately 0.5–0.6, close to the noise floor.
- Net-of-cost Sharpe estimated at 0.65–0.75 for the 3-day variant on these liquid ETFs (see Transaction Costs section).
- The "best sub-period" (2021–2025) coincides with the discovery period — this is a standard data-mining artefact where the selected parameters perform best in the period used to select them.
- The mechanism has external support from the ETF flow / price-pressure literature: ETF redemptions and systematic de-risking do create recoverable price pressure within uptrends.

**ASSUMPTIONS:**
- Assumed EODHD adjusted close prices are accurate and split-adjusted for the period.
- Assumed the 600-test grid was not intentionally biased towards specific outcomes (i.e., the search was genuinely exploratory).
- Assumed "AI-assisted" refers to analytical support, not to additional hidden searches beyond the documented 600.

---

## Future Research Needed

1. **Independent replication on held-out ETF universe** (e.g., sector SPDR ETFs: XLK, XLF, XLP, XLV, XLE, XLI): apply only MA200 + 3-day specification with no re-optimisation and full transaction costs.
2. **Deflated Sharpe Ratio calculation** using the full set of 600 trials to establish the statistically corrected expected Sharpe.
3. **Live forward test:** Paper trade the 6-ETF portfolio with exact specifications for 12 months; compare to backtest expectations.
4. **Sub-period analysis with genuine OOS:** Optimise on 2006–2015 only; validate on 2016–2025 without re-fitting.
5. **Regime analysis:** Does the strategy performance vary by VIX regime (low/medium/high vol) or interest rate environment? If it only works in low-vol/QE regimes, it may be regime-specific luck.
