# SPXW Short-Dated Put-Writing with Kelly/VIX Dynamic Sizing

## Overview

Systematic cash-secured put selling on SPXW (S&P 500 weekly/daily options) with expirations
of 0–5 days to expiry, where position size is determined by one of three dynamic sizing models:
Kelly criterion, VIX-based volatility regime scaling, or a hybrid combining both. The strategy
harvests the well-documented Volatility Risk Premium (VRP) — the persistent gap between implied
volatility and subsequently realized volatility — while modulating exposure to avoid catastrophic
sizing during high-volatility regimes. The paper's contribution is not the discovery of VRP
(already well-established) but the empirical evaluation of Kelly/VIX sizing as a practical
framework for capturing it in ultra-short-dated SPXW options.

Source: arXiv 2508.16598 (Maciej Wysocki, August 2025)

---

## Asset Class / Timeframes

- **Market:** US Equity Index Options (SPXW / SPX weekly and daily options)
- **Underlying:** S&P 500 Index (SPXW)
- **Timeframe:** Daily entry, intraday execution; holding period 0–1 trading days
- **Options DTE:** 0–5 DTE (best results: 0–1 DTE per accessible summaries)
- **Style:** Short volatility / volatility risk premium harvesting

---

## Core Logic

1. **Instrument selection:** Sell put options on SPXW; target moneyness approximately 2%
   out-of-the-money (best-performing per accessible summaries)
2. **DTE selection:** Prefer ultra-short-dated contracts (0 or 1 DTE)
3. **Position sizing — three variants compared:**
   - **Kelly:** Full or fractional Kelly criterion: `f* = (μ − r) / σ²`, where μ and σ² are
     estimated from rolling historical options P&L
   - **VIX-based:** Scale position size inversely with current VIX level; reduce exposure
     when VIX is elevated
   - **Hybrid:** Combine Kelly and VIX signals — Kelly determines the "base" size, VIX acts
     as a dampening multiplier in high-volatility regimes
4. **Volatility estimator:** Garman-Klass estimator with 63-day memory window emerges as best
   performer
5. **Exit:** Hold to expiry (same session for 0 DTE); receive maximum premium if put expires
   worthless; no explicit early stop-loss rule described in accessible summaries
6. **Cost modeling:** Entries and exits priced using 1-minute OHLC + bid-ask quotes from CBOE

---

## Economic Rationale

The Volatility Risk Premium (VRP) is one of the most thoroughly documented risk premia in
financial markets. Implied volatility of equity index options systematically and persistently
exceeds subsequently realized volatility; sellers of downside protection earn this premium over
time. The economic mechanism: institutional investors demand put options as portfolio insurance;
their willingness to pay above the actuarially "fair" price creates a structural transfer of
wealth to put sellers.

**When the edge should disappear:**
1. During market crashes (e.g., COVID March 2020, Oct 2008, Aug 2015, Feb 2018 Volmageddon),
   when realized vol spikes far above implied vol and short put positions sustain large losses.
2. When the VIX is already depressed (sub-12 range), the absolute premium per contract shrinks
   to levels where bid-ask spread alone consumes the edge.
3. When systematic put-selling becomes heavily crowded, as it was in 2023–2024; crowding
   compresses the VRP and amplifies the disorderly exit risk in any crash.
4. When the strategy is sized too aggressively (Kelly overbetting), a single gap-down event
   can cause ruin even if the long-run expectation is positive.

This mechanism is genuinely falsifiable: the edge is predicted to fail when realized vol > implied
vol — a directly measurable condition, not a post-hoc narrative.

---

## Entry Conditions

- **Sell** one (or fractional-Kelly-sized) SPXW put
- **Strike:** Approximately 2% below current index level (2% OTM = best-performing moneyness)
- **DTE:** 0 or 1 DTE
- **Entry time:** Not specified in accessible summaries
- **Always-on approach:** No conditional filter on implied state (unlike Vilkov's conditional
  method); the sizing formula, not a binary on/off signal, controls risk
- **Position size:** Determined by Kelly fraction, VIX-based scalar, or hybrid combination

---

## Exit Conditions

- Default: Hold to expiry (0 DTE → same session, 1 DTE → next session)
- Maximum profit achieved when put expires worthless (collected premium fully retained)
- No hard stop-loss rule described in accessible summaries
- Large adverse moves (flash crash) would require forced exit or margin call in a cash-secured
  structure

---

## Risk Management

- **Primary mechanism:** Dynamic sizing — reduce notional exposure when VIX is elevated
  (hybrid/VIX sizing variants)
- **Kelly sizing:** Controls long-run expected geometric growth but does NOT prevent
  single-session catastrophic losses during gap-down events
- **Tail risk:** Short put positions have asymmetric risk — many small wins, occasional large
  losses; a -5% intraday index drop can wipe out months of premium at typical leverage
- **Explicit stop-loss:** NOT REPORTED in accessible summaries
- **Margin:** Cash-secured puts require full strike × multiplier in cash margin (large capital
  requirement relative to premium received)
- **Overall assessment:** Weak (3/10) — sizing-based risk management handles normal vol
  regimes but provides no floor for tail events; no explicit stop/max-loss rule found

---

## Indicators Used

- Garman-Klass realized volatility estimator (63-day rolling window) — best performer
- VIX index level (standard 30-day implied vol)
- VIX9D (short-term VIX, 9-day) — mentioned as data source in the paper
- Implied volatility of specific options (from 1-min bid-ask data from CBOE)
- Kelly fraction: derived from rolling estimate of options P&L distribution (mean, variance)

---

## Transaction Costs & Capacity

- **Cost modeling rigor:** Among the best in this archive for options strategies — 1-minute
  OHLC plus bid and ask quotes from CBOE used directly. This explicitly captures bid-ask spread
  costs.
- **Commissions / clearing fees:** NOT REPORTED in accessible summaries
- **Slippage concern:** Far OTM 0DTE puts are thin books; bid-ask spread as % of premium can
  be large (e.g., a $0.02/$0.04 market on a $0.05-premium option = 40–60% round-trip cost);
  this may significantly reduce net returns at higher notional sizes
- **Capacity:** NOT REPORTED — SPXW has deep overall liquidity, but deep OTM short-dated puts
  thin out rapidly; strategy capacity is likely bounded at ~$1–5M before market impact matters
- **Overall:** 6/10 — bid-ask costs explicitly modeled, a meaningful positive; commissions
  and capacity not addressed

---

## Backtest Evidence

- arXiv preprint only (August 2025, Maciej Wysocki, single author)
- Data source: CBOE (1-minute OHLC + bid-ask for SPXW and VIX/VIX9D)
- Backtest period: NOT REPORTED in accessible summaries (paper mentions 2024 specifically;
  daily SPXW expirations became liquid ca. 2022; likely covers ≤5 years)
- CAGR: ~20–25% annualized for optimal parameter combination (CLAIMED, UNVERIFIED — extracted
  from a single web search snippet of the paper; not from a primary accessible source)
- Sharpe Ratio: NOT REPORTED
- Max Drawdown: NOT REPORTED
- Parameter space: "broad design space" — moneyness levels, volatility estimators, memory
  horizons — optimal parameters selected from this grid (overfitting risk)
- No OOS test or walk-forward described in accessible summaries

## Forward-Test Evidence

NOT REPORTED — no live or paper-trading results found in accessible sources.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED | NOT REPORTED | — |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | ~20–25% (optimal params) | CLAIMED, UNVERIFIED | Web search excerpt of arXiv 2508.16598 |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | 0–1 trading days | CLAIMED, UNVERIFIED | arXiv 2508.16598 abstract |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% (always-on sizing; no conditional on/off) | CLAIMED, UNVERIFIED | arXiv 2508.16598 |

**Automatic scrutiny trigger check:** CAGR 20–25% is below the 50% threshold. No other
metrics are available to trigger automatic scrutiny. The absence of Sharpe and max DD data
is itself noteworthy — these are the primary metrics for any short-vol strategy.

---

## Community Sentiment

No community discussion found in accessible sources. The paper is a recent arXiv preprint
(August 9, 2025) by a single author; no forum threads, independent replications, or academic
reviews were found during this search session.

The broader short-volatility community (r/thetagang, TastyTrade forums, Options Alpha users)
is generally familiar with the VRP concept, though they tend to focus on individual equity
options rather than SPXW index puts. No specific discussion of this paper's methodology was
found.

---

## Why This Might Be Nothing

### 1. Overfitting from broad parameter selection (primary concern)

The paper explicitly explores "a broad design space, including moneyness levels, volatility
estimators, and memory horizons." Reporting the best-performing combination from this grid
almost certainly overstates forward-looking performance. The optimal configuration
(Garman-Klass, 63-day, 2% OTM, 0-1 DTE) was selected *because* it performed best in sample.
Without a held-out OOS test or walk-forward, this is textbook in-sample parameter selection
bias. The "20-25% CAGR for the optimal parameter combination" almost certainly includes
significant selection-bias inflation.

### 2. Short backtest period in a structurally favorable regime

Daily SPXW options with meaningful liquidity effectively began ca. 2022. The likely backtest
period covers 2022–2025 at most, which includes: (a) a significant equity selloff and vol spike
(2022), and (b) a sustained low-VIX bull market (2023–2025). This four-year window is short
relative to the several tail events short-vol strategies must survive. There is no evidence in
accessible summaries that the strategy was tested through COVID (March 2020), Volmageddon
(Feb 2018), or the 2015 China flash crash.

### 3. Catastrophic tail risk not documented

Short put strategies are structurally exposed to large-loss events that are impossible to
detect from CAGR alone. The "robust drawdown control" comment applies specifically to 2024's
low-vol conditions, not to crisis periods. For a short-vol strategy, the critical metric is
max drawdown during a crash, which is NOT REPORTED. Without it, it is impossible to evaluate
whether the 20-25% CAGR persists on a risk-adjusted basis.

### 4. Cost ambiguity on far OTM short-dated options

For a put 2% OTM with 0 DTE, the option premium might be $0.05–$0.20 depending on conditions.
A typical bid-ask spread of $0.02–$0.05 represents 10–100% of the premium in round-trip costs.
The paper uses bid-ask data, which is positive, but (a) whether fills are assumed at the
midpoint or bid/ask is unspecified in accessible summaries, and (b) commissions and regulatory
fees (SEC fee, OCC clearing) are not explicitly modeled.

### 5. Crowding in the short-vol space

Zero-DTE put selling on SPX became one of the most popular strategies among retail and
institutional traders in 2023–2024. This crowding effect means:
(a) The VRP has likely been partially arbitraged over the period of the backtest.
(b) In any crash, simultaneous unwinding of many short-vol positions amplifies the loss
    relative to what historical data (from less-crowded regimes) would predict.

### 6. Kelly overbetting in practice

Full Kelly is notoriously difficult to estimate accurately for options (non-normal P&L
distribution, fat tails), and even small errors in the estimate of μ lead to overbetting
and eventual ruin. Fractional Kelly (quarter or half-Kelly) is the standard practitioners'
approach. Whether the paper uses full or fractional Kelly is not specified in accessible
summaries.

**The one piece of evidence that would most change my mind:** An independent replication
with walk-forward testing and explicit reporting of max drawdown during at least one
significant market stress event (VIX > 40), together with a peer review confirming that the
parameter selection was OOS.

---

## Strengths / Weaknesses

### Strengths

- The Volatility Risk Premium mechanism is one of the best-documented risk premia in financial
  markets — peer-reviewed evidence spans decades across multiple markets
- Cost modeling via 1-minute actual bid-ask quotes from CBOE is more rigorous than most
  options papers in the archive
- The hybrid Kelly+VIX sizing is a logical and theoretically-grounded improvement over static
  sizing
- SPXW is among the world's most liquid options markets; no data access barrier for live trading
- The strategy is implementable in Python with ThetaData or CBOE data subscription

### Weaknesses

- arXiv preprint only; no peer review; single-author paper
- "Broad design space" language signals overfitting from parameter selection
- Sharpe ratio and max drawdown NOT REPORTED — the two most critical metrics for short-vol
- 20-25% CAGR from a single non-primary search snippet — insufficient for confident evaluation
- No OOS test or walk-forward described
- Tail risk (crash performance) not validated in accessible summaries
- Backtest period likely short (≤5 years) and excludes major vol spikes pre-2020

---

## Confidence Score

### Per-dimension breakdown

| Dimension | Weight | Score | Weighted | Rationale |
|---|---|---|---|---|
| Logic / economic soundness (falsifiable) | 3 | 8 | 24 | VRP mechanism is peer-reviewed and falsifiable; conditions for failure clearly stated |
| Transparency & reproducibility | 2 | 5 | 10 | Rules described at level of abstraction in accessible summaries; full formulas require the paper PDF |
| Evidence auditability | 2 | 2 | 4 | arXiv preprint only; no peer review; no independent replication; single author |
| Risk management quality | 2 | 3 | 6 | Dynamic sizing is the sole risk control; no explicit stop-loss; tail risk unaddressed |
| Cost & capacity realism | 2 | 6 | 12 | Bid-ask costs modeled via 1-min actual quote data (positive); commissions and capacity not addressed |
| Honest treatment of drawdowns / failure | 1.5 | 2 | 3 | Max DD not reported; crash periods not validated; mentions "drawdown control" only for 2024 |
| Robustness evidence (OOS / walk-forward) | 1.5 | 2 | 3 | No OOS or walk-forward; broad parameter grid with in-sample optimal reported |
| Survived independent scrutiny | 1 | 1 | 1 | No community discussion or independent review found |
| **Total** | **15** | — | **63** | |

`latent_score = round(63 / 150 × 100) = 42`

Evidence auditability score = 2 (≤ 4) → `confidence = min(42, 59) = 42`

**`latent 42 (capped to 42 — Evidence auditability = 2: arXiv preprint only, no peer review, single author, no independent replication)`**

**Band: Experimental (40–59)**

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Moderate — requires options data subscription (CBOE, ThetaData, or
  OptionsDX), Garman-Klass volatility calculation (4 parameters: open, high, low, close of
  the underlying), rolling Kelly estimation, and VIX monitoring
- **Scalability:** Limited — SPXW far OTM short-dated puts have meaningful bid-ask spreads;
  strategy capacity approximately $1–5M before market impact becomes significant
- **Automation feasibility:** Technically feasible with 1-minute options data feed, Python +
  Interactive Brokers TWS API or Tradier API; relatively straightforward execution logic

---

## MQL5 / Python / Pine Feasibility

- **Python:** Feasible — options data via ThetaData ($25/month) or OptionsDX; Garman-Klass
  estimator is a standard formula; Kelly calculation straightforward; execution via IB TWS API
  or Alpaca. No open code found for this specific paper.
- **MQL5 (MetaTrader):** Not practical — MetaTrader does not natively support SPXW options
  trading
- **Pine Script (TradingView):** Not practical — TradingView does not provide intraday SPXW
  options data

---

## Similar Strategies

- `0dte-conditional-rules-spxw` — same market (SPXW/SPX), different approach: Vilkov uses
  conditional entry rules (logistic regression on implied-state features, sometimes no trade);
  this paper uses always-on sizing adjustment with no conditional entry filter; complementary
- `vix-etn-dual-approach` — also harvests the VRP but via VIX ETNs (VXX/SVXY) rather than
  direct options; higher leverage risk but no bid-ask issues with individual options

---

## Red Flags

- `BROAD DESIGN SPACE` + no OOS = strong overfitting suspicion
- Single-author arXiv preprint; no peer review
- Sharpe ratio and max drawdown NOT REPORTED
- 20-25% CAGR from a single web summary snippet only (not a verified primary-source quote)
- Backtest period likely ≤5 years and excludes major crash events pre-2020
- No explicit stop-loss or tail-risk management beyond sizing dampening

---

## Source Links

- arXiv 2508.16598: https://arxiv.org/abs/2508.16598 (retrieved 2026-06-08; HTTP 403 for HTML/PDF; abstract and key details from web search excerpts)
- ResearchGate mirror: https://www.researchgate.net/publication/394942155 (retrieved 2026-06-08; HTTP 403)
- ADS abstract: https://ui.adsabs.harvard.edu/abs/2025arXiv250816598W/abstract (retrieved 2026-06-08; HTTP 403)
- CBOE PutWrite benchmark context: Bondarenko (2019) — https://cdn.cboe.com/resources/education/research_publications/PutWriteCBOE19_v14_by_Prof_Oleg_Bondarenko_as_of_June_14.pdf

---

## Analyst Notes

**FACTS:** Author: Maciej Wysocki. arXiv submission: August 9, 2025. Categories: q-fin.PM,
q-fin.CP, q-fin.PR, q-fin.TR. Strategy: systematic put-writing on SPXW options (0–5 DTE).
Three sizing approaches compared: Kelly criterion, VIX-based scaling, and hybrid. Data:
1-minute OHLC + bid-ask from CBOE; VIX and VIX9D also used. Best-performing configuration:
Garman-Klass estimator (63-day window), ~2% OTM puts, 0–1 DTE. Claimed CAGR ~20–25% for
optimal parameters. Sharpe, max DD, win rate, backtest period: NOT REPORTED in accessible
summaries.

**ANALYSIS:** The VRP mechanism itself is extremely well-established and not in question. The
paper's specific contribution is the comparison of Kelly, VIX-scaling, and hybrid sizing for
ultra-short-dated SPXW options — an area the paper correctly notes is "underdeveloped in the
literature." The bid-ask modeling is a genuine strength. The critical weaknesses are: (1) the
undisclosed parameter selection process from a "broad design space" without OOS testing,
(2) the absence of Sharpe and max DD data in accessible summaries, and (3) the likely short
backtest period that excludes pre-2020 crash events. The Garman-Klass 63-day window, while
plausible, is exactly the kind of parameter that would be found by in-sample optimization if
the training data happened to have favorable properties.

**ASSUMPTIONS:** (1) The 20-25% CAGR figure is net of bid-ask spread costs (since the paper
uses bid-ask data). (2) The backtest likely covers 2022–2025 given SPXW daily-expiry liquidity
timeline. (3) Max drawdown during a crash event (COVID-level selloff) would be materially
larger than any rolling window drawdown in 2022–2025. (4) The Kelly estimate uses historical
options P&L rather than a theoretical Black-Scholes-based estimate.

---

## Future Research Needed

1. Access full paper PDF to extract Sharpe ratio, max DD, Calmar, win rate, and exact
   backtest period
2. Independent replication with explicit crash-period performance (March 2020 VIX > 80,
   Feb 2018 Volmageddon, Aug 2015 China flash crash)
3. Walk-forward or OOS parameter stability test for Garman-Klass 63-day specification
4. Comparison to CBOE S&P 500 PutWrite Index (PUT) as a simple benchmark
5. Explicit Kelly fraction used (full/half/quarter) and sensitivity analysis
6. Community reception and independent replications (no public forum discussion found yet)
