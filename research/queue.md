# Candidate Strategy Queue

Last updated: 2026-06-03 (run 18)

## Pending

- `commodity-intramarket-correlation-momentum` — Commodity momentum regime filter via 20-day vs 250-day correlation (Vojtko/Pauchlyová SSRN 4964417, Sep 2024); 4 ETFs: DBA/DBB/DBE/DBP

## Researching

*(none currently)*

## Completed

- `orb-stocks-in-play` — score 78 (High Potential) — 2026-05-28
- `catching-crypto-trends-donchian-ensemble` — score 64 (Worth Researching) — 2026-05-28
- `london-breakout-forex` — score 47 (Experimental) — 2026-05-28
- `adaptivetrend-crypto` — score 41 (Experimental) — 2026-05-28
- `intraday-momentum-spy` — score 50 (Experimental) — 2026-05-29
- `interpretable-hypothesis-driven-microstructure` — score 52 (Experimental) — 2026-05-29 — METHODOLOGY PAPER: no edge demonstrated (p=0.34)
- `forecast-to-fill-gold-futures` — score 37 (Low Confidence) — 2026-05-29 — SUSPICIOUS METRICS: Sharpe 2.88 and max DD 0.52% implausible
- `small-cap-multipattern-poudel` — score 53 (Experimental) — 2026-05-29 — 6 strategy families; OOS Sharpe 0.7–0.9 credible but requires independent replication
- `regime-switching-jump-model-equity` — score 68 (Worth Researching) — 2026-05-29 — Peer-reviewed (Journal of Asset Management 2024); open code (jumpmodels PyPI); implementation candidate
- `network-momentum-trend-following` — score 52 (Experimental) — 2026-05-29 — Pre-print; lead-lag network signal; 28 futures OOS 2005-2024; net Sharpe > MACD; no peer review; no code
- `omega-model-intraday-nasdaq` — score 55 (Experimental) — 2026-05-29 — Peer-reviewed PLOS One; NASDAQ-100 outperforms all 3 periods; S&P 500 only COVID; authors: not suitable long-term
- `fx-mean-reversion-futures-monthly` — score 46 (Experimental) — 2026-05-29 — Linear Sharpe 0.12 = null result; exponential better but unverified; SSRN working paper (Quantpedia)
- `quantevolve-multi-agent-strategy` — score 24 (Low Confidence) — 2026-05-29 — FRAMEWORK PAPER: data mining by design; 6+2 asset evaluation; workshop paper; no specific deployable strategy
- `attention-factors-statistical-arbitrage` — score 66 (Worth Researching) — 2026-05-29 — Full ICAIF 2025 paper; Pelger (Stanford) group; OOS Sharpe 2.28 net / >4 gross over 24 yrs on 500 large-caps; OOS ends Dec 2021; momentum-dependent (net Sharpe→0.59 w/o past returns); implementation candidate pending code release
- `options-wheel-llm-bayesian` — score 24 (Low Confidence) — 2026-05-29 — arXiv preprint; LLM-Bayesian hybrid wheel strategy; 0% assignment rate implausible; probable lookahead bias; max DD -8.2% through 2008/2020 understated; no open code; no peer review
- `causal-inference-directional-trading` — score 15 (Low Confidence) — 2026-05-29 — REJECTED: 2-month backtest (Jun–Aug 2023) on 3 stocks; 100% win rate = overfitting on tiny sample; no OOS; no costs
- `sector-rotation-tsx60` — score 54 (Experimental) — 2026-05-29 — Peer-reviewed MDPI JRFM (Jan 2026); open code (GitHub); Sharpe 0.922 vs 0.624 B&H; median+quarterly rotation; OOS 2020–2025; TSX 60 Canadian equities; multiple testing over 72 variants; max DD paywalled
- `graph-clustering-pairs-trading` — score 49 (Experimental) — 2026-05-29 — arXiv:2406.10695 + SSRN:4849095; graph clustering + ML classifier (top 10% filter) + Kelly criterion; 17-year OOS (2006–2022) survivorship-corrected; IR > SPY at 1% significance; specific Sharpe/max DD NOT REPORTED
- `vix-cmf-ml-walkforward` — score 45 (Experimental) — 2026-05-29 — PLOS One April 2024 (peer-reviewed); 7 ML models on VIX CMF term structure; μt + Δroll features; C-MVO IR 0.623; 11-year walk-forward; Sharpe/max DD NOT REPORTED; Volmageddon tail risk unaddressed; daily costs unknown
- `hmm-rl-regime-taa` — score 44 (Experimental) — 2026-05-29 — arXiv:2605.27848 preprint (May 2026) + related practitioner open code (2-state HMM); SPY/TLT/GLD ETF rotation; Sharpe 1.22 / CAGR 19.41% / Max DD −19.54% CLAIMED but full-sample HMM = in-sample optimism; 2022 bond-equity correlation break unaddressed; costs not modeled; no OOS
- `bitcoin-max-min-trendrev` — score 45 (Experimental) — 2026-05-29 — SSRN 4955617 (Beluška/Vojtko, Sep 2024); combined MAX+MIN 10-day lookback; return 98.43% / vol 47.75% / MDD −37.67% / Ret/Vol 2.06 (full sample); OOS (2022–2024) −12% MDD; MIN component weak (80%+ DD); no costs; not peer-reviewed; latent 45 = tied with vix-cmf-ml-walkforward
- `ict-silver-bullet-forex` — score 34 (Low Confidence) — 2026-05-31 — YouTube / ICT (Michael Huddleston); killzone-timed (3 windows/day) liquidity sweep + MSS + FVG entry; 70–80% WR CLAIMED but creator blew audited public accounts (2016, 2024); MSS identification subjective; fraud allegations; session-timing element independently supported; latent 34
- `bitcoin-multitf-trend` — score 33 (Low Confidence) — 2026-05-30 — SSRN 5748642 (Mesíček/Vojtko, Nov 2025); D1H1 STOP Sharpe 1.07 / Calmar 0.87; costs NOT modeled (presumptive invalidation at ~143 trades/yr × Gemini fees); long-only in structural BTC bull market; no OOS; SSRN working paper only
- `crypto-ts-xs-momentum-realistic` — score 55 (Experimental) — 2026-05-30 — SSRN 4675565 (Han/Kang/Ryu, Dec 2023, Sungkyunkwan); TS momentum Sharpe 1.51 (market 0.84) at 28-day/5-day CLAIMED; 15bps costs modeled; XS momentum fails after costs; ~471 coins; no OOS; SSRN working paper only
- `gold-cross-asset-regimes` — score 51 (Experimental) — 2026-05-30 — Quantpedia blog (Dujava, Dec 2025); long GLD when BOTH GLD and IEF 12M return positive; CAGR 8.5% / Sharpe 0.71 / Max DD -33.7% (1992-2026 Allocate Smartly); independently replicated; underperforms B&H gold absolute return; no peer review; no OOS documented
- `mining-gold-regimes` — score 46 (Experimental) — 2026-05-30 — Journal of Alternative Investments Vol 28 No 1 Summer 2025 (peer-reviewed, Bhansali/Suvak et al.); PCA + k-means (k=3) regime identification → gold TAA; 3 regimes (stable currency / inflation hedge / low-rate); TAA outperforms benchmarks CLAIMED; all metrics paywalled; no open code; regime 3 first Dec 2008
- `fx-carry-value-momentum-200yr` — score 51 (Experimental) — 2026-05-31 — SSRN working paper (Joseph Chen, revised 2024); carry Sharpe 0.391 (short rates) / 0.361 (long bonds) over 200+ yr CLAIMED; momentum only works latter half of sample; reversal only post-Bretton Woods; yield-curve flattening most robust; no cost modeling; not peer-reviewed
- `currency-momentum-factor-menkhoff` — score 59 (Experimental) — 2026-05-31 — Peer-reviewed JFE Vol 106 (2012), Menkhoff/Sarno/Schmeling/Schrimpf; BIS WP 366; in-sample Sharpe 0.95 (MOM(1,1), 1976–2010); OOS Sharpe −0.32 post-publication (Hutchinson et al. IRFA 2022); G10 momentum stopped post-GFC; publication effect documented; latent 59 = highest in Experimental band; basis-momentum (Fan 2025) researched as evolution candidate — see currency-basis-momentum-fan
- `currency-basis-momentum-fan` — score 39 (Low Confidence) — 2026-05-31 — Peer-reviewed EFM 2025 (Fan/Han/Li/Liu; SSRN 4925173); BM-3 Sharpe 0.52 CLAIMED; paradoxically lower than standalone carry (0.63) and momentum (0.79); dominant components are known decayed factors; no OOS test; 48 currencies (G10+EM); bid-ask costs modeled; sub-period 3 (2011–2020) unconfirmed; all sources HTTP 403; latent 39
- `fx-news-sentiment-xgboost` — score 30 (Low Confidence) — 2026-06-01 — arXiv 2505.16136 (Zhang, May 2025); GDELT+FinBERT+XGBoost; Sharpe 5.87/4.65/4.65 CLAIMED; CAGR >50% FX CLAIMED; automatic scrutiny triggers fired (Sharpe >3, CAGR >50%); FinBERT temporal leakage + CV hyperparameter overfitting most likely explanation; no peer review; no independent replication; code on GitHub but reproduction results pending; latent 30
- `vix-etn-dual-approach` — score 48 (Experimental) — 2026-06-01 — SSRN 5316487 (Zarattini/Aziz/Mele; Swiss Finance Institute WP 25-91; June 2025); CAGR 16.3% / Sharpe 1.0 / equity correlation ~15% CLAIMED; realistic costs stated; 2008–2025 (17 yrs); max drawdown NOT REPORTED; Volmageddon 2018 handling critical unknown; SVXY structural break post-2018; not peer-reviewed; latent 48
- `industry-trend-century` — score 64 (Worth Researching) — 2026-06-01 — SSRN 4857230 (Zarattini/Antonacci; CMT Award 2025; June 2024); CAGR 18.2% / Sharpe 1.39 / max DD 33%; 98-year backtest (48 Fama-French industries); ETF OOS (20 yr) by authors; walk-forward "persistent challenges" (arXiv 2412.14361) — magnitude NOT QUANTIFIED; SSRN working paper; implementation candidate (Ken French + SPDR ETFs); latent 64
- `drift-regime-factor-singha` — score 21 (Low Confidence) — 2026-06-01 — arXiv 2511.12490 (Singha, Nov 2025); Sharpe 13 / CAGR 158.6% / max DD -11.9% CLAIMED; both scrutiny triggers fired (Sharpe >3, CAGR >50%); no transaction costs; survivorship bias; single non-finance author; randomization test inadequate; no peer review; latent 21
- `gold-bitcoin-dual-momentum` — score 49 (Experimental) — 2026-06-02 — Quantpedia blog (Dujava, Apr 2026); X-week dual momentum GLD+IBIT; vol-capped composite: 12.01% CAGR / Sharpe 1.37 / MDD -12.27% CLAIMED; uncapped 8-wk: 79.91% CAGR / Sharpe 1.64 (CAGR >50% triggers scrutiny); 10 lookbacks tested without OOS; IBIT proxy pre-2024; regime luck (BTC 2020-2021 bull); latent 49
- `tsmom-china-commodity-futures` — score 60 (Worth Researching) — 2026-06-02 — Journal of Futures Markets 2025 (Zheng/Zhang/Lien/Yu, peer-reviewed); 64 Chinese commodity futures 2003–2023; robust after costs (JFoM); basic Sharpe 0.07 / enhanced Sharpe 0.75 (Liu et al. 2023, CLAIMED); non-positive risk-return in China; China accessibility constraint; latent 60 = implementation candidate (TSMOM formula inspectable from Moskowitz 2012)
- `trendfollios-fixed-income` — score 40 (Experimental) — 2026-06-02 — arXiv 2506.09330 (Lu/Rojas/Yeung/Convery, June 2025); not peer-reviewed; Sharpe 0.33 (inception) / 0.04 (3Y) / 0.65 (1Y) CLAIMED; 5.89% excess/yr vs Bloomberg US Agg CLAIMED; 17-19% vol inconsistent with pure fixed income; proprietary "line-based/curve-based" signals; 3Y Sharpe 0.04 in 2022 rate shock; commercial-pitch-on-arXiv; latent 40
- `ml-btc-hourly-cost-aware` — score 46 (Experimental) — 2026-06-03 — arXiv 2606.00060 (Bysik/Ślepaczuk, Jun 2026); 27-fold walk-forward; 10bps costs explicitly modeled; naive sign strategies fail after costs; CAGR >65%/Sharpe >1 (long-only XGBoost, CLAIMED); CAGR >50% triggers scrutiny (BTC bull market); max drawdown NOT REPORTED; no open code; latent 46
- `em-us-momentum-taa` — score 49 (Experimental) — 2026-06-03 — Quantpedia blog (Vojtko/Dujava, Apr 2026); SPY/EEM monthly SMA/ROC; long-short SMA composite CAR 7.13%/Sharpe 0.564/MDD -20.31% CLAIMED; US dominance regime dominates sample (2010-2026); pre-2003 data synthetic; ETF borrowing costs not modeled; no OOS; no peer review; latent 49

## Rejected

- `mean-reversion-currencies-quantpedia` — DUPLICATE of `fx-mean-reversion-futures-monthly` — same paper (SSRN 5002058, Beluška/Vojtko, Oct 2024); already fully researched 2026-05-29 with score 46
