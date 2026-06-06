# Implementation Candidate: 0DTE Conditional Trading Rules (SPXW)

**Score:** 66 (latent 66, capped at 66) — Worth Researching
**Strategy file:** research/strategies/0dte-conditional-rules-spxw.md
**Added:** 2026-06-06

---

## Why This is an Implementation Candidate

- Latent score 66 ≥ 60 threshold ✓
- Full open Python replication package at https://github.com/vilkovgr/0dte-strategies ✓
- Strategy logic is fully inspectable from the annotated paper markdown ✓
- Fixed-time rule (10:00 ET logistic signal) is automatable ✓

**Caveat:** Performance is CLAIMED, UNVERIFIED (working paper, not peer-reviewed). The
implementation candidate status reflects that the logic is codeable and inspectable, NOT
that the performance claims are verified.

---

## Implementation Summary

**What to implement:** Conditional logistic entry in SPXW near-ATM multi-leg options at
10:00 ET, hold to settlement at 16:00 ET. Signal uses 10:00 ET implied state features.

**Best performing structure (OOS):**
- Put ratio spread 1×2: long 1 put + short 2 puts, near-ATM (|M−1| ≤ 0.01)
- OOS net Sharpe: 0.93; Hit rate: 54.2%; Mean net PnL: +8.5 bps/day

**Top-3 basket (diversified, recommended):**
- OOS net Sharpe: 0.82; Max drawdown: 3.2% of underlying; Mean net PnL: +6.2 bps/day

---

## Data Requirements

**Tier 1 (replication only — pre-built panels):**
- Download from GitHub LFS (~400 MB Parquet files)
- No external data subscription needed
- Reproduces all paper tables/figures
- `python code/run_replication.py`

**Tier 2 (full pipeline with live capability):**
- ThetaData API subscription (option-level SPXW data, historical)
- OR Massive API subscription
- Raw Cboe files are proprietary and excluded from the repo

**Implementation-ready data:**
- SPXW near-ATM option chain quotes at 10:00 ET daily
- SPX realized moments (rolling daily returns, variance, skewness)
- VIX term structure (VIX M1, M2 futures for slope)

---

## Implementation Steps

1. **Clone repository:** `git clone https://github.com/vilkovgr/0dte-strategies`
2. **Install dependencies:** `pip install -r requirements.txt` (Python ≥ 3.10)
3. **Run Tier 1 replication:** `python code/run_replication.py` — verify results match paper
4. **Review code/analysis/ scripts** for conditional model implementation details
5. **Subscribe to ThetaData** (or Massive API) for live/Tier 2 data if proceeding beyond
   replication
6. **Build execution layer:** SPXW options execution via Interactive Brokers, Tastytrade,
   or similar API supporting multi-leg index options orders
7. **Paper trading first:** Given the no-dynamic-hedging assumption and tail risk of put
   ratio spreads, paper trading for ≥ 3 months before capital deployment is strongly advised

---

## Key Implementation Risks

1. **Put ratio spread tail risk:** Short 2 puts vs. long 1 put means net short exposure to
   large downward gaps; max loss is theoretically unbounded below the second short strike
2. **Data subscription cost:** ThetaData historical + live data adds to operational cost;
   this must be factored into profitability calculations
3. **Execution timing:** Strategy requires option orders routed at or near 10:00 ET — any
   significant delay degrades signal
4. **Post-paper regime:** Strategy trained and OOS-tested through Jan 2026; real-time
   performance forward of this date is untested
5. **Not hedged:** The study's unhedged assumption may not match actual practitioner workflow

---

## Feasibility Rating

| Dimension | Assessment |
|-----------|-----------|
| Code availability | HIGH (full Python package on GitHub) |
| Data accessibility | MEDIUM (Tier 1 free; Tier 2 requires paid subscription) |
| Execution complexity | MEDIUM-HIGH (multi-leg SPXW orders, fixed 10:00 ET timing) |
| Capital requirements | LOW (notional size flexible; SPXW is highly liquid) |
| Monitoring burden | LOW-MEDIUM (one trade per day, fixed time window) |
| Overall feasibility | MEDIUM — viable for a quantitative practitioner with options data access |
