# The Intramonth Momentum Cycle (PreTOM)

## Overview

Standard cross-sectional WML (Winner-Minus-Loser) momentum held **only during a concentrated
6-trading-day window** before each month-end: days T−9 through T−4 relative to the last trading
day of the month. The strategy holds cash during the remaining ~16 trading days. The mechanism is
not slow information diffusion but **market plumbing**: investors sell their losing stocks to meet
month-end payment obligations ("dash-for-cash"), creating predictable and concentrated selling
pressure on losers in the PreTOM window.

Causal identification: the SEC's May 2024 transition from T+2 to T+1 equity settlement shifted
the settlement-driven selling window by exactly one day — and this shift appears in individual
stock returns, the momentum spread, and mutual fund returns. This natural experiment substantially
strengthens confidence in the mechanism.

## Asset Class / Timeframes

- **Market:** US Equities (CRSP, 1980–2025); also replicated across 19 developed markets
- **Portfolio:** Value-weighted cross-sectional WML long-short
- **Holding period:** 6 trading days per month (T−9 to T−4 relative to last trading day); adjusts
  to T−8 to T−3 under T+1 settlement (post May 2024)
- **Rebalancing:** Monthly

## Core Logic

1. Form a standard cross-sectional momentum WML portfolio each month: long top-decile past winners,
   short bottom-decile past losers (formation period: likely 12-1 month — NOT CONFIRMED in
   accessible sources; assumed standard cross-sectional momentum construction).
2. Enter the WML portfolio at the open of trading day T−9 (9 days before month-end). Under T+1
   settlement (post May 2024), the predicted window shifts to T−8.
3. Hold for 6 trading days (through T−4 close, or T−3 under T+1).
4. Exit entire WML exposure; hold cash (T-bills) for remaining trading days of the month.
5. Repeat monthly.

The strategy's return is entirely driven by the **loser leg**: bottom-decile momentum losers
underperform the market by 9.5 basis points per day (value-weighted) during the PreTOM window.
Winners show no economically meaningful PreTOM differential.

## Economic Rationale

**Why the edge should exist:** Month-end payment obligations (margin calls, redemptions, payroll
trusts, derivatives settlements) create a demand for settled cash. Investors preferentially sell
their losing positions to meet these obligations (disposition effect + realization aversion: they
are less reluctant to crystallize a loss in a stock they already dislike). This selling pressure
artificially depresses loser prices during the PreTOM window, concentrating the momentum return
to the short side.

**When this edge should disappear:**
- Settlement times approach T+0 (same-day), eliminating the need to sell in advance of payment.
- Payment obligation scheduling shifts away from month-end concentration.
- The window becomes publicly known and is systematically front-run, causing the selling to
  migrate earlier — as has already begun post-publication per the T+1 evidence.
- Momentum itself decays if the underlying cross-sectional premium arbitrages away.

The T+1 settlement natural experiment is the highest-quality piece of evidence: a policy change
(not chosen by market participants) caused a predictable, verified 1-day shift in the PreTOM
window. This strongly rules out data mining as the sole explanation.

## Entry Conditions

- **Calendar trigger:** Enter at open of trading day T−9 relative to last trading day of the
  month. (Under T+1 settlement, entered at T−8.)
- **Long leg:** Top-decile US stocks by prior 12-1 month return (formation details NOT CONFIRMED).
- **Short leg:** Bottom-decile US stocks by prior 12-1 month return.
- **Sizing:** Value-weighted within each leg (confirmed in paper).

## Exit Conditions

- **Calendar exit:** Exit entire WML exposure at close of trading day T−4 (T−3 under T+1).
- No stop-loss or early-exit rule specified in accessible sources.

## Risk Management

- **Value-weighting** inherently limits concentration in micro-cap stocks.
- Reduced **time-in-market** (~27% of calendar days) limits directional exposure vs. full-month
  holding.
- No explicit stop-loss, drawdown limit, or regime exit rule specified in accessible sources.
  The strategy does not appear to incorporate a momentum-crash regime filter.

## Indicators Used

- **WML signal:** Past 12-month return minus most-recent-month return (12-1 formation assumed;
  NOT CONFIRMED in full paper).
- **Calendar filter:** Month-end trading-day counter (T−9 to T−4 relative to last trading day).
- **Settlement adjustment:** T+1 rule (post May 2024): window shifts by 1 day.

## Transaction Costs & Capacity

- Transaction cost model: **Novy-Marx and Velikov (2016) effective spread estimates** applied
  (confirmed in secondary sources).
- Strategy is "economically significant post-decimalization" per paper (specific net-cost Sharpe
  NOT REPORTED in accessible sources).
- **Value-weighted** construction mitigates capacity constraints vs. equal-weighted.
- Bottom-decile losers may include small-cap and illiquid stocks with wider bid-ask spreads —
  this is a genuine cost concern not explicitly addressed in accessible summaries.
- Monthly turnover is similar to a standard monthly momentum strategy (one round-trip per position
  per month); the timing compression does not change total turnover, only timing of trades.
- Capacity estimate: NOT REPORTED.

## Backtest Evidence

- SSRN working paper, NOT peer-reviewed. CLAIMED, UNVERIFIED.
- Long sample: 1980–2025 (45 years), CRSP data, value-weighted.
- 19 developed markets international replication: CLAIMED, UNVERIFIED.
- Post-decimalization (post-2001) subsample: strategy survives, CLAIMED, UNVERIFIED.

## Forward-Test Evidence

- T+1 settlement shift (May 2024): predicted 1-day shift observed in individual stock returns,
  momentum spread, and mutual fund returns. CLAIMED, UNVERIFIED (not yet independently replicated).
- This constitutes the strongest available evidence — a near-causal out-of-sample test — but
  covers less than 2 years of post-event data.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED | NOT REPORTED | Full paper inaccessible (HTTP 403) |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | Full paper inaccessible |
| CAGR | NOT REPORTED (see Note) | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | ~6 trading days (PreTOM window) | CLAIMED, UNVERIFIED | Nathan/Suominen/Tasa SSRN 6426026 |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~27% (6/22 trading days per month) | ANALYSIS | Computed from 6 PreTOM days out of ~22 trading days/month |

**Dollar growth (stated):** $1 → $18.78 (PreTOM-only WML, 1980–2025) vs. $2.37 (non-PreTOM
days) vs. $45.06 (full-month WML). CLAIMED, UNVERIFIED. Source: Nathan/Suominen/Tasa SSRN 6426026.

**Note on implied CAGR:** $1 → $18.78 over 45 years implies CAGR ≈ 6.7% (ANALYSIS, not stated
by source). This is the long-short portfolio return only during the 6-day monthly window; the
risk-adjusted Sharpe (per calendar time or per invested day) is NOT REPORTED and cannot be
derived without the full return series.

## Community Sentiment

- **2nd place, Quantpedia Awards 2026** (expert review panel; papers published May 2025–April 2026).
- High-quality co-author: Matti Suominen (Aalto University), a prominent finance professor with
  peer-reviewed publications in top finance journals.
- Not yet submitted/accepted to a peer-reviewed journal (as of June 2026).
- No forum-level community discussion found in accessible sources.
- The natural experiment (T+1 shift) has attracted significant attention in quant finance circles.
- Source: https://quantpedia.com/quantpedia-awards-2026-winners-announcement/

## Why This Might Be Nothing

### 1. Calendar effect data mining (most likely benign explanation)
Sorting momentum portfolio returns by trading day within the month across 45 years and finding a
6-day cluster of above-average returns is a multiple-testing problem. How many day-of-month
combinations were tested before T−9 to T−4 was selected? The paper does not disclose the search
procedure in accessible summaries. Without a Bonferroni-corrected test across all day-of-month
windows, the statistical significance is inflated.

### 2. The T+1 evidence is narrow and recent
The T+1 natural experiment is compelling, but covers only ~24 months of post-event data (May 2024
to June 2026). Small-sample concerns apply. The documented "1-day shift" has not been independently
replicated by other researchers using different data sources. A single-year confirmation of a
directional shift in a noisy signal does not rule out coincidence.

### 3. The cost case
Bottom-decile momentum losers skew toward small-cap, high-bid-ask-spread stocks. Novy-Marx &
Velikov (2016) effective spread estimates are a well-regarded model, but they were calibrated on
a broad cross-section. Entering/exiting the short leg (losers) exactly during a period when other
motivated sellers are also selling concentrates adverse price impact within the 6-day window.
The claim of cost survival is stated but the specific net-cost Sharpe is NOT REPORTED in
accessible sources — it cannot be verified here.

### 4. Regime dependence and momentum crash risk
Standard momentum strategies suffer acute crashes during market recoveries (2009, March 2020):
losers rally violently when market rebounds. If the 6-day PreTOM window happens to overlap with
a market reversal, the short-loser position suffers its worst days. The paper notes that COVID
and 2008 "confirm the broader mechanism" but does not (in accessible summaries) report the maximum
drawdown during those episodes. A strategy with Sharpe concentrated in 6 days/month could
experience outsized monthly drawdowns when the mechanism fails (negative PreTOM momentum return).

### 5. Post-publication decay
The paper was publicly posted March 2026 and won 2nd place at Quantpedia 2026, making the
PreTOM window broadly known. Sophisticated investors may now front-run the entry at T−10 or T−11,
eroding the edge in exactly the way the T+1 settlement shift eroded the prior window. The
strategy's edge is self-defeating if widely implemented.

### What single piece of evidence would most change my mind
Independent replication of the full return series (Sharpe, drawdown) using a different data
source (Bloomberg/Compustat vs. CRSP), with explicitly reported net-of-cost Sharpe post-
decimalization and post-T+1 settlement. Peer-reviewed publication in a top finance journal (JFE,
RFS, JF, JFQA). These are not currently available.

## Strengths / Weaknesses

**Strengths:**
- Clear, specific, falsifiable mechanism (payment obligations → dash-for-cash → loser selling)
- T+1 settlement natural experiment provides rare quasi-causal evidence
- 19-market international replication reduces data-mining concern
- 45-year backtest period
- Economically and statistically significant after costs per authors
- Transaction cost methodology is well-regarded (Novy-Marx & Velikov)

**Weaknesses:**
- Full paper inaccessible (HTTP 403); Sharpe, drawdown, and subsample returns NOT REPORTED
- Not peer-reviewed (as of June 2026)
- Very short post-T+1 evidence window (~2 years)
- Momentum crash risk during PreTOM window not addressed in accessible sources
- Post-publication crowding risk already present
- Bottom-decile loser execution in period of concentrated selling is adversely timed

## Confidence Score

| Dimension | Raw | Weight | Contribution |
|-----------|-----|--------|--------------|
| Logical / economic soundness (falsifiable) | 9 | 3 | 27 |
| Transparency & reproducibility | 5 | 2 | 10 |
| Evidence auditability | 6 | 2 | 12 |
| Risk management quality | 4 | 2 | 8 |
| Cost & capacity realism | 6 | 2 | 12 |
| Honest treatment of drawdowns / failure | 3 | 1.5 | 4.5 |
| Robustness (OOS / walk-forward / multi-market) | 8 | 1.5 | 12 |
| Survived independent scrutiny | 6 | 1 | 6 |
| **Total weighted** | | | **91.5** |

Max possible = 150. Latent score = round(91.5/150 × 100) = **61**.

Evidence auditability = 6 (5–7 range) → verification cap = 74.
Latent 61 < 74, so no cap applies.

**confidence = latent 61 (no cap required; Evidence auditability 6/10: described methodology
with strong causal ID via T+1 natural experiment, but not peer-reviewed; Sharpe and drawdown
NOT REPORTED)**

Band: **Worth Researching** (60–74).

## Complexity / Scalability / Automation feasibility

- **Complexity:** Moderate. Standard cross-sectional momentum (12-1 WML) is well-documented;
  the PreTOM calendar filter adds one additional rule (day-of-month counter).
- **Scalability:** Value-weighted; reasonable capacity at institutional scale for larger stocks.
  Bottom-decile losers (small-cap, illiquid) will cause capacity constraints.
- **Automation feasibility:** HIGH. Calendar trigger is fully automatable; signal is deterministic.
  Requires daily OHLCV + market cap data and a reliable trading-day calendar.

## MQL5 / Python / Pine feasibility

- **Python:** YES. Straightforward using CRSP (via WRDS subscription), Compustat, or open
  proxies (yfinance + CRSP-equivalent). Standard momentum construction + calendar filter.
  `pandas` + `numpy` implementation of 12-1 sort → PreTOM date filter → daily return series.
- **Pine Script (TradingView):** Limited. Pine Script operates per-symbol; cross-sectional
  sorting across hundreds of stocks is not feasible in standard Pine. Not recommended.
- **MQL5:** Not feasible as written. MQL5 lacks built-in cross-sectional sorting. Would require
  custom multi-symbol framework.

## Similar Strategies

- `cmm-daily-return-weighted-momentum` (JBF 2025, score 54): Overlapping domain (momentum
  formation period optimization) but orthogonal approach — weights days in formation period,
  not timing of holding. Could be combined with PreTOM timing.
- `factor-timing-momentum-regime-tai` (SSRN 2026, score 34): Z-score momentum regime timing;
  different mechanism and signals.
- `fractional-momentum-smoothing` (SSRN 2024, score 52): Smoothing the signal, not timing
  the holding window.

## Red Flags

- Full paper inaccessible (HTTP 403) — Sharpe, maximum drawdown, annual return NOT REPORTED.
- Not peer-reviewed.
- Post-publication crowding risk is immediately active (paper is 3 months old as of this analysis).
- The PreTOM calendar window was identified retrospectively — multiple testing risk.

## Source Links

| URL | Retrieved | Used For |
|-----|-----------|----------|
| https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6426026 | 2026-06-17 | Primary paper |
| https://danielrnathan.com/assets/pdf/The_Intramonth_Momentum_Cycle.pdf | 2026-06-17 | PDF attempt (HTTP 403) |
| https://theideafarm.com/portfolio-management/the-intramonth-momentum-cycle/ | 2026-06-17 | Secondary summary (HTTP 403) |
| https://quantpedia.com/quantpedia-awards-2026-winners-announcement/ | 2026-06-17 | Award context |

## Analyst Notes

**FACTS (from sources):**
- $1 → $18.78 over 1980–2025 for PreTOM-only WML; $2.37 for non-PreTOM days; $45.06 for
  full-month WML. (CLAIMED, UNVERIFIED)
- 6 trading days: T−9 to T−4. Window shifts 1 day with T+1 settlement (May 2024). (CLAIMED, UNVERIFIED)
- Bottom-decile losers underperform market by 9.5 bps/day during PreTOM. (CLAIMED, UNVERIFIED)
- Value-weighted WML. CRSP universe. 1980–2025. (CLAIMED, UNVERIFIED)
- Novy-Marx & Velikov (2016) cost model applied; "economically significant post-decimalization." (CLAIMED, UNVERIFIED)
- 19 developed markets replication; same loser-driven pattern. (CLAIMED, UNVERIFIED)
- T+1 shift (May 2024) confirmed in individual stocks, momentum spread, mutual fund returns. (CLAIMED, UNVERIFIED)
- Quantpedia 2026 Awards: 2nd place.
- Authors: Daniel Nathan (PolyU HK), Matti Suominen (Aalto), Joni Tasa.

**ANALYSIS (my reasoning, not from sources):**
- Implied CAGR of PreTOM-only strategy: ($18.78)^(1/45) − 1 ≈ 6.7% (calendar time CAGR for
  a long-short strategy that earns returns only 6 days/month).
- For comparison: full WML CAGR ≈ ($45.06)^(1/45) − 1 ≈ 8.8%; non-PreTOM CAGR ≈ ($2.37)^(1/45) − 1 ≈ 1.9%.
- The PreTOM strategy captures ~76% of the CAGR differential between PreTOM and non-PreTOM,
  in ~27% of calendar days, suggesting a substantially higher Sharpe per unit of calendar-time
  risk than the full WML.
- If Sharpe of full WML ≈ 0.4–0.6 (typical academic value-weighted momentum), and PreTOM
  captures most profits in 27% of days, the PreTOM Sharpe (per calendar day) could be materially
  higher. This is speculative — the actual Sharpe depends on the volatility of the return series
  during PreTOM days.
- The "dollar growth" metric confounds calendar-time performance with invested-time performance.
  A fair comparison requires explicitly reported Sharpe, which is NOT REPORTED.

**ASSUMPTIONS (flagged):**
- WML formation period is 12-1 month (assumed; industry standard; NOT confirmed in accessible sources).
- T+1 window is now T−8 to T−3 (assumed; follows logically from 1-day shift; NOT confirmed).

## Future Research Needed

1. Access full paper PDF (when HTTP 403 is resolved): extract Sharpe ratio, maximum drawdown,
   annual return, sub-period performance, and full cost analysis.
2. Independent replication using Compustat/CRSP data to verify PreTOM concentration.
3. Post-T+1 settlement (June 2024–present) out-of-sample analysis: does the 1-day-shifted
   window continue to concentrate momentum returns?
4. Interaction with momentum crash regimes: does limiting exposure to PreTOM 6 days reduce
   crash severity relative to full-month WML?
5. Monitor for peer-review acceptance (authors have submitted to journals).
6. Assess whether the effect persists post-publication (crowding decay test by 2027).
