# Opening Range Breakout — Stocks In Play (ORB-SIP)

## Overview

A 5-minute Opening Range Breakout strategy applied exclusively to "Stocks in Play" — US equities
exhibiting abnormally high trading volume on a given day, predominantly driven by fundamental
news events. Researched in a peer-reviewed academic context by Zarattini, Barbon, and Aziz
(2024) and independently replicated on QuantConnect.

## Asset Class / Timeframes

- **Asset class:** US Equities
- **Timeframe:** 5-minute intraday (also tested at 15, 30, 60 min — 5-min best-performing)
- **Holding period:** Intraday (positions close at end of day)

## Core Logic

1. Each day, identify "Stocks in Play" — US equities with abnormally high trading volume
   relative to their historical baseline, primarily triggered by overnight fundamental news.
2. The first 5-minute candle after market open defines the opening range (high and low).
3. The color of the first 5-minute candle sets the permitted trade direction:
   - First candle is **green** → long positions only (no shorts).
   - First candle is **red** → short positions only (no longs).
4. Enter at the breakout of the opening range in the permitted direction.
5. Stop loss: 10% of ATR(14) from entry price.
6. Exit: End of day (time-based exit; no overnight holding).

## Economic Rationale

**FACTS (from sources):** Zarattini et al. attribute the edge to fundamental-news-driven order flow.
When a stock is "in play" due to overnight news, institutional and retail participants form a
directional bias before market open. The first 5-minute range captures the initial price
discovery; a subsequent breakout signals that directional conviction is sufficient to
overwhelm initial resistance/support. This is analogous to the empirical literature on
intraday momentum (Gao et al., 2018) showing that the first 30-minute return predicts the
rest-of-day return for high-volume stocks.

**ANALYSIS:** The volume filter is the key differentiator. Without it, ORB applied to the
general stock universe shows weak or no edge. Restricting to news-driven, high-volume stocks
concentrates the strategy on days where informational asymmetry is highest and directional
momentum is most likely to persist. The directional-filter (first-candle color) further
reduces false signals.

## Entry Conditions

- Stock qualifies as "Stocks in Play" for the day (abnormally high relative volume vs. 30-day baseline)
- First 5-minute candle closes
- Price breaks above the 5-min high → **Long** (only if first candle was green)
- Price breaks below the 5-min low → **Short** (only if first candle was red)
- QuantConnect implementation: universe of ~1,000 liquid US equities, top 20 by relative volume selected daily

## Exit Conditions

- Primary exit: **End of day** (market close or 15 min before close to avoid closing auction)
- Stop loss: Entry ± (ATR × 0.10)
- No intraday profit target specified in the original paper

## Risk Management

- ATR-based stop loss (10% of ATR from entry) — tight stop relative to typical intraday range
- End-of-day forced exit eliminates overnight gap risk
- Portfolio-level diversification: paper tests top-20 SiP portfolio simultaneously; QuantConnect
  implementation uses 1,000-stock universe with ~20 concurrent positions
- No leverage specified in paper

## Indicators Used

- **Relative Volume** (vs. 30-day average): universe selection filter
- **ATR (14-period)**: stop loss sizing
- **First 5-min candle direction**: trade direction filter
- **5-min opening range high/low**: breakout entry levels

## Backtest Evidence

**AUDITABLE:** Zarattini, Barbon & Aziz (2024), Swiss Finance Institute Research Paper No. 24-98
(SSRN 4729284). Dataset: 7,000+ US stocks, 2016–2023.

**AUDITABLE (independent replication):** QuantConnect Research Notebook
(https://www.quantconnect.com/research/18444/) — independent implementation with 1,000-stock
universe, 5-min ORB, volume filter. Tested using QC's built-in data, not the authors' data.

## Forward-Test Evidence

NOT REPORTED. No independently audited forward-test results found.

## Reported Metrics

All sourced from Zarattini et al. (2024) paper and QuantConnect replication:

| Metric | Value | Source | Tag |
|--------|-------|---------|-----|
| Total net return (top-20 SiP portfolio, 2016-2023) | >1,600% | Zarattini et al. | CLAIMED, UNVERIFIED |
| Sharpe ratio (top-20 SiP) | 2.81 | Zarattini et al. | CLAIMED, UNVERIFIED |
| Annualized alpha vs benchmark | 36% | Zarattini et al. | CLAIMED, UNVERIFIED |
| Sharpe ratio (QuantConnect replication) | 2.4 | QuantConnect | CLAIMED, UNVERIFIED |
| Beta (QuantConnect replication) | ~0 | QuantConnect | CLAIMED, UNVERIFIED |
| Win rate | NOT REPORTED | — | — |
| Maximum drawdown | NOT REPORTED | — | — |
| Of 25 parameter combos tested (QC), % with Sharpe > benchmark | 68% | QuantConnect | CLAIMED, UNVERIFIED |

## Community Sentiment

**Positive:**
- QuantConnect research community independently replicated and confirmed the general finding.
- Paper generated attention in systematic trading communities (r/algotrading, quant blogs).
- Stoic Research Substack noted an ORB variant "hit nearly 1 Sharpe" in independent testing.

**Skeptical/Critical:**
- No independent criticism specifically targeting this paper found.
- General ORB criticism: strategy is well-known and may be crowded; execution quality
  (slippage, fill at breakout) is critical and hard to simulate accurately.
- ATR stop of 10% is very tight — may result in many stop-outs before the move develops.
- Transaction costs (especially for high-turnover intraday trading) can significantly erode
  returns in practice; paper claims to account for these but specific cost model not verified.
- Short-selling restriction on many brokers for certain "in play" stocks (e.g., stocks with
  recent gaps, hard-to-borrow names) could reduce strategy capacity.

## Strengths / Weaknesses

**Strengths:**
- Academic paper with clear, reproducible rules
- Strong economic rationale (news-driven institutional order flow)
- Multi-year dataset (2016–2023) with survivorship-bias mitigation
- Independent replication confirms directional finding
- Market-neutral (beta ~0) — not just riding the index
- Simple, low parameter count (5-min window, relative volume filter)

**Weaknesses:**
- No forward-test or live-trading evidence found
- Maximum drawdown NOT REPORTED — hard to assess capital adequacy
- Win rate NOT REPORTED — distribution of returns unknown
- Tight ATR stop may produce choppy performance in low-conviction breakouts
- Execution complexity: requires real-time volume monitoring pre-open and sub-minute execution
- Scalability limited: concentrating on top-20 volume names means position sizes may impact market

## Confidence Score

| Dimension | Raw (0–10) | Weight | Weighted |
|-----------|-----------|--------|----------|
| Logical/economic soundness | 8 | 3 | 24 |
| Transparency & reproducibility | 9 | 2 | 18 |
| Evidence quality | 8 | 2 | 16 |
| Risk management quality | 7 | 2 | 14 |
| Honest treatment of drawdowns/failure | 7 | 1.5 | 10.5 |
| Robustness (OOS, walk-forward, multi-market) | 7 | 1.5 | 10.5 |
| Simplicity / low overfitting risk | 8 | 1 | 8 |
| Survived independent scrutiny | 8 | 1 | 8 |
| **Total** | | **14** | **109** |

**Score: 109 / 140 × 100 = 78 → HIGH POTENTIAL**

**Justification:** Academic paper with multi-year dataset, clear rules, auditable methodology,
and independent replication. Score capped below 80 due to missing max drawdown reporting,
absent forward-test, and execution concerns not fully addressed.

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low-medium. Requires pre-open volume monitoring and fast order routing.
- **Scalability:** Limited — top-20 names may have capacity constraints for larger portfolios.
- **Automation:** High feasibility. Rules are mechanical, no discretion required.

## MQL5 / Python / Pine Feasibility

- **Python:** High — QuantConnect already provides an implementation blueprint. Backtrader,
  VectorBT, or Zipline could replicate with intraday US equity data.
- **MQL5:** Limited — US equity intraday data is not natively available in MT5; would require
  a custom data bridge.
- **Pine Script (TradingView):** Possible for visualization/signal generation; not for
  full intraday backtesting with volume filter.

## Similar Strategies

- `london-breakout-forex.md` — same breakout-of-range concept, different session/market
- QuantConnect Boot Camp ORB (without volume filter, weaker edge)

## Red Flags

- None major. Minor: absence of max drawdown reporting and win rate in the paper.

## Source Links

| Source | URL | Retrieved |
|--------|-----|-----------|
| Zarattini, Barbon, Aziz (2024) — SSRN 4729284 | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4729284 | 2026-05-28 |
| Swiss Finance Institute Research Paper No. 24-98 | https://www.sfi.ch/en/publications/n-24-98-a-profitable-day-trading-strategy-for-the-u.s.-equity-market | 2026-05-28 |
| QuantConnect Research Notebook | https://www.quantconnect.com/research/18444/opening-range-breakout-for-stocks-in-play/ | 2026-05-28 |
| Semantic Scholar entry | https://www.semanticscholar.org/paper/A-Profitable-Day-Trading-Strategy-For-The-U.S.-Zarattini-Barbon/dcfcdcfa942360114cc034ccaab18bde1d28a943 | 2026-05-28 |
| Concretum Group (authors' website) | https://concretumgroup.com/a-profitable-day-trading-strategy-for-the-u-s-equity-market/ | 2026-05-28 |

## Analyst Notes

**FACTS:** Paper published Feb 2024, SSRN 4729284, SFI Research Paper 24-98. Dataset: 7,000+ US
stocks, 2016–2023. 5-min ORB on top-20 daily Stocks in Play (by relative volume). Net total
return >1,600%, Sharpe 2.81, alpha 36% annualized. QuantConnect independent replication:
Sharpe 2.4, beta ~0, 68% of 25 parameter combos outperformed benchmark.

**ANALYSIS:** This is one of the better-documented intraday strategy papers I've reviewed.
The volume filter is theoretically motivated (not arbitrary) and the independent QuantConnect
replication is an important quality signal. The claimed Sharpe of 2.81 is unusually high for
a systematic strategy — this warrants scrutiny of the transaction cost model and execution
assumptions. The paper claiming >1,600% net return across 7 years is striking; without max
drawdown data, it is impossible to assess capital requirements or actual risk-adjusted
performance for a real account.

**ASSUMPTIONS (flagged):** Assuming the paper's transaction cost model is realistic for
retail/semi-institutional execution. Assuming the QuantConnect replication used realistic
slippage and market impact (QuantConnect often underestimates execution costs for
high-turnover intraday strategies).

## Future Research Needed

- Obtain max drawdown and win rate from the full paper (SSRN 4729284 — not fetched due to 403)
- Verify the specific transaction cost model used by Zarattini et al.
- Test whether the edge persists in 2024–2026 data (out-of-sample for the paper)
- Examine the related paper: "Beat the Market: An Effective Intraday Momentum Strategy for
  S&P500 ETF (SPY)" by the same authors (SSRN 4824172)
- Build a Python implementation using QuantConnect's blueprint and run on live paper trading
