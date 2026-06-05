# Implementation Candidate: commodity-futures-curve-dynamics-ns

**Strategy:** NS slope/level/curvature change momentum on commodity futures  
**Score:** 62 (Worth Researching) | **Latent:** 62  
**Paper:** Bianchi/Fan/Miffre/Zhang, Journal of Banking & Finance, Vol. 154, Sep 2023  
**arXiv:** 2308.00383 | **SSRN:** 3749061

## Why It Qualifies

- latent_score = 62 ≥ 60 threshold
- Nelson-Siegel model is standard econometrics — well-documented, no proprietary
  components; implementable from Diebold-Li (2006) methodology
- Long-short portfolio construction is straightforward (rank by NS parameter change,
  equal capital to longs and shorts)
- Monthly rebalancing variant is operationally feasible

## Performance Status

- Slope strategy Sharpe 1.41 pre-cost → 0.44–1.04 net (monthly) — CLAIMED, UNVERIFIED
- Specific CAGR and max drawdown: NOT REPORTED / paywalled
- Performance is still unverified pending access to full paper

## Implementation Approach

**Language:** Python (preferred)

**Data requirements:**
- Full commodity futures term structure data (multiple maturities per commodity)
- ~21 contracts across energy, metals, agriculture (exact universe paywalled)
- Providers: Bloomberg, Refinitiv, Norgate Data, Quandl (futures)
- For a simplified test: use publicly available CME/NYMEX settlement data

**Algorithm:**
1. For each commodity at each period end, collect prices at maturities τ₁, τ₂, ..., τₙ
2. Fit Diebold-Li NS model:
   - Fix λ (or grid-search); regress prices/yields on the NS loading factors
   - Extract L_t, S_t, C_t time series per commodity
3. Compute ΔS_t = S_t − S_{t-1} (or ΔL_t, ΔC_t for other variants)
4. Rank all commodities by ΔS_t; long top quartile, short bottom quartile
5. Equal capital allocation; hold for 1 month (monthly variant)
6. Roll positions to next-nearby at standard roll dates

**Key libraries:**
- `scipy.optimize` or `statsmodels`: NS non-linear regression
- `pandas`, `numpy`: data handling, ranking
- `matplotlib`: performance visualization
- `quantstats` or custom: performance metrics

## Critical Implementation Notes

- The NS lambda (decay parameter) calibration choice significantly affects slope
  interpretation; use the Diebold-Li fixed λ = 0.0609 as starting point
- Roll cost needs to be modeled at the contract-level, not just portfolio level
- For the daily variant: post-cost performance not confirmed; start with monthly
- The commodity universe is NOT confirmed — identify ~21 liquid futures from
  CME, ICE, CBOT data

## Research Gates Before Implementation

1. Access full JBF paper (via institutional library or HAL open access) to confirm:
   - Exact commodity universe (names + exchanges)
   - Precise evaluation period
   - Post-cost Sharpe for daily variant
   - Max drawdown
2. Consider testing the simplified version: NS slope proxy = (1st nearby − 12th nearby)
   / maturity_spread (a linearly approximated slope) before implementing full NS fitting
