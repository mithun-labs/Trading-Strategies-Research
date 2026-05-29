# Implementation Candidate: Attention Factors for Statistical Arbitrage

**Score:** latent 66 (capped to 66 pending verification — conference paper; no open code yet for this paper)
**Band:** Worth Researching
**Date added:** 2026-05-29
**Performance status:** CLAIMED, UNVERIFIED (Gross Sharpe >4, Net Sharpe 2.28; specific results paywalled; no open code yet)

---

## Why This Is an Implementation Candidate

1. **Inspectable, well-specified architecture:** cross-attention factor model over 39 firm
   characteristics → 30 attention factors → LongConv (32 convolutions, 30-day residual lookback)
   → factor-neutral arbitrage portfolio. The pipeline is described in enough detail to reconstruct.
2. **Predecessor has open code:** "Deep Learning Statistical Arbitrage" (Guijarro-Ordonez, Pelger,
   Zanotti; Management Science 2022) has public code/data on Pelger's Stanford site
   (mpelger.people.stanford.edu/data-and-code). Same architecture family — a strong starting scaffold.
3. **Full peer review:** ICAIF 2025 full paper; methodology is reviewed (auditability 7).
4. **In-scope market:** US large-cap equities — liquid, well-documented.

**Caveat:** This is the *most infrastructure-heavy* candidate in the database. Unlike `jumpmodels`
(pip-installable) it requires building/porting a deep-learning pipeline AND licensed characteristic
data. Listed as a candidate on logic-inspectability per the framework rule, NOT because it is quick
to stand up.

---

## Implementation Approach (high-level — NOT a verified spec)

```text
1. Data layer (the hard part):
   - Daily returns for the ~500 largest, most-liquid US equities
   - 39 firm characteristics in 6 categories (size, value, momentum, profitability,
     investment, liquidity) — source: WRDS / Compustat / CRSP (licensed)
     Likely characteristic set: Green-Hand-Zhang (2017) or Hou-Xue-Zhang (2020) style.

2. Factor layer (Attention Factors):
   - Cross-attention groups stocks into K=30 latent factors from time-varying characteristics
   - Extract idiosyncratic residual returns (factor-neutral)

3. Signal layer (LongConv):
   - 32 long-convolution kernels over 30-day residual return windows
   - Output → per-stock long/short signal

4. Portfolio layer:
   - Jointly optimize factor structure + trading policy for NET (after-cost) Sharpe
   - Transaction costs AND shorting costs in the objective (this is the paper's key advance)

5. Validation:
   - 8-year rolling training window, retrain annually, evaluate strictly OOS
```

---

## Pre-Implementation Checklist

- [ ] Check Pelger Stanford site for a code release specific to *this* paper (raises evidence to ≥8 if found)
- [ ] If only DLSA code is available, fork it and add the cross-attention factor module
- [ ] Secure WRDS/Compustat/CRSP access (or approximate with a reduced public-data characteristic set)
- [ ] **Run the held-out 2022–2025 period** — the paper's OOS ends Dec 2021; this gap is the #1 test
- [ ] Reproduce the ablation: confirm net Sharpe collapses (~0.59) without past-return info — establishes how much is reversal/momentum premium vs. true arbitrage
- [ ] Model realistic market impact at target AUM (capacity is NOT REPORTED in the paper)
- [ ] Measure max drawdown (NOT REPORTED) — required before any sizing decision

---

## Key Risks Before Deployment

1. **Regime gap:** OOS ends Dec 2021. 2022 momentum crash + rate regime untested. Highest-priority risk.
2. **Premium, not arbitrage:** Ablation suggests heavy dependence on cross-sectional reversal/momentum —
   a possibly-crowded, possibly-decaying risk premium rather than a durable pricing-error edge.
3. **Cost sensitivity:** Gross >4 → Net 2.28 means ~43% of gross eaten by costs even with cost-aware
   training; real execution + market impact at scale could erode further.
4. **Data dependency:** Requires licensed fundamental characteristic data; results may not replicate on
   a free-data approximation.
5. **Complexity / overfitting:** 39 chars + 30 factors + 32 kernels + tuning = high parameter surface.

---

## MQL5 / Python / Pine

- **Python:** FEASIBLE but HIGH effort — PyTorch/TensorFlow; DLSA code as scaffold; licensed data needed
- **MQL5:** NOT FEASIBLE — deep-learning inference must run externally; signals pre-computed
- **Pine Script:** NOT FEASIBLE
