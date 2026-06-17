# Implementation Candidate: Intramonth Momentum Cycle (PreTOM)

**Slug:** `intramonth-momentum-pretom`
**Score:** 61 (Worth Researching) | latent 61 (no cap)
**Source:** Nathan/Suominen/Tasa, SSRN 6426026 (March 2026)
**Quantpedia Awards 2026: 2nd place**

## Why it's an implementation candidate

- latent_score = 61 ≥ 60 threshold
- Logic is fully inspectable and codeable: standard cross-sectional momentum WML + calendar filter
- Python implementation is straightforward with standard financial data libraries
- No closed-source or proprietary components required

## Implementation Notes

### Signal Construction (Python)
1. Pull daily OHLCV + market cap for US equity universe (yfinance, CRSP via WRDS, or Compustat).
2. Each month-end, compute 12-1 month returns for all stocks (prior 12 months excluding most
   recent month — standard skip-month). Sort into deciles. Top decile = winners; bottom decile = losers.
3. Build a value-weighted long-winners / short-losers portfolio.
4. Identify the PreTOM window: trading days T-9 through T-4 relative to the last trading day
   of each calendar month. (Post May 2024: T-8 to T-3 under T+1 settlement.)
5. Enter the WML portfolio at the open on day T-9 (or T-8 post-May 2024).
6. Exit the WML portfolio at the close on day T-4 (or T-3 post-May-2024).
7. Hold T-bills / cash during all other trading days.

### Key Assumptions to Validate
- WML formation period: assumed 12-1 month. Paper details NOT CONFIRMED in accessible sources.
  Must validate by accessing full paper or running multiple formation periods (6-1, 12-1) to
  reproduce the $18.78 dollar growth figure.
- Universe: assumed NYSE/AMEX/NASDAQ CRSP universe with standard filters (price ≥ $5,
  market cap above NYSE 20th percentile to avoid micro-caps). NOT CONFIRMED.
- Value-weighting confirmed; equal-weighting also tested (per secondary sources).

### Cost Model
- Novy-Marx & Velikov (2016) effective spread estimates should be applied.
- Alternative: use 5bps/side for large-cap stocks; 20-50bps/side for bottom-decile (small-cap)
  losers. The short leg cost is the critical unknown.

### Settlement Window Update (CRITICAL)
- Strategy window has shifted 1 day since May 2024 (SEC T+1 settlement):
  - Pre-May 2024: T-9 to T-4
  - Post-May 2024: T-8 to T-3
- Any implementation must use the current T+1 window.

### Risk Management (recommended additions)
- Momentum crash filter: add a market-distress indicator (e.g., VIX > 40 or S&P 500 drawdown
  from recent peak > 20%) to pause the strategy or flip the long leg to cash during crash regimes.
- Position limits: cap individual stock weights at 5% of portfolio to limit concentration.
- Stop-loss: consider 10% trailing stop at monthly-entry price for the overall portfolio.

## Performance Baseline (for validation)
- PreTOM-only WML: $1 → $18.78 over 1980-2025 (CLAIMED)
- Non-PreTOM WML: $1 → $2.37 over same period (CLAIMED)
- Full WML: $1 → $45.06 (CLAIMED)
- Any implementation should target replicating the $18.78 trajectory before live trading.

## Status
- **Not yet independently replicated.** This is a research-stage candidate.
- Full paper PDF not accessible (HTTP 403 as of 2026-06-17). Must access before implementation.
- Sharpe ratio and maximum drawdown NOT REPORTED — critical unknowns before live deployment.
