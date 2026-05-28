# Master Strategy Index

Last updated: 2026-05-28 (pass 2)

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

---

## Scam / Reject Archive

| Slug | Reason | Date |
|------|--------|------|
| *(none yet)* | | |

---

## Related Papers Not Yet Fully Researched

| Title | Authors | Identifier | Notes |
|-------|---------|------------|-------|
| Beat the Market: Intraday Momentum for SPY | Zarattini, Aziz, Barbon | SSRN 4824172 | Extension of ORB-SIP to SPY ETF |
| ~~AdaptiveTrend: Crypto Trend Following~~ | Duc Bui, Thanh Nguyen | arxiv 2602.11708 | **RESEARCHED 2026-05-28** → see `adaptivetrend-crypto.md` |
| Interpretable Hypothesis-Driven Trading | Deep, Deep, Lamptey | arxiv 2512.12924 | Walk-forward framework; Sharpe 0.33 (methodology paper) |

---

## Notes

- Initialized: 2026-05-28 (first run)
- Repository structure created: 2026-05-28
