# ATH Breakout Stock Trend Following (Zarattini / Pagani / Wilcox 2025)

## Overview

Long-only trend-following strategy on individual US stocks using all-time-high (ATH) closing price breakouts as entries and ATR trailing stops as exits. Zarattini, Pagani, and Wilcox (SSRN 5084316, January 2025) revisit and extend the original 2005 white paper by Wilcox and Crittenden, applying the same framework to a survivorship-bias-free dataset of all liquid US stocks from 1950 through November 2024 (66,000+ simulated trades).

---

## Asset Class / Timeframes

- **Asset class:** US Equities (NYSE, AMEX, NASDAQ) — all liquid stocks
- **Timeframe:** Daily bars; positions held for multi-week to multi-month trends
- **Rebalancing:** Daily
- **Universe:** All liquid US stocks (survivorship-bias-free); liquidity and minimum price filters applied (exact thresholds NOT REPORTED in accessible summaries)

---

## Core Logic

1. **Entry signal:** Today's adjusted closing price equals or exceeds the stock's highest-ever adjusted closing price (a new all-time high).
2. **Execution:** Enter long at the open of the following trading day (t+1).
3. **Exit signal:** Each day, an ATR-based trailing stop is calculated and updated upward (never lowered). If price at close falls below the stop level, exit at the open of the following day.
4. **Portfolio construction:** Equal-weighted across all stocks simultaneously satisfying the entry criterion and not yet stopped out.
5. **Turnover Control (TC) algorithm:** Zarattini (2025) introduces an unspecified proprietary algorithm to mitigate the high daily turnover inherent in this approach, which the authors claim makes the strategy viable across all portfolio sizes even after fees. Mechanics of the TC algorithm are NOT REPORTED in accessible sources — this is a Concretum Group proprietary element.

---

## Economic Rationale

Stock returns follow an extremely fat-tailed (power-law / Pareto) distribution: a small number of exceptional "outlier" stocks (Amazon 1997–2024, Netflix 2002–2021, etc.) generate the vast majority of long-run market returns, while the majority of individual stocks return less than T-bills over their lifetime (Bessembinder 2018). Trend following via ATH breakout attempts to systematically capture these outlier winners by:

- **Entering at ATH:** New all-time highs are information revelation events — they signal that the firm is genuinely in a phase of sustained value creation, liquidity accumulation, or fundamental re-rating. Institutional capital often begins to rotate into ATH stocks (momentum cascade).
- **ATR trailing stop:** Cuts exposure quickly when price momentum reverses, preventing small profits from becoming large losses.

**When the edge should disappear:**
- Sustained bear markets or secular downtrends with no new ATH stocks (e.g., Japan 1990–2020 would produce few entry signals).
- Markets where the ATH signal is so crowded that the average entry price post-signal is already above fair value (reflexivity / momentum crowding).
- Extreme illiquidity events where bid-ask spreads and market impact on ATH entries consume the gross alpha entirely.
- Periods of mean-reverting market structure (e.g., post-dotcom 2000–2002, GFC 2008–2009) where trend-following generates whipsaw losses.

FACTS from sources. The "Bessembinder 2018" observation about individual stock returns is a directly relevant economic anchor.

---

## Entry Conditions

1. Stock passes liquidity filter (minimum daily dollar volume — exact threshold NOT REPORTED).
2. Stock passes minimum price filter (exact threshold NOT REPORTED).
3. Adjusted closing price today ≥ highest adjusted closing price in the stock's full trading history.
4. Buy at open on trading day t+1.

---

## Exit Conditions

1. Calculate ATR-based trailing stop at each daily close (the stop level can only move up, never down).
2. If adjusted closing price < trailing stop, sell at open on trading day t+1.

The ATR lookback period for the stop is stated as 10 periods in the Wilcox/Crittenden (2005) original paper; whether Zarattini (2025) preserves exactly this parameter is NOT CONFIRMED in accessible summaries.

---

## Risk Management

- ATR trailing stop is the primary risk control — defined before entry.
- Equal weighting across all active positions.
- No explicit maximum position count, maximum portfolio loss, or leverage limit mentioned in accessible sources.
- Turnover Control algorithm (proprietary, mechanics NOT REPORTED) addresses transaction cost risk.
- The highly skewed trade distribution (<7% of trades driving all profits) means the strategy requires a full portfolio of concurrent holdings to capture the outlier winners — single-stock selection defeats the mechanism.

---

## Indicators Used

- **ATH breakout:** Rolling all-time high of adjusted closing prices.
- **ATR (Average True Range):** 10-period ATR trailing stop (Wilcox/Crittenden 2005; Zarattini 2025 period NOT CONFIRMED).
- **Volume/price filters:** Liquidity filter (exact parameters NOT REPORTED).
- **Turnover Control algorithm:** Proprietary (parameters NOT REPORTED).

---

## Transaction Costs & Capacity

**Costs in original study (Wilcox/Crittenden 2005):**
- 0.5% round-turn deducted per trade (commission + slippage estimate for 2005 conditions).
- After-cost annual return 1983–2004: 19.3% (from Quantpedia summary of original paper).

**Costs in Zarattini (2025):**
- "Extensive daily turnover" explicitly identified as the key challenge.
- Without Turnover Control: strategy "not viable for portfolios under $1M AUM" (CLAIMED).
- With Turnover Control algorithm: "attractive across all tested portfolio sizes even after fees" (CLAIMED).
- Net-of-cost Sharpe, CAGR, and drawdown for the Zarattini 2025 portfolio are NOT REPORTED in accessible summaries.

**Assessment (ANALYSIS):** The 0.5% round-turn assumption from 2005 likely understates true costs for:
- Small-cap and micro-cap ATH breakout stocks (bid-ask spreads 1–3%+).
- Daily rebalancing across a large equal-weighted portfolio (market impact at scale).
- Modern HFT environment where ATH entries are contested.
- Brokerage commissions, SEC fees, and stock borrow (for risk hedging).

The fact that Zarattini (2025) acknowledges "extensive daily turnover" as a fundamental problem and introduces a proprietary TC algorithm suggests the gross strategy is not cost-viable as described — only the TC-modified version claims viability, and the TC specifics are not available for scrutiny.

**Capacity:** No explicit capacity analysis in accessible summaries. An equal-weighted portfolio across potentially hundreds of concurrent ATH-breakout stocks is a large-cap-friendly structure in theory, but the ATH filter naturally produces many small-cap names where capacity is limited.

---

## Backtest Evidence

**CLAIMED, UNVERIFIED**

- Zarattini/Pagani/Wilcox (2025): 66,000+ trades, survivorship-bias-free, 1950–Nov 2024.
- CAGR 15.02–15.19% (gross, theoretical portfolio; annual alpha 6.19%) — CLAIMED, UNVERIFIED.
- Max drawdown 31.75% — CLAIMED, UNVERIFIED.
- Sharpe ratio: NOT REPORTED.
- Net-of-cost performance: NOT REPORTED.

**Original Wilcox/Crittenden (2005) — CLAIMED, UNVERIFIED:**
- Period: 1983–2004.
- Annual return: 19.3% (after 0.5% round-turn).
- Sharpe ratio: 1.24.
- Volatility: 15.6%.
- Max drawdown: −33.74%.

---

## Forward-Test Evidence

NOT REPORTED. No live trading results or audited forward-test data accessible.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED | NOT REPORTED | Zarattini 2025 |
| Sharpe Ratio | 1.24 | CLAIMED, UNVERIFIED | Wilcox/Crittenden 2005 (Quantpedia summary) |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — (implied low: <7% of trades drive profits) |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | 31.75% | CLAIMED, UNVERIFIED | Zarattini 2025 (HarbourFront summary) |
| Maximum Drawdown | 33.74% | CLAIMED, UNVERIFIED | Wilcox/Crittenden 2005 (Quantpedia summary) |
| CAGR | 15.02–15.19% (gross) | CLAIMED, UNVERIFIED | Zarattini 2025 |
| Annual Return | 19.3% (after 0.5% costs) | CLAIMED, UNVERIFIED | Wilcox/Crittenden 2005 |
| Annual Alpha | 6.19% | CLAIMED, UNVERIFIED | Zarattini 2025 |
| Number of Trades | 66,000+ (1950–2024) | CLAIMED, UNVERIFIED | Zarattini 2025 |
| Average Trade Duration | NOT REPORTED | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED | NOT REPORTED | — |

---

## Community Sentiment

**Practitioner reception (positive):** The Wilcox/Crittenden (2005) white paper has been discussed in CTA/trend-following practitioner communities for two decades and is widely cited as foundational documentation that trend following can work at the individual stock level. Zarattini/Pagani/Wilcox (2025) is positioned as the authoritative 20-year OOS update.

**Quantpedia:** Lists the underlying strategy (Trend-Following Effect in Stocks) in its strategy database.

**Sceptical observations from community:**
- "Standard trend following is not expected to work with stocks since their correlation is too high" — this critique applies to index-level timing, not individual ATH selection, so it slightly misidentifies the mechanism. However, the high intra-portfolio correlation during equity market drawdowns remains a genuine risk.
- The acknowledgement of "extensive daily turnover" and the need for a proprietary TC algorithm to make costs manageable is itself the most credible signal from the authors that the base strategy is NOT cost-viable as described.

**Independent replication:** No independent academic replication of the Zarattini 2025 update found. The original 2005 white paper has been cited but not independently peer-reviewed. No open code repository found.

---

## Why This Might Be Nothing

**1. Transaction cost annihilation (most likely benign explanation):**
The authors themselves acknowledge "extensive daily turnover." The TC algorithm is proprietary and its mechanics are not disclosed. The net-of-cost Sharpe and CAGR for the 2025 portfolio are NOT REPORTED. This pattern — gross results available, net-of-cost results absent — is a significant red flag. The most parsimonious explanation is that the TC-unadjusted strategy fails after costs, and the TC algorithm's viability rests on implementation details that cannot be verified. The 6.19% annual alpha (gross) would need to survive round-trip costs of potentially 1–3%+ per trade for small-cap ATH stocks, plus daily rebalancing friction across the entire portfolio.

**2. Post-publication decay:**
The authors note "a modest decline in average trade profitability following the original publication" in the 2005–2024 OOS period. This is the expected post-publication pattern when a strategy becomes known to the market. The original 19.3% CAGR (1983–2004) versus 15.02% CAGR (1991–2024, theoretical, gross) suggests meaningful attenuation. Whether the remaining net edge is economically significant after costs is the unanswered question.

**3. Look-back period and regime luck:**
The 1991–2024 theoretical portfolio overlaps substantially with the greatest US equity bull market in history (1990s boom, post-GFC 2009–2021 recovery, 2022–2024 AI boom). A long-only ATH breakout strategy is by construction long this secular tailwind. The 1950–1990 portion of the 74-year dataset may behave very differently (pre-computer trading, much wider spreads, smaller investable universe).

**4. Illiquidity premium, not momentum premium:**
Many ATH breakout stocks (especially in the early part of the dataset) are small-cap names. Bessembinder (2018) shows most US stocks underperform T-bills — but the few that outperform dramatically are dominated by large-cap growth names that also set ATHs persistently. If the strategy's returns are simply compensation for illiquidity (small-cap premium) rather than momentum alpha, the risk-adjusted returns after appropriate illiquidity adjustment may be much lower than reported.

**5. Single piece of evidence that would most change my mind:**
An independently audited, net-of-cost Sharpe ratio ≥ 0.5 for the 2005–2024 OOS period, computed by a third party using the same survivorship-bias-free data, with the Turnover Control algorithm's exact specification disclosed for replication.

---

## Strengths / Weaknesses

**Strengths:**
- Extremely simple, transparent entry/exit rules (ATH breakout + ATR stop).
- Survivorship-bias-free dataset over 74 years — methodologically superior to most retail backtests.
- Genuine 20-year OOS period (2005–2024) built-in via the original 2005 publication date.
- The <7% trade skew is economically motivated (Bessembinder 2018 documents this empirically for stock returns).
- ATR trailing stop is a legitimate, well-understood risk control.
- Equal-weighted portfolio provides natural diversification across ATH stocks.

**Weaknesses:**
- Net-of-cost performance NOT REPORTED — the fundamental viability question is unanswered.
- Turnover Control algorithm is proprietary; cannot be independently verified.
- Sharpe ratio for the 2025 study NOT REPORTED.
- Long-only strategy with no hedge: full-market-downturn drawdowns expected.
- Portfolio correlation is high during market crises — all long positions suffer simultaneously.
- "Modest decline in profitability" OOS is concerning.
- No academic peer review.
- Implementation complexity increases with portfolio size (market impact at scale).

---

## Confidence Score

| Dimension | Score | Weight | Weighted |
|---|---|---|---|
| Logical/economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 6 | 2 | 12 |
| Evidence auditability | 4 | 2 | 8 |
| Risk management quality | 5 | 2 | 10 |
| Cost & capacity realism | 3 | 2 | 6 |
| Honest treatment of drawdowns/failure | 6 | 1.5 | 9 |
| Robustness evidence (OOS/walk-forward) | 6 | 1.5 | 9 |
| Survived independent scrutiny | 4 | 1 | 4 |

**Weighted sum:** 76 / 150
**Latent score:** round(76/150 × 100) = **51**
**Verification cap:** Evidence auditability = 4/10 → cap at 59
**Capped confidence:** min(51, 59) = **51**

**Record:** latent 51 (capped to 51; cap would be 59 but latent is already below ceiling; cap does not bind; pending peer-reviewed replication and net-of-cost disclosure)

**Band:** Experimental (40–59)

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low (ATH breakout + ATR trailing stop). Basic version is straightforward to implement.
- **Scalability:** Limited — the ATH breakout filter naturally surfaces many small-cap names where slippage and market impact are high. Large portfolios face capacity constraints in illiquid names.
- **Automation:** Feasible. Daily bar data + ATH comparison + ATR calculation is simple. Automation requires handling stock additions/delistings (survivorship bias) and the TC algorithm (which is proprietary).
- **Data requirements:** Survivorship-bias-free daily OHLCV + adjusted closes for all US stocks (e.g., Norgate Data, Compustat, CRSP, or similar — not free).

---

## MQL5 / Python / Pine Feasibility

- **Python:** Yes — daily ATH breakout + ATR trailing stop can be implemented in pandas/vectorbt/backtesting.py. Full survivorship-bias-free universe requires CRSP or Norgate Data (paid). PyFolio or Quantstats for performance reporting.
- **PineScript (TradingView):** Feasible for individual stocks but NOT for portfolio-level implementation (TradingView cannot simulate cross-stock portfolio strategies).
- **MQL5:** Feasible for single-instrument implementation (not for stock portfolio management at scale; MetaTrader is primarily FX/CFD oriented).
- **QuantConnect/Lean:** Best platform for this strategy — provides free survivorship-bias-corrected US equity data, daily rebalancing, equal-weight portfolio construction, and realistic transaction cost modeling.

---

## Similar Strategies

- [`industry-trend-century`](industry-trend-century.md) — Same lead author (Zarattini); same ATH/Donchian/Keltner trend entry concept but applied to 48 Fama-French industry portfolios rather than individual stocks. Score: 64 (Worth Researching). The industry version avoids individual stock transaction cost problems.
- [`intraday-momentum-spy`](intraday-momentum-spy.md) — Different timeframe but same author (Zarattini). Intraday momentum on SPY ETF. Score: 50.
- [`fast-alphas-intraday-overlay`](fast-alphas-intraday-overlay.md) — Zarattini/Pagani 2026; intraday overlay on ATR trend signals. Score: 55.
- [`orb-stocks-in-play`](orb-stocks-in-play.md) — Also individual US stocks; different entry mechanism (Opening Range Breakout on high-volume catalyst stocks); different alpha source. Score: 78.

---

## Red Flags

- Net-of-cost performance NOT REPORTED (primary concern).
- Turnover Control algorithm is proprietary (Concretum Group) — key to viability but not disclosable.
- No Sharpe ratio in the 2025 study.
- "Extensive daily turnover" acknowledged — strategy is costs-dependent in a regime where the TC algorithm mechanics cannot be verified.
- Moderate post-publication decay ("modest decline in profitability").
- Long-only: full exposure to bear markets (31–34% max drawdown confirmed).

---

## Source Links

| Source | URL | Retrieved |
|--------|-----|-----------|
| SSRN 5084316 (Zarattini/Pagani/Wilcox 2025) | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5084316 | 2026-06-19 |
| Concretum Group strategy page | https://concretumgroup.com/does-trend-following-still-work-on-stocks/ | 2026-06-19 |
| HarbourFront Quant Substack review | https://harbourfrontquant.substack.com/p/does-trend-following-still-work-on | 2026-06-19 |
| Quantpedia — Trend Following in Stocks | https://quantpedia.com/strategies/trend-following-effect-in-stocks/ | 2026-06-19 |
| Wilcox/Crittenden 2005 original (CIS Penn PDF) | https://www.cis.upenn.edu/~mkearns/finread/trend.pdf | 2026-06-19 |
| The Idea Farm summary | https://theideafarm.com/alternative-investment/trend-following/does-trend-following-still-work-on-stocks/ | 2026-06-19 |

---

## Analyst Notes

**FACTS (from sources):**
- Entry rule: Close ≥ all-time adjusted-close high → buy at open next day.
- Exit rule: 10-period ATR trailing stop (never lowered) → sell at open next day if price < stop.
- Portfolio: Equal-weighted, daily-rebalanced, all qualifying stocks simultaneously.
- Dataset: Survivorship-bias-free US equities, 1950–Nov 2024, 66,000+ trades.
- Trade distribution: <7% of trades account for cumulative profitability.
- CAGR 15.02–15.19% gross, alpha 6.19%, max DD 31.75% (1991–2024 theoretical portfolio) — CLAIMED.
- Original (1983–2004): Return 19.3%, Sharpe 1.24, Volatility 15.6%, Max DD −33.74% (after 0.5% round-turn costs) — CLAIMED.
- OOS performance (2005–2024) shows "modest decline" in profitability vs. in-sample.
- "Extensive daily turnover" acknowledged by authors; Turnover Control algorithm introduced.

**ANALYSIS (my reasoning):**
- The gross CAGR decline from 19.3% (1983–2004) to 15.02–15.19% (1991–2024) is partly a longer period effect and partly post-publication decay.
- With no Sharpe ratio for the 2025 study, it is impossible to assess whether the CAGR premium compensates for the ~32% max drawdown risk.
- Implied Sharpe from CAGR ~15% and max DD ~32%: Calmar = 15/32 = 0.47 (not using the usual Calmar formula exactly, but heuristic). This is acceptable but not exceptional for a systematic equity strategy.
- For a 20-year OOS period showing "modest decline," the compound effect is material: if the pre-2005 Sharpe was 1.24 and has "modestly declined," the current Sharpe is likely below 1.0 — probably 0.6–0.8 if costs are already embedded (as in the original 0.5% round-turn).
- The TC algorithm being proprietary is the core obstacle to independent evaluation.
- The `industry-trend-century` paper by the same author (Zarattini) achieves very similar gross metrics (CAGR 18.2%, Sharpe 1.39, Max DD 33%) on industry indices with much lower turnover. This makes individual-stock ATH breakout look inferior to the industry version on a risk-adjusted, cost-adjusted basis — the industry approach avoids most of the transaction cost problems of individual stock rebalancing.

**ASSUMPTIONS:**
- Assuming 10-period ATR is unchanged from original Wilcox/Crittenden 2005 in the Zarattini update (NOT CONFIRMED from accessible sources).
- Assuming equal weighting in Zarattini 2025 (consistent with original paper; NOT EXPLICITLY CONFIRMED for the TC variant).

---

## Future Research Needed

1. **Primary:** Access the full SSRN PDF to extract net-of-cost Sharpe ratio, CAGR after TC algorithm, and exact TC algorithm specification.
2. Determine whether any independent researcher has replicated this strategy on a survivorship-bias-free universe and what their net-of-cost results show.
3. Compare performance by market cap decile: how much of the gross alpha comes from small-cap vs. large-cap ATH breakouts?
4. Check whether the "QaunTip: Global Tactical Asset Allocation" (Zarattini/Gabriel/Pagani, June 2026) extends or supersedes this strategy.
5. Test on non-US equity markets (Japan, EU) where the 74-year US bull market tailwind is absent.
