# Implementation Candidate: ORB — Stocks In Play

**Score:** 78 (High Potential)
**Added:** 2026-05-28
**Strategy file:** `research/strategies/orb-stocks-in-play.md`

## Summary

5-minute Opening Range Breakout on news-driven high-volume US equities. Academic backing
(Zarattini et al. 2024, SSRN 4729284). Independent QuantConnect replication confirms Sharpe ~2.4.

## Implementation Priority

HIGH — Python/QuantConnect blueprint already exists.

## Key Pre-Implementation Research Needed

1. Obtain max drawdown and win rate from full paper (SSRN 4729284)
2. Verify transaction cost model assumptions
3. Confirm edge persistence in 2024–2026 data (out-of-sample for the paper)

## Suggested Stack

- **Data:** Alpaca Markets (free US equity intraday + pre-market volume) or QuantConnect
- **Backtest:** QuantConnect (blueprint available), VectorBT, or Backtrader
- **Execution:** Alpaca API (commission-free), Interactive Brokers

## Risk Parameters to Establish

- Max daily loss limit
- Position sizing (paper: top-20 Stocks in Play; start with 1–5 positions for initial testing)
- Stop: ATR × 0.10 from entry

## References

- QuantConnect blueprint: https://www.quantconnect.com/research/18444/
- SSRN paper: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4729284
