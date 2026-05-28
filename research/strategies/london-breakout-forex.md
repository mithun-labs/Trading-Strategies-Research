# London Breakout Strategy — Forex (LB-FX)

## Overview

A session-based breakout strategy for forex pairs. The Asian session (roughly 0300–0800 London
time) defines a consolidation range; the strategy enters in the direction of any breakout when
the London session opens (0800+). Widely discussed in the retail forex community, with multiple
independent backtests that collectively show **weak and inconsistent edge**, particularly on
EURUSD.

**Assessment: INSUFFICIENT EVIDENCE for a repeatable edge. EXPERIMENTAL at best.**

## Asset Class / Timeframes

- **Asset class:** Forex (EURUSD primary, GBPUSD, GBPJPY also tested)
- **Timeframe:** 15-minute and 30-minute charts
- **Holding period:** Intraday (sessions close or explicit time exit)

## Core Logic

1. Define the Asian session range: high and low from approximately 0300–0800 London time
   (exact hours vary by source; some use 0000–0700 or 0300–0700).
2. At London open (0800 LT+), wait for a candle to close **outside** the Asian session range.
3. Enter in the direction of the breakout.
4. Stop loss: just outside the opposite side of the Asian range.
5. Profit target: 1.5–3× the stop distance (approximately 20–30 pips typical target).
6. Optional filters: RSI/MACD confirmation, 50-period SMA trend direction, news filter
   (skip major economic events), time-based exit (close at day end if target not reached).

## Economic Rationale

**ANALYSIS:** The theoretical rationale is that the London session is the world's highest-volume
forex session. When price breaks the low-liquidity Asian range, institutional order flow
entering at London open can sustain a directional move. This is a plausible but weak
structural argument: the Asian range is simply a price consolidation, not a meaningful
order-flow barrier. Breakouts can and frequently do fail (false breakouts) because the range
itself is arbitrary — its boundaries are not tied to institutional resting orders in a
verifiable way. Unlike the ORB strategy (which is anchored in fundamental news flow), this
strategy has no verified informational driver.

**PATTERN-ONLY:** No cited academic rationale found in sources retrieved.

## Entry Conditions

- Time: 0800–1100 London time window only
- Entry signal: Candle body closes outside the Asian session high or low
- Long: Close above Asian session high
- Short: Close below Asian session low
- Optional filters (as described in GitHub backtest): RSI, MACD, 50-SMA trend

## Exit Conditions

- Target: 1.5–3× stop distance (20–30 pips typical)
- Stop: Opposite side of Asian range
- Time exit: End of London session or end of day (all positions closed)

## Risk Management

- Stop loss defined by range boundary (opposite side)
- Profit/loss ratio: 1:1.5 to 1:3
- No position sizing rules specified beyond basic risk management
- News filter recommended (skip before/after major economic releases)

## Indicators Used

- **Asian session high/low** (time-based range: 0300–0800 LT)
- **RSI** (optional filter): NOT REPORTED which parameters
- **MACD** (optional filter): NOT REPORTED which parameters
- **50-period SMA** (optional trend filter)

## Backtest Evidence

**PRIMARY (GitHub, inspectable code):** MHZardary/london-strategy-backtest
(https://github.com/MHZardary/london-strategy-backtest)
- Markets: EURUSD, GBPUSD, GBPJPY
- Timeframes: 15-min, 30-min
- Period: July 12, 2024 – July 11, 2025 (1 year)
- Total trades: 242
- Win rate: ~54.1%
- Average PnL per trade: +$0.00018 (essentially break-even)
- Standard deviation of trade PnL: $0.00429
- Author conclusion: "Weak results when applying this strategy"

**SECONDARY (QuantifiedStrategies.com):** Strategy testing on EURUSD shows the simple version
"loses money" or "shows no persistent edge" in standard backtests.

**SECONDARY (Babypips.com community test):** London Statistical Breakout System — mixed results
reported, with the strategy requiring many filters to show any edge.

**SECONDARY (various quant blogs):** Consistent finding that the strategy requires careful
parameter selection and often shows only marginal statistical significance.

## Forward-Test Evidence

NOT REPORTED. No independently audited forward-test results found.

## Reported Metrics

| Metric | Value | Source | Tag |
|--------|-------|---------|-----|
| Win rate (EURUSD/GBPUSD/GBPJPY, 2024–2025) | 54.1% | MHZardary GitHub | CLAIMED, UNVERIFIED |
| Average PnL per trade | +$0.00018 | MHZardary GitHub | CLAIMED, UNVERIFIED |
| Trade count (1 year) | 242 | MHZardary GitHub | CLAIMED, UNVERIFIED |
| CAGR | NOT REPORTED | — | — |
| Sharpe ratio | NOT REPORTED | — | — |
| Maximum drawdown | NOT REPORTED | — | — |

## Community Sentiment

**Mixed/Negative:**
- Multiple independent backtests (QuantifiedStrategies, GitHub, Babypips) report weak edge.
- ForexFactory thread on "Range Breakout System" shows mixed practitioner results, with most
  profitable versions requiring significant parameter tuning (suggesting overfitting risk).
- General consensus in quant communities: simple London breakout without filters is not
  profitable; filtered versions show marginal significance and may not survive out-of-sample.

**Occasional Positive:**
- GBP/JPY tends to show stronger directional continuation during London session (higher
  volatility, trend persistence) — some practitioners report better results on this pair.
- Strategy remains popular in retail forex communities despite weak systematic evidence.

## Strengths / Weaknesses

**Strengths:**
- Very simple, fully mechanical rules
- Logical session-based timing (London session is highest volume)
- Inspectable code available (GitHub)
- Reasonable risk management (defined stop and target)

**Weaknesses:**
- Multiple independent backtests show weak/no persistent edge on EURUSD
- Essentially break-even average trade PnL over 1-year period tested
- No academic support or economic rationale beyond loose session-flow argument
- No explicit out-of-sample test by any source reviewed
- Parameter sensitivity: results highly dependent on exact session definition, filter choices
- High false-breakout rate in range-bound or overlapping-session periods
- Transaction costs erode already-thin edge

## Confidence Score

| Dimension | Raw (0–10) | Weight | Weighted |
|-----------|-----------|--------|----------|
| Logical/economic soundness | 4 | 3 | 12 |
| Transparency & reproducibility | 8 | 2 | 16 |
| Evidence quality | 3 | 2 | 6 |
| Risk management quality | 5 | 2 | 10 |
| Honest treatment of drawdowns/failure | 5 | 1.5 | 7.5 |
| Robustness (OOS, walk-forward, multi-market) | 2 | 1.5 | 3 |
| Simplicity / low overfitting risk | 8 | 1 | 8 |
| Survived independent scrutiny | 3 | 1 | 3 |
| **Total** | | **14** | **65.5** |

**Score: 65.5 / 140 × 100 = 47 → EXPERIMENTAL**

**Justification:** Simple rules and inspectable code earn transparency points, but the evidence
for a reliable edge is weak. Multiple backtests show break-even to slightly negative performance.
The economic rationale is speculative. Scored in "Experimental" band — may have edge in
specific pairs (GBPJPY) or with specific filters, but the base version does not appear to
have a repeatable, robust edge.

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Very low. Simple range definition and breakout detection.
- **Scalability:** High — forex has sufficient liquidity for most position sizes.
- **Automation:** High feasibility. Fully mechanical, no discretion required.

## MQL5 / Python / Pine Feasibility

- **MQL5:** High — MT4/MT5 are the dominant platforms for this strategy; many free EAs exist.
- **Python:** High — easy to implement with OANDA or FXCM data via pandas/backtrader.
- **Pine Script:** High for visualization; limited for precise session-time range logic across timezones.

## Similar Strategies

- `orb-stocks-in-play.md` — same breakout-of-range concept; stronger evidence base
- "Big Ben Breakout Strategy" (popularized by Steve Hopwood) — similar concept with specific rules

## Red Flags

- No robust edge demonstrated across multiple independent backtests.
- Simple version (EURUSD, raw breakout) appears unprofitable.
- High risk of overfitting when adding filters to produce positive backtest results.

## Source Links

| Source | URL | Retrieved |
|--------|-----|-----------|
| MHZardary GitHub backtest | https://github.com/MHZardary/london-strategy-backtest | 2026-05-28 |
| QuantifiedStrategies.com | https://www.quantifiedstrategies.com/london-breakout-strategy/ | 2026-05-28 |
| QuantifiedStrategies Substack | https://quantifiedstrategies.substack.com/p/london-breakout-strategy-rules-and | 2026-05-28 |
| Babypips London Statistical Breakout | https://www.babypips.com/trading/trading-system-test-london-statistical-breakout-system | 2026-05-28 |
| ForexFactory Range Breakout System | https://www.forexfactory.com/thread/1299658-range-breakout-system | 2026-05-28 |
| Medium backtest review (Tzimopoulos) | https://medium.com/@phitzi/backtest-results-revealed-is-the-london-breakout-strategy-worth-it-0e9df65dd63b | 2026-05-28 |

## Analyst Notes

**FACTS:** GitHub backtest (MHZardary, July 2024–July 2025): EURUSD/GBPUSD/GBPJPY, 15–30min,
242 trades, 54.1% win rate, avg PnL ~$0 per trade. Multiple secondary sources confirm weak
results on EURUSD in standard tests. Strategy uses Asian session range (0300–0800 LT) as
consolidation zone, enters on London breakout (0800+). TP 1.5–3x, SL at range opposite.

**ANALYSIS:** A 54.1% win rate sounds positive, but with PnL essentially at zero, the wins
and losses are roughly equal in magnitude. This means the theoretical 1.5–3:1 reward-to-risk
is not being achieved in practice, which is a major red flag. The false breakout rate is high
enough that the directional edge assumed by the strategy doesn't materialize consistently.
The weak results are consistent across multiple independent sources, suggesting the result is
not data-specific but a genuine reflection of limited edge.

**ASSUMPTIONS (flagged):** The GitHub backtest period (1 year, 2024–2025) may not be
representative — the strategy could perform differently in trending vs. ranging environments.
GBP/JPY performance was noted as stronger; targeted testing on that pair with proper
walk-forward might reveal a different picture.

## Future Research Needed

- Isolate GBPJPY results from the GitHub backtest (pair-specific analysis)
- Test whether adding specific fundamental catalysts (e.g., BOE policy days) improves edge
- Compare to ICT "killzone" approach (similar time targeting but different entry logic)
- Seek academic literature on forex session-open breakouts specifically
