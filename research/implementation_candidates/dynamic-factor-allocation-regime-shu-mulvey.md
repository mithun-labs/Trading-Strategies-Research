# Implementation Candidate: Dynamic Factor Allocation via SJM Regime Switching

**Score:** latent 61 (capped to 74; not binding at 61)
**Band:** Worth Researching
**Date added:** 2026-06-04
**Performance status:** CLAIMED, UNVERIFIED — IR improvement 0.05→0.4 accessible; absolute Sharpe/CAGR/drawdown paywalled in JPM

---

## Why This Is an Implementation Candidate

1. **Related open-source code:** `pip install jumpmodels` — the SJM engine (the core algorithmic
   component) is openly available on PyPI/GitHub. The factor allocation (BL integration) requires
   standard portfolio construction code.
2. **Peer-reviewed methodology:** Published in Journal of Portfolio Management Vol. 51 Issue 3, 2025
   (JPM is a top practitioner-academic journal for factor investing).
3. **Factor data is publicly available:** MSCI, FTSE Russell, or factor ETFs (VLUE, MTUM, QUAL,
   USMV, SIZE, IWF/IUSG) provide proxies for the 6 style factors.
4. **In-scope markets:** US equities — applies to NAS100 components, S&P 500 constituents.
5. **Extension of verified prior work:** Builds on `regime-switching-jump-model-equity` (already
   in our database), extending the SJM from market timing to factor rotation.

---

## Implementation Approach

```python
# Conceptual implementation sketch (requires paper access for exact feature set)
import pandas as pd
import numpy as np
from jumpmodels import SparseJumpModel  # or JumpModel with jump_penalty
import yfinance as yf

# 1. Fetch factor ETF data as proxies
factor_tickers = {
    "value":      "VLUE",   # iShares MSCI USA Value Factor
    "momentum":   "MTUM",   # iShares MSCI USA Momentum Factor
    "quality":    "QUAL",   # iShares MSCI USA Quality Factor
    "low_vol":    "USMV",   # iShares MSCI USA Min Vol Factor
    "size":       "IJR",    # iShares Core S&P Small-Cap ETF (size proxy)
    "growth":     "IWF",    # iShares Russell 1000 Growth ETF
}
market_ticker = "SPY"

data = yf.download(list(factor_tickers.values()) + [market_ticker], start="1998-01-01")
prices = data["Adj Close"]
returns = prices.pct_change().dropna()

# 2. Compute active returns per factor (excess over market)
market_ret = returns[market_ticker]
factor_active_rets = {f: returns[t] - market_ret for f, t in factor_tickers.items()}

# 3. Fit SJM per factor to identify bull/bear regimes
regime_signals = {}
for factor, active_ret in factor_active_rets.items():
    # Feature engineering: risk & return measures from active returns + market features
    # (exact features NOT OPEN — need JPM paper; approximate with rolling Sharpe + vol)
    features = compute_features(active_ret, market_ret)  # placeholder
    sjm = SparseJumpModel(n_components=2, jump_penalty=lambda_cv)
    sjm.fit(features)
    regime_signals[factor] = sjm.predict_online(features)  # 0=bull, 1=bear

# 4. Black-Litterman portfolio construction
# Use regime signals as BL views (positive expected active return if bull, 0 if bear)
# Combine BL posterior with market-cap prior → portfolio weights
weights = black_litterman_allocation(regime_signals, factor_covariance)
```

---

## Pre-Implementation Checklist

- [ ] Obtain full JPM paper for exact feature set used in SJM per factor
- [ ] Identify which factor indices are used (MSCI, Russell, CRSP, or specific ETFs)
- [ ] Confirm Black-Litterman prior construction (market-cap implied returns, τ parameter)
- [ ] Run `jumpmodels` on US factor ETF data since 2004 (post-ETF launch date)
- [ ] Verify single-factor L/S results match paper's "positive Sharpe across all 6 factors"
- [ ] Measure IR of dynamic allocation vs. equal-weight across recent regimes (2020, 2022)
- [ ] Assess factor-specific regime signals during 2022 (value/growth rotation year)
- [ ] Benchmark against factor-momentum (e.g., FAANG-heavy momentum in 2020–2021)

---

## Key Risks Before Deployment

1. **Paywalled specification:** Feature set and BL parameters require paper access.
2. **Factor ETF proxies:** iShares ETFs may not match the paper's factor indices exactly,
   leading to regime misidentification.
3. **Factor crash risk:** Momentum factor crashes (~2009, ~2022) could be identified as bull
   regimes at onset, leading to large concentrated drawdowns.
4. **Benchmark comparison:** IR of 0.4 vs. EW benchmark; unknown vs. factor-momentum benchmark.
5. **Implementation lag:** SJM online prediction has 1-day delay; monthly rebalancing mitigates.

---

## MQL5 / Python / Pine

- **Python:** HIGH feasibility — `jumpmodels` for SJM; `PyPortfolioOpt` or custom BL; yfinance
  or similar for factor ETF data; ~300–500 lines for full MVP
- **MQL5:** LOW — requires external regime signals; equity factor ETFs not natively available
- **Pine Script:** NOT FEASIBLE for the SJM computation; could consume pre-computed signals
