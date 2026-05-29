# Master Strategy Index

Last updated: 2026-05-29 (run 3)

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
| `forecast-to-fill-gold-futures` | Trend + momentum regime signal → vol-targeted fractional Kelly position sizing + ATR exits \| Trend indicator + momentum indicator (formulas NOT REPORTED) + ATR \| Daily \| Gold Futures (GC/XAUUSD) | 37 | Low Confidence | 2026-05-29 | SUSPICIOUS METRICS: Sharpe 2.88 and max DD 0.52% implausible; no peer review; no live trading; no independent replication |
| `small-cap-multipattern-poudel` | Six strategy families: volatility-momentum, gap-and-go ORB, mean reversion, breakout-retest, event-driven vol, regime-filtered composite \| OHLCV + volume + catalyst events + regime classifier \| Intraday \| US Small-Cap Equities ($300M–$2B) | 53 | Experimental | 2026-05-29 | Credible claims (OOS Sharpe 0.7–0.9, CAGR ~14%, max DD <17%); good bias control; retail author, no independent replication |
| `regime-switching-jump-model-equity` | Binary 0/1 equity index timing via Statistical Jump Model regime identification \| Daily return + risk features (3 total) + jump penalty λ \| Daily \| Major Equity Indices (US, Germany, Japan; NAS100 example) | 68 | Worth Researching | 2026-05-29 | Peer-reviewed (Journal of Asset Management 2024); open code (GitHub/PyPI); OOS 1990–2023; specific metrics paywalled |
| `network-momentum-trend-following` | Cross-market lead-lag network signal (Lévy area + DTW) augmenting MACD trend on 28 futures \| MACD + Lévy area + DTW graph-adjacency network signal \| Daily \| 28 Futures (agriculture, energy, metals, equity indices) | 52 | Experimental | 2026-05-29 | Pre-print; no peer review; net Sharpe > MACD baseline confirmed; overfitting risk from 28×28 parameter matrix |

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
| ~~Forecast-to-Fill Gold Futures~~ | Singha, Aguilera-Toste, Lahiri | arxiv 2511.08571 | **RESEARCHED 2026-05-29** → see `forecast-to-fill-gold-futures.md` |
| ~~Small-Cap Stock Trading Strategies~~ | Poudel | SSRN 5921742 | **RESEARCHED 2026-05-29** → see `small-cap-multipattern-poudel.md` |

---

## Notes

- Initialized: 2026-05-28 (first run)
- Repository structure created: 2026-05-28
