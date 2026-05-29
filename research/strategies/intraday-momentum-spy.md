# Intraday Momentum — SPY ETF (IM-SPY)

**Slug:** `intraday-momentum-spy`
**Last Updated:** 2026-05-29
**Status:** Active — Experimental

---

## Overview

A systematic intraday trend-following strategy applied to SPY (S&P 500 ETF), one of the world's most liquid instruments. The strategy builds on the academic literature establishing intraday momentum (Gao, Han, Li, Zhou 2013) but departs from it by not restricting entries to the final 30 minutes of the session. Instead, it enters as soon as intraday price action crosses a dynamically computed "noise boundary," interpreted as an indication of an abnormal demand/supply imbalance. Exits use a VWAP-anchored trailing stop and a forced close at session end.

Published as Swiss Finance Institute Research Paper No. 24-97 (SSRN 4824172, May 2024) by Carlo Zarattini, Andrew Aziz, and Andrea Barbon — the same group that published the ORB-SIP paper (SSRN 4729284, researched 2026-05-28).

---

## Asset Class / Timeframes

- **Asset:** SPY (S&P 500 ETF)
- **Market hours:** Regular US equity session
- **Signal check frequency:** At each HH:00 and HH:30 (half-hour marks) intraday
- **Trade duration:** Intraday only — all positions close at session end

---

## Core Logic

**Noise Boundaries (computed each minute):**
- Upper bound = Daily opening price × (1 + avg(|intraday return from open|) over last 14 trading days)
- Lower bound = Daily opening price × (1 − avg(|intraday return from open|) over last 14 trading days)
- Adjustment: if prior night had a gap-up, lower the lower bound further; if gap-down, raise the upper bound further (overnight gap correction)
- This creates a time-varying corridor that widens across the session as average moves grow

**Entry (checked at HH:00 and HH:30 only):**
- If SPY price > upper boundary → open LONG position in SPY
- If SPY price < lower boundary → open SHORT position in SPY
- Only one position at a time; held until trailing stop or close

**Exit — VWAP Trailing Stop:**
- Long: trailing stop = max(upper boundary, intraday VWAP)
  - Exit long if price falls below this level
- Short: trailing stop = min(lower boundary, intraday VWAP)
  - Exit short if price rises above this level
- Hard close: all open positions terminated at market close regardless

**Position Sizing:**
- Volatility-targeted: sized to achieve 2% daily volatility, based on realized SPY volatility over the past 14 trading days
- Allows up to 4× leverage when SPY realized volatility is below 2%

---

## Economic Rationale

Intraday momentum has a documented academic basis (Gao et al. 2013, SSRN 2440866): the first 30 minutes of SPY daily return predicts the last 30 minutes — a result attributed to institutional order flow and late-day rebalancing. This paper extends the concept: when SPY moves outside the range of typical intraday noise early in the session, it signals a genuine directional imbalance — momentum is more likely to persist than reverse.

The VWAP trailing stop has practical logic: institutional participants anchor execution to VWAP; a long trade falling back through VWAP indicates demand has exhausted, making the signal unsustainable. The noise boundary exit similarly provides a rules-based reversion guard.

---

## Entry Conditions

1. Check at HH:00 or HH:30 only
2. Compute upper and lower noise boundaries for that minute
3. Bullish signal: close > upper boundary → enter LONG
4. Bearish signal: close < lower boundary → enter SHORT
5. No re-entry until prior position is closed

---

## Exit Conditions

1. VWAP trailing stop hit (see Core Logic above)
2. Market close (hard stop — every trade is intraday)
3. If holding long and price < max(upper_boundary, VWAP) → exit
4. If holding short and price > min(lower_boundary, VWAP) → exit

---

## Risk Management

- **Volatility targeting:** 2% daily vol target with 14-day realized vol
- **Maximum leverage:** 4× (applied when realized vol < 2%)
- **Hard stop:** All positions closed at end of session — no overnight risk
- **Gap risk:** Eliminated by intraday-only structure
- **Practical concern:** 4× leverage may trigger margin calls during spikes; broker-level margin requirements not addressed in the paper

---

## Indicators Used

- Intraday VWAP (anchor: session open)
- 14-day rolling average of |intraday returns from open| (noise boundary)
- Overnight gap (prior close to current open) — for boundary adjustment

---

## Backtest Evidence

- **Instrument:** SPY (S&P 500 ETF)
- **Period:** January 2007 – early 2024 (≈17 years)
- **Source:** SSRN 4824172 (Zarattini, Aziz, Barbon 2024)
- **Status:** CLAIMED, UNVERIFIED — Not peer-reviewed; no independent replication found

Reported metrics:
- Total return (net of costs): **1,985%** (CLAIMED, UNVERIFIED)
- Annualized return: **19.6%** (CLAIMED, UNVERIFIED)
- Sharpe Ratio: **1.33** (CLAIMED, UNVERIFIED)
- SPY buy-and-hold Sharpe over same period: **0.45** (CLAIMED, UNVERIFIED)
- Beta: **slightly below zero** (CLAIMED, UNVERIFIED) — market-neutral character
- Sharpe in high-VIX regime (VIX > 40): **3.50** (CLAIMED, UNVERIFIED)
- Max drawdown: **NOT REPORTED** in sources retrieved
- Win rate: **NOT REPORTED** in sources retrieved

**Community-derived adjustment (not in paper):**
- When de-levered to 1× (no vol targeting, no leverage amplification), Sharpe drops to approximately **0.399** — barely above the raw SPY buy-and-hold Sharpe (CLAIMED, community analysis, UNVERIFIED)

---

## Forward-Test Evidence

**NOT REPORTED.** No documented out-of-sample or live trading results post-publication were found.

---

## Reported Metrics

| Metric | Value | Tag |
|--------|-------|-----|
| Total return (2007–2024) | 1,985% | CLAIMED, UNVERIFIED |
| Annualized return | 19.6% | CLAIMED, UNVERIFIED |
| Sharpe Ratio (strategy) | 1.33 | CLAIMED, UNVERIFIED |
| Sharpe Ratio (SPY B&H benchmark) | 0.45 | CLAIMED, UNVERIFIED |
| Sharpe (VIX > 40 regime) | 3.50 | CLAIMED, UNVERIFIED |
| Beta | ~0 (slightly negative) | CLAIMED, UNVERIFIED |
| Max drawdown | NOT REPORTED | — |
| Win rate | NOT REPORTED | — |
| Sharpe at 1× leverage | ~0.399 | CLAIMED, community analysis, UNVERIFIED |
| Total transaction costs (est.) | $27,318 (16.7% of returns) | CLAIMED, community analysis, UNVERIFIED |

---

## Community Sentiment

**Criticism (from independent reviews and community analysis):**

1. **Leverage inflates Sharpe:** Up to 4× leverage is used. At 1× leverage, the community estimates Sharpe drops to ~0.399 — nearly indistinguishable from passive SPY exposure. The paper's headline 1.33 Sharpe is leverage-amplified.
2. **Drawdown underreported:** The paper calculates drawdowns using end-of-day portfolio values, not intraday. A strategy with 4× leverage on a volatile ETF will experience intraday drawdowns substantially larger than the reported figures.
3. **"Too good to be true":** Reviewers describe the results as "a bit too good to be true." The risk-adjusted performance, once adjusted for leverage, is not compelling.
4. **Parameter choices:** 14-day lookback, half-hour checkpoints, 2% vol target, 4× leverage cap — several free parameters not subjected to rigorous walk-forward analysis.
5. **Not peer-reviewed:** Published as SFI Research Paper, not in a traditional peer-reviewed journal. Authors are affiliated with Bear Bull Traders (Andrew Aziz — retail trading educator), which does not invalidate the work but is worth noting in terms of publication incentive.
6. **Overfitting concern amplified by Maróy (2025):** A follow-up paper (SSRN 5095349) applies parameter optimization to the Zarattini framework and achieves Sharpe >3 on QQQ. If parameter optimization on a single instrument nearly triples the Sharpe, the original parameters may themselves be optimized to historical SPY data.

**Neutral/positive notes:**
- The strategy has genuine economic motivation rooted in the academic intraday momentum literature.
- The 17-year backtest period spans multiple market regimes (2007–2008 GFC, 2010–2019 bull market, 2020 COVID crash, 2022 drawdown).
- Market-neutral character (near-zero beta) is valuable for portfolio construction.
- Regime analysis (better in high-VIX environments) is a constructive contribution that supports the "demand imbalance" hypothesis.

---

## Strengths / Weaknesses

**Strengths:**
- Rules are fully specified and reproducible
- Long backtest covers multiple regimes
- Zero overnight risk (intraday-only)
- Near-zero beta — potential diversifier for equity long portfolios
- Economic rationale is grounded in documented intraday momentum literature
- Works best in volatile regimes (VIX > 40) — provides convexity during stress events

**Weaknesses:**
- Leverage-inflated Sharpe; 1× Sharpe ~0.399 is unimpressive
- Max drawdown and win rate not reported — critical risk metrics absent
- Intraday drawdown underreported by EOD calculation method
- No out-of-sample test post-publication
- Not peer-reviewed
- 4× leverage amplifies tail risk in real implementation
- Transaction costs estimated at ~16.7% of gross returns — execution-sensitive
- Drawdown in stress regime (the same regime where it performs best: high VIX) could be extreme with leverage

---

## Confidence Score

| Dimension | Weight | Score (0–10) | Reasoning |
|---|---|---|---|
| Logical/economic soundness | 3 | 7 | Intraday momentum is documented (Gao et al. 2013); noise boundary and VWAP exit have economic logic |
| Transparency & reproducibility | 2 | 7 | Entry/exit/sizing rules fully stated; boundary formula derivable from paper description |
| Evidence quality (auditable > claimed > none) | 2 | 4 | 17-year backtest is good length; but not peer-reviewed, no independent replication, all results from paper alone |
| Risk management quality | 2 | 5 | Vol targeting and VWAP stop are sound; 4× leverage and no intraday drawdown reporting are significant concerns |
| Honest treatment of drawdowns/failures | 1.5 | 3 | Max drawdown not reported; EOD drawdown calculation significantly underestimates true leverage-adjusted intraday drawdown |
| Robustness (OOS, walk-forward, multi-market) | 1.5 | 4 | No walk-forward test; no multi-instrument test by original authors (Maróy extends to QQQ but is a separate paper) |
| Simplicity / low overfitting risk | 1 | 4 | Multiple free parameters; half-hour checkpoint rule feels empirically optimized |
| Survived independent scrutiny | 1 | 3 | Community identifies leverage inflation and EOD drawdown flaw; no replication with positive outcome found |

**Weighted sum:** (7×3) + (7×2) + (4×2) + (5×2) + (3×1.5) + (4×1.5) + (4×1) + (3×1)  
= 21 + 14 + 8 + 10 + 4.5 + 6 + 4 + 3 = **70.5**

**Score = (70.5 / 140) × 100 = 50.4 → 50**

**Band: Experimental (40–59)**

The core economic mechanism is sound, and the long backtest is a genuine asset. However, leverage inflation, missing max-drawdown data, no post-publication out-of-sample results, and community concerns about the Sharpe calculation methodology collectively keep this in the Experimental band.

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Medium. Rules are well-specified. Main challenge is computing VWAP and noise boundaries in real time.
- **Scalability:** High — SPY is extremely liquid; strategy size would not move the market.
- **Automation:** Straightforward for Python/QuantConnect. Requires minute-level or higher-frequency SPY data, intraday VWAP, and real-time boundary recomputation.

---

## MQL5 / Python / Pine Feasibility

- **Python:** HIGH FEASIBILITY — VWAP and rolling volatility are standard. QuantConnect or Zipline with Alpaca data. Key step: real-time VWAP reset at session open, 14-day rolling abs-return average, check signal at HH:00/HH:30 only.
- **MQL5:** LOW PRIORITY — MT5 has limited SPY data; SPXUSD CFD available but liquidity and spread conditions differ from the SPY ETF.
- **Pine Script:** FEASIBLE for signal visualization and approximate backtesting on TradingView; VWAP is built-in to Pine. Not suitable for precise backtesting at required granularity.

---

## Similar Strategies

- [`orb-stocks-in-play`](orb-stocks-in-play.md) — Same research group (Zarattini, Aziz, Barbon); also an ORB strategy but applied to high-volume news-driven individual equities rather than the index ETF. Scored 78.
- Original Gao et al. (2013) "Market Intraday Momentum" (SSRN 2440866) — academic predecessor; restricts to last-30-minutes entry, no trailing stop.
- Maróy (2025) "Improvements to Intraday Momentum Strategies" (SSRN 5095349) — follow-up with parameter optimization; applies framework to QQQ; Sharpe >3 (NOT RESEARCHED — in queue).

---

## Red Flags

- **Leverage inflation:** 4× leverage; headline Sharpe 1.33 drops to ~0.399 at 1× — barely above passive SPY.
- **EOD-only drawdown:** Intraday drawdowns not measured; methodology systematically underreports risk with 4× leverage.
- **Max drawdown not reported:** Cannot assess risk without maximum drawdown.
- **No independent replication with confirmed results.**
- **Not peer-reviewed:** SFI series is working-paper quality, not journal-reviewed.
- **Associated author (Andrew Aziz):** Retail trading educator/promoter; publication has commercial context (Bear Bull Traders). Not fabrication, but warrants acknowledgment.

---

## Source Links

| URL | Retrieved | Notes |
|-----|-----------|-------|
| https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4824172 | 2026-05-29 | Primary paper (SSRN 4824172) |
| https://www.sfi.ch/en/publications/n-24-97-beat-the-market-an-effective-intraday-momentum-strategy-for-s-p500-etf-spy | 2026-05-29 | SFI publication page |
| https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5095349 | 2026-05-29 | Maróy follow-up (SSRN 5095349) |
| https://www.quantconnect.com/forum/discussion/17091/beat-the-market-an-effective-intraday-momentum-strategy-for-s-amp-p500-etf-spy/ | 2026-05-29 | QuantConnect community discussion |
| https://www.cxoadvisory.com/momentum-investing/complex-intraday-time-series-momentum-strategy-applied-to-spy/ | 2026-05-29 | CXO Advisory review (HTTP 403, not retrieved) |
| https://newsletter.huntgathertrade.com/p/intraday-momentum-researched-based | 2026-05-29 | Hunt Gather Trade deep dive (HTTP 403, not retrieved) |
| https://quantmacro.substack.com/p/paper-review-an-effective-intraday | 2026-05-29 | QuantMacro paper review (HTTP 403, not retrieved) |
| https://quantifiedstrategies.com/intraday-momentum-trading-strategy/ | 2026-05-29 | QuantifiedStrategies review (HTTP 403, not retrieved) |

---

## Analyst Notes

**FACTS (from sources):**
- Paper published May 2024, SSRN 4824172, by Zarattini, Aziz, Barbon at SFI/University of St. Gallen and Concretum Group.
- Backtest period: January 2007 – early 2024 (≈17 years).
- Reported net-of-costs return: 19.6% annual, Sharpe 1.33.
- Uses up to 4× leverage via volatility targeting.
- Drawdowns reported on end-of-day portfolio values only.
- One instrument only: SPY.
- Entry at HH:00 or HH:30 only; exit via VWAP trailing stop or session close.
- Noise boundary = daily open × (1 ± rolling 14-day mean absolute intraday return).
- Community analysis estimates 1× leverage Sharpe ≈ 0.399.

**ANALYSIS (my reasoning):**
The headline Sharpe of 1.33 is substantially explained by the leverage structure. At 1× leverage, the strategy barely outperforms passive SPY on a risk-adjusted basis. The economic mechanism (intraday momentum driven by demand imbalance) is real and well-documented in the broader literature, but the specific parameter choices (14-day lookback, half-hourly checks) feel calibrated to SPY history. The missing max-drawdown figure is a significant gap — with 4× leverage, the true intraday drawdown during the 2020 COVID event or 2022 bear market could be severe. Until independent replication and forward-test results appear, this is a "worth watching" strategy, not a deployment candidate.

**ASSUMPTIONS (flagged):**
- I assume "1× leverage Sharpe ≈ 0.399" is a community-computed figure and not stated in the paper. Source was a web search summary, not a direct document read. Treat as directionally informative but not precisely verified.
- I assume the 14-day lookback is the primary parameter; the paper may contain additional parameters not captured in accessible summaries.

---

## Future Research Needed

1. Full paper PDF access (SSRN 4824172) — need max drawdown, win rate, full trade statistics.
2. Independent forward-test results post-May 2024 publication.
3. Sensitivity analysis: does the strategy hold up if 14-day lookback is changed to 10, 20, or 30 days?
4. Intraday-level drawdown analysis with 4× leverage during stress events (2020 March, 2022 Oct).
5. Comparison to Maróy (2025) extension on QQQ — once researched, note cross-links.
6. Replication on QuantConnect with proper transaction cost assumptions.
