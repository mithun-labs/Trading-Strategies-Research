# Master Strategy Index

Last updated: 2026-05-30 (run 13)

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
| `omega-model-intraday-nasdaq` | Omega ratio portfolio optimization on daily-ranked top/bottom 15 S&P 500 or NASDAQ-100 stocks \| Intraday return ranking (15–60 min momentum / 60–300 min reversal) + Omega ratio weights \| Intraday \| US Equities (S&P 500, NASDAQ-100 components) | 55 | Experimental | 2026-05-29 | Peer-reviewed (PLOS One 2023); NASDAQ-100 outperforms all 3 periods; S&P 500 only during COVID volatility; authors say not suitable long-term |
| `fx-mean-reversion-futures-monthly` | Monthly long/short FX futures sorted by deviation from historical mean \| Historical mean deviation + linear/exponential sizing \| Monthly \| Forex FX futures (G10) | 46 | Experimental | 2026-05-29 | Linear variant Sharpe 0.12 = null result; exponential better but unverified; not peer-reviewed; SSRN working paper |
| `quantevolve-multi-agent-strategy` | LLM multi-agent evolutionary framework generating diverse strategies via quality-diversity optimization \| QD feature map (Sharpe, MDD, Sortino, turnover, category) + LLM hypothesis generation \| NOT REPORTED \| 6 equities + 2 futures (NOT REPORTED) | 24 | Low Confidence | 2026-05-29 | FRAMEWORK PAPER: data mining by design; 6+2 asset evaluation; workshop paper only; no open code; no specific deployable strategy |
| `attention-factors-statistical-arbitrage` | Cross-attention characteristics-driven factor structure + LongConv residual signal; jointly optimized for net-of-cost performance \| 39 firm characteristics + 30 attention factors + 32 LongConv convolutions (30-day residual lookback) \| Daily \| US Equities (500 largest) | 66 | Worth Researching | 2026-05-29 | Full ICAIF 2025 conference paper; Pelger (Stanford) group; OOS Sharpe 2.28 net / >4 gross over 24 years; OOS ends Dec 2021 (2022–2025 gap); no open code yet |
| `options-wheel-llm-bayesian` | Options wheel (cash-secured put → covered call) decisions guided by LLM-constructed Bayesian network with historical analogue selection \| Implied vol + market conditions + news (exact features NOT REPORTED) \| Options-expiry driven (weekly/monthly assumed) \| US Equities Options (underlying NOT REPORTED) | 24 | Low Confidence | 2026-05-29 | arXiv preprint only; probable lookahead bias; 0% assignment rate implausible through 2008/2020; -8.2% max DD through 2008 understated; no open code; no peer review |
| `causal-inference-directional-trading` | Multi-stage causal inference pipeline (GMM + GCT + PCMCI + ETE) on clustered equity pairs + DTW/KNN trade timing \| GMM volatility clusters + Granger causality + PCMCI + Effective Transfer Entropy + DTW + KNN \| Intraday \| US Equities (QCOM, TSLA, AMZN — 3 stocks) | 15 | Low Confidence | 2026-05-29 | 2-month backtest only (Jun–Aug 2023); 100% win rate = curve-fitting on tiny sample; no OOS; no costs; no peer review; REJECT |
| `sector-rotation-tsx60` | Systematic sector rotation via median-performer selection on S&P/TSX 60 \| Recent sector return ranking \| Quarterly rebalancing \| Canadian Large-Cap Equities (S&P/TSX 60, 11 GICS sectors) | 54 | Experimental | 2026-05-29 | Peer-reviewed MDPI JRFM (Jan 2026); open code (GitHub); Sharpe 0.922 vs 0.624 B&H; OOS 2020–2025; max DD paywalled; multiple testing over 72 variants; TSX 60 market-specific |
| `graph-clustering-pairs-trading` | Multi-pair stat arb via return-correlation graph clustering + ensemble ML classifier top-10% filter + Kelly criterion sizing \| Return correlation graph + ensemble ML + Kelly criterion \| Daily (10-day rebalancing) \| US Equities (S&P 500 historical components) | 49 | Experimental | 2026-05-29 | arXiv preprint + SSRN + WNE Working Paper; no peer review; 17-year OOS (2006–2022) survivorship-corrected; IR > SPY at 1% significance; specific metrics (Sharpe, max DD) NOT REPORTED |
| `vix-cmf-ml-walkforward` | Daily ML prediction of VIX CMF next-day returns via term structure features (μt + Δroll) + C-MVO portfolio \| μt (VIX CMF mean level) + Δroll (roll yield change) + 7 ML models \| Daily \| VIX Constant Maturity Futures (CBOE) | 45 | Experimental | 2026-05-29 | PLOS One 2024 (peer-reviewed, open access); 11-year walk-forward; C-MVO IR 0.623; Sharpe/max DD NOT REPORTED; Volmageddon tail risk unaddressed in accessible summaries |
| `hmm-rl-regime-taa` | 2-or-3-state HMM (VIX change) + RL policy → SPY/TLT/GLD daily rotation \| Gaussian HMM on Δ VIX + RL policy (paper) \| Daily \| Multi-Asset ETF (SPY + TLT + GLD) | 44 | Experimental | 2026-05-29 | arXiv preprint (May 2026); open practitioner code (2-state); Sharpe 1.22 / CAGR 19.41% / max DD −19.54% CLAIMED but full-sample HMM = in-sample optimism; 2022 bond-equity correlation break unaddressed; costs not modeled |
| `bitcoin-max-min-trendrev` | Rolling N-day MAX price triggers long (trend continuation) + rolling N-day MIN price triggers long (mean reversion); combined or standalone \| N-day rolling MAX/MIN price \| Daily \| Bitcoin (BTC/USD) | 45 | Experimental | 2026-05-29 | Quantpedia working paper (SSRN 4955617); not peer-reviewed; combined 10-day strategy: return 98.43%, vol 47.75%, MDD −37.67%, Ret/Vol 2.06; OOS (2022–2024) −12% MDD; MIN component structurally weak (80%+ drawdowns); no costs modeled |
| `bitcoin-multitf-trend` | H1 MACD crossover entry filtered by D1 MACD trend direction + trailing stop exit (Elder D1H1 multi-TF approach) \| MACD (H1 + D1) + trailing stop \| H1 entry / D1 filter \| Bitcoin (BTC/USD) | 33 | Low Confidence | 2026-05-30 | Quantpedia working paper (SSRN 5748642, Nov 2025); not peer-reviewed; D1H1 STOP Sharpe 1.07 / Calmar 0.87 CLAIMED; costs not modeled (presumptive invalidation at ~143 trades/yr); long-only bull-market window |
| `crypto-ts-xs-momentum-realistic` | Long market when J-day momentum positive (TS, long-only) vs rank-and-hold long-short XS momentum; 15bps transaction costs modeled \| J-day rolling return (28-day best) \| Daily (5-day hold) \| Crypto (~471 Binance coins, market cap + volume filtered) | 55 | Experimental | 2026-05-30 | SSRN 4675565 (Han/Kang/Ryu, Dec 2023, Sungkyunkwan); not peer-reviewed; TS Sharpe 1.51 (market 0.84) CLAIMED; XS fails after costs; 15bps modeled; no OOS; no open code |

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
