# Master Strategy Index

Last updated: 2026-05-29 (run 2)

## About This Index

This index tracks all strategies researched, their fingerprints, confidence scores, and
last-updated dates. Use it to detect duplicates and track strategy evolution.

## Fingerprint Format

`core mechanism | key indicators | timeframe | market`

---

## Strategy Registry

| Slug | Fingerprint | Score | Band | Last Updated | Status |
|------|-------------|-------|------|--------------|--------|
| `orb-stocks-in-play` | Opening range breakout on high-volume news-driven stocks \| Relative volume + ATR + 5-min candle color \| 5-min intraday \| US Equities | 78 | High Potential | 2026-05-28 | Active |
| `catching-crypto-trends-donchian-ensemble` | Multi-period Donchian channel ensemble with volatility-based sizing \| Donchian channels (9 lookbacks: 5–360d) + realized vol \| Daily \| Crypto (BTC + altcoins) | 64 | Worth Researching | 2026-05-28 | Active |
| `london-breakout-forex` | Asian session range breakout at London open \| Session high/low + optional RSI/MACD/SMA \| 15–30 min \| Forex (EURUSD, GBPUSD, GBPJPY) | 47 | Experimental | 2026-05-28 | Weak evidence |
| `adaptivetrend-crypto` | Multi-component trend-following: momentum entry + volatility-calibrated trailing stop + rolling Sharpe asset selection + 70/30 long/short allocation \| Momentum (H6) + rolling Sharpe + intra-day vol estimate \| 6-hour \| Crypto (Binance Futures perpetual, 150+ pairs) | 41 | Experimental | 2026-05-28 | Preprint only; no independent replication; key rules not retrieved |
| `intraday-momentum-spy` | Noise-boundary intraday momentum with VWAP trailing stop + vol targeting \| 14-day rolling avg abs-return (noise boundary) + intraday VWAP \| Intraday (entry at HH:00/HH:30) \| US Equities (SPY ETF) | 50 | Experimental | 2026-05-29 | Leverage-inflated Sharpe; max drawdown not reported; no independent replication |
| `interpretable-hypothesis-driven-microstructure` | Walk-forward RL framework testing 5 microstructure pattern families (accumulation, flow momentum, mean reversion, breakout, range-value) \| Daily OHLCV + RL execution \| Daily \| US Equities (100-stock universe) | 52 | Experimental | 2026-05-29 | Methodology paper; NO EDGE demonstrated (p=0.34, Sharpe 0.33); value is the validation framework, not the signals |

---

## Scam / Reject Archive

| Slug | Reason | Date |
|------|--------|------|
| *(none yet)* | | |

---

## Related Papers Not Yet Fully Researched

| Title | Authors | Identifier | Notes |
|-------|---------|------------|-------|
| ~~Beat the Market: Intraday Momentum for SPY~~ | Zarattini, Aziz, Barbon | SSRN 4824172 | **RESEARCHED 2026-05-29** → see `intraday-momentum-spy.md` |
| ~~AdaptiveTrend: Crypto Trend Following~~ | Duc Bui, Thanh Nguyen | arxiv 2602.11708 | **RESEARCHED 2026-05-28** → see `adaptivetrend-crypto.md` |
| ~~Interpretable Hypothesis-Driven Trading~~ | Deep, Deep, Lamptey | arxiv 2512.12924 | **RESEARCHED 2026-05-29** → see `interpretable-hypothesis-driven-microstructure.md` |
| Forecast-to-Fill Gold Futures | Singha, Aguilera-Toste, Lahiri | arxiv 2511.08571 | Trend+momentum XAUUSD, Sharpe 2.88 CLAIMED (metrics suspicious) |
| Small-Cap Stock Trading Strategies | Poudel | SSRN 5921742 | 6 strategy families, OOS Sharpe 0.7-0.9, Dec 2025 |

---

## Notes

- Initialized: 2026-05-28 (first run)
- Repository structure created: 2026-05-28
