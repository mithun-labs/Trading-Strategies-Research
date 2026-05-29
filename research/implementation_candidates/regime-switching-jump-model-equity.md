# Implementation Candidate: Statistical Jump Model Regime-Switching Equity

**Score:** latent 68 (no cap — peer-reviewed + open code)
**Band:** Worth Researching
**Date added:** 2026-05-29
**Performance status:** CLAIMED, UNVERIFIED (specific numbers paywalled; methodology peer-reviewed)

---

## Why This Is an Implementation Candidate

1. **Open-source code exists:** `pip install jumpmodels` — full Python library with
   scikit-learn-style API. Example notebook for Nasdaq-100 available at
   `examples/Nasdaq/example.ipynb` on GitHub.
2. **Peer-reviewed methodology:** Published in Journal of Asset Management (2024), which
   provides stronger evidence for the underlying statistical approach than an unreviewed
   pre-print.
3. **Feasible with public data:** Daily index prices from Yahoo Finance (the same source
   used in the example notebook). No proprietary data required.
4. **In-scope markets:** Nasdaq-100 (NAS100), S&P 500, potentially US30 (extension).

---

## Implementation Approach

```python
# Minimal implementation sketch
import yfinance as yf
from jumpmodels import JumpModel

# Fetch data
data = yf.download("^NDX", start="2000-01-01")  # Nasdaq-100
returns = data["Close"].pct_change().dropna()

# Compute features (return + risk measures — exact formula in paper/code)
# See examples/Nasdaq/example.ipynb for full feature construction
features = compute_features(returns)  # 3-dimensional feature set

# Fit and predict
jm = JumpModel(n_components=2, jump_penalty=lambda_cv)
jm.fit(features)
regime = jm.predict_online(features)  # 0 = bull, 1 = bear

# 0/1 allocation with 1-day delay
allocation = (regime == 0).shift(1)  # 1 = long, 0 = flat
```

---

## Pre-Implementation Checklist

- [ ] Run example notebook on 2023-2026 data (post paper's OOS window) to verify signals
- [ ] Identify exact 3 features from open code (not disclosed in accessible sources)
- [ ] Replicate the time-series CV for λ selection
- [ ] Measure live Sharpe/drawdown vs buy-and-hold on Nasdaq-100 since paper cutoff (2023)
- [ ] Test partial-exposure variant (0%–100% using regime probabilities from `.predict_proba()`)
- [ ] Assess behavior during 2020 COVID V-shape and 2022 bear market (known failure modes)

---

## Key Risks Before Deployment

1. **Paywall gap:** Specific Sharpe/drawdown numbers not verified — full paper needed.
2. **Binary allocation:** The 0/1 simplicity may need to be extended for live use.
3. **V-shape recovery risk:** Strategy likely exits during crashes and misses early recovery.
4. **Regime prediction lag:** 1-day delay is a lower bound; model latency in practice may
   add additional lag.

---

## MQL5 / Python / Pine

- **Python:** HIGH feasibility — `jumpmodels` library, public data, <200 lines for MVP
- **MQL5:** LOW — matrix ops not native; regime could be pre-computed externally
- **Pine Script:** NOT FEASIBLE
