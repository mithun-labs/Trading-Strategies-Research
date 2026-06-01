# Implementation Candidate: Industry Trend Following — Century of Profitable Industry Trends

**Score:** 64 (Worth Researching)
**Added:** 2026-06-01
**Strategy file:** `research/strategies/industry-trend-century.md`

## Summary

Long-only trend following on 48 Fama-French US industry portfolios (or 31 SPDR sector ETFs),
using Donchian Channel (20-day high / 40-day low) combined with Keltner Channel (20-day EMA ±
1.4 × ATR) breakout signals. Exits to T-bills when all signals are absent. 98-year backtest:
Sharpe 1.39, CAGR 18.2%, max DD 33%. CMT Award 2025 winner. Ken French data is publicly free.
IMPORTANT: independent walk-forward study (arXiv 2412.14361) found "persistent challenges" —
quantify the OOS degradation before deploying capital.

## Implementation Priority

MEDIUM — Python/sector-ETF implementation is straightforward, but walk-forward degradation
must be quantified first. The rule verification itself IS the key remaining research step.

## Key Pre-Implementation Research Needed

1. **Replicate and walk-forward test:** Download Ken French 48-industry data; implement 20/40
   Donchian + Keltner; run rolling 5-year walk-forward windows to quantify OOS Sharpe degradation.
   This is the critical gate: if walk-forward Sharpe is < 0.5, the edge is largely illusory.
2. **ZIRP-era isolation:** Measure performance in 2009–2021 (near-zero T-bill rates) to separate
   cash yield contribution from trend signal contribution.
3. **ETF-only backtest:** Replicate on 31 SPDR sector ETFs with realistic costs (2004–2026)
   independently of the original authors.

## Suggested Stack

- **Data:** Ken French Data Library (free, CSV downloads) for historical; Yahoo Finance / yfinance
  for SPDR sector ETF prices (XLK, XLF, XLE, XLV, XLI, XLY, XLP, XLU, XLB, XLC, XLRE + smaller)
- **Signal:** pandas-ta or manual 20-day rolling max (Donchian) + EWM + ATR (Keltner)
- **Position sizing:** ATR-based equal-risk weighting
- **Backtest:** VectorBT (vectorized for 48 instruments) or Backtrader
- **Execution:** Alpaca (commission-free ETFs), Interactive Brokers, or TD Ameritrade for sector ETFs

## Risk Parameters to Establish

- Maximum portfolio drawdown limit (suggest: -25% from peak → reduce all positions 50%)
- Maximum sector concentration (suggest: cap at 30% in any single sector ETF)
- Walk-forward minimum Sharpe threshold: if rolling 3-year walk-forward Sharpe < 0.4, pause trading
- Rebalancing frequency: daily signal check; trade only on signal changes (not daily rebalancing)
