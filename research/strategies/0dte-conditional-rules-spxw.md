# 0DTE Conditional Trading Rules (SPXW)

## Overview

A systematic approach to same-day-expiration SPX options (SPXW 0DTEs) that exploits the
variance risk premium (VRP) selectively, using logistic regression on 10:00 ET implied-state
features to decide whether to open a position and in which direction. The central finding:
the unconditional VRP is too small to monetize after realistic trading costs, but conditional
timing on a strict out-of-sample protocol achieves net Sharpe ratios of 0.82–0.93 for the
best strategy families.

- **Author:** Grigory Vilkov (Professor, Frankfurt School of Finance & Management)
- **Paper:** "0DTE Trading Rules" (SSRN 4641356; first posted December 2023; updated to
  February 2026)
- **Replication code:** Open Python package at https://github.com/vilkovgr/0dte-strategies
- **Status:** Working paper — NOT peer-reviewed. Related paper "0DTEs: Trading, Gamma
  Risk and Volatility Propagation" (Dim/Eraker/Vilkov) is Revise & Resubmit at Review of
  Financial Studies (as of research date).
- **Data period:** September 2016 – January 2026 (~9.5 years, 2,509 trading days)
- **OOS period:** April 2019 – January 2026 (strict no-look-ahead protocol)

---

## Asset Class / Timeframes

- **Asset:** SPXW options (S&P 500 Index Weeklys — European-style, cash-settled)
- **Timeframe:** Intraday, fixed-time entry + hold to expiration
  - Entry: 10:00 ET
  - Exit: 16:00 ET settlement (same day; zero days to expiration)
- **Market:** US Equity Index Options (SPX/SPXW)
- **Moneyness range studied:** 0.98 to 1.02 (approximately ±2% OTM); representative
  near-ATM configuration (|M−1| ≤ 0.01) used in conditional models

---

## Core Logic

Six strategy families are studied, each with both long and short variants:

1. **Straddles / Strangles** — delta-neutral, volatility-focused (long or short combined
   call+put)
2. **Iron Butterflies / Condors** — bounded payoff, short-vol, credit-receiving structures
3. **Risk Reversals** — skew-focused, open-tailed (long call / short put or vice versa)
4. **Bull Call / Bear Put Spreads** — directional, bounded risk
5. **Call / Put Ratio Spreads 1×2** — asymmetric tail exposure (e.g. long 1 call, short
   2 calls)
6. Individual puts and calls at various moneyness points

The **unconditional approach** (buy/sell every day at 10:00 ET): Mean PnL near zero or
negative for most structures; unconditional annualized Sharpe ranges from −0.15 to +0.32;
downside risk large relative to mean carry.

The **conditional approach**: A logistic regression classifier trained on 10:00 ET implied
state features (IV level, skew proxies, term structure slopes, lagged realized SPX moments,
lagged strategy PnL) predicts the sign of same-day PnL. The model determines whether to
open a position, and in which direction. Expanding or rolling 252-day estimation window,
strict no-lookahead OOS from April 2019.

---

## Economic Rationale

**Primary mechanism:** Variance risk premium (VRP). Implied variance (as embedded in 0DTE
option prices) tends to exceed realized variance, creating a structural premium for volatility
sellers. Market makers are the dominant short-vol participants; retail buyers systematically
overpay relative to realized outcomes (Bogousslavsky/Muravyev: retail 0DTE trades lose 4.7%
on average).

**Why the edge should be absent:**
- When realized variance is high relative to implied (e.g. sudden intraday gap/jump), the
  VRP collapses and sellers lose.
- When bid-ask spreads are wide relative to the premium (always true unconditionally for
  0DTEs — the paper documents this explicitly).
- In regimes with high realized skewness, directional asymmetry dominates the VRP signal and
  neither systematic side is reliably profitable.
- Post-2022 daily-expiry expansion altered microstructure; some strategies shifted behavior.

**Conditional timing rationale:** The logistic features capture same-morning implied-state
signals that forecast whether the VRP will be monetizable that day. Directional skewness
(not just variance level) is the dominant driver — directional classification outperforms
magnitude prediction.

**ANALYSIS:** The conditional VRP rationale partially passes the falsifiability test. The
paper names the failure conditions (insufficient VRP, high realized skewness, regime shift
post-2022), which is a positive signal. However, the feature-to-outcome mapping (IV/skew at
10:00 ET predicts same-day PnL) is primarily empirical, not structural — there is no clear
model for *why* 10:00 ET skew should forecast 10:00–16:00 outcomes, beyond "the market
encodes information we can read from the surface." This weakens the rationale from structural
to informational/empirical.

---

## Entry Conditions

**Conditional (OOS recommended) approach:**
1. At 10:00 ET, observe the 10:00 ET implied state of SPXW near-ATM options:
   - Current IV level and term structure slope
   - Skew proxy (e.g. OTM put vs. call IV differential or risk reversal price)
   - Lagged realized SPX daily moments (return, variance, skewness from prior days)
   - Lagged strategy-specific PnL (rolling)
2. Run the logistic classifier (trained on 252-day rolling expanding window; OOS estimated
   parameters only)
3. If classifier output exceeds threshold, open the strategy in the direction predicted
4. Specific moneyness: near-ATM (|M−1| ≤ 0.01 in the conditional models)
5. Execution: mid-quote baseline (most realistic available); half-spread per leg if
   bid-ask-based execution

**Important:** Positions are opened at a fixed time (10:00 ET), not at open. The paper also
tests 13:00 and 15:00 ET entry; the 10:00 ET window is the main analysis and provides the
longest holding period.

---

## Exit Conditions

- **No active exit:** All positions held to settlement at 16:00 ET (4:00 PM ET)
- **No dynamic intraday hedging** — this is explicitly a feature of the study: unhedged
  hold-to-settlement positions
- **No stop-loss:** The study does not implement intraday stops or early exits
- This means all gamma risk is accepted unhedged; a large intraday move cannot be reduced

---

## Risk Management

- Multi-leg bounded-risk structures (iron condors, spreads, ratio spreads) cap the maximum
  loss at the width of the spread minus the premium received (or the net debit for debit
  spreads)
- Straddles and strangles have theoretically unlimited loss potential (but are practically
  bounded by max intraday SPX move)
- Put ratio spreads 1×2 (best performer): long 1 put + short 2 puts = equivalent to net
  short an additional put beyond breakeven — meaningful tail risk on downward gaps
- The paper recommends "small tactical overlay with explicit turnover control, tail-risk
  budgeting, and ongoing OOS monitoring" — **not** a standalone core strategy
- **No formal position sizing rules defined** in the paper; portfolio-level risk management
  is the analyst's responsibility
- ES₁% for top-3 basket: −0.58% of underlying; worst-day outcomes are severe across
  strategies

---

## Indicators Used

**Signal features (conditional model, 10:00 ET observation):**
- IV level at 10:00 ET (implied volatility of near-ATM options)
- Term structure slopes (e.g. VIX term structure, maturity differentials)
- Skew proxies (OTM put vs. call IV or risk reversal price)
- Lagged realized SPX moments (return, variance, skewness)
- Lagged strategy-specific PnL (rolling)

**Specific formulas:** NOT REPORTED in accessible sources; exact feature engineering
documented in the code at https://github.com/vilkovgr/0dte-strategies

---

## Transaction Costs & Capacity

**Costs explicitly modeled (three tiers):**
1. **Mid-quote baseline** — best-case execution at midpoint
2. **Bid/ask execution** — half-spread per leg (realistic for liquid near-ATM SPX options)
3. **Half-spread + 0.5 bp slippage/fees** — most conservative scenario

**Gross-to-net degradation is material:**
- Iron butterfly/condor: Gross SR 0.77 → Net SR **negative** after costs. This is the key
  finding: seemingly attractive gross performance is entirely erased by costs.
- Put ratio spread: Gross SR 1.18 → Net SR 0.93 (most cost-resilient)
- Top-3 basket: Gross SR 1.12 → Net SR 0.82

**Capacity:** SPXW has extremely deep liquidity at near-ATM moneyness (largest options
market by notional volume). Capacity is not a binding constraint at the notional sizes
typical of a tactical overlay. However, market impact for large institutional size (not
studied) could compress the edge.

**ANALYSIS:** The cost modeling is among the most rigorous in this strategy archive — three
explicit tiers, with results shown for each. The paper itself uses the gross-to-net gap as
a key analytical finding. The one concern: half-spread assumptions for near-ATM SPX options
during normal liquidity hours are reasonable, but during intraday stress (which is precisely
when the strategy may be tested), spreads widen materially. The paper's ES₁% includes
tail-event days, but the cost model may not reflect tail-day spread widening.

---

## Backtest Evidence

**Unconditional (full in-sample):** Annual Sharpe −0.15 to +0.32 across strategy families;
unconditional VRP too small to monetize. CLAIMED, UNVERIFIED.

**Conditional OOS (April 2019 – January 2026):**
- Strict no-lookahead rolling/expanding 252-day estimation window
- Benjamini-Hochberg FDR multiple testing corrections applied
- This constitutes a genuine OOS evaluation. CLAIMED, UNVERIFIED (source is the
  SSRN working paper and GitHub annotated paper; not peer-reviewed).

---

## Forward-Test Evidence

NOT REPORTED. No live trading results referenced.

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio (unconditional, gross) | −0.15 to +0.32 | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper (Vilkov 2026) |
| Sharpe Ratio (put ratio spread, OOS gross) | 1.18 | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper |
| Sharpe Ratio (put ratio spread, OOS net) | 0.93 | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper |
| Sharpe Ratio (top-3 basket, OOS gross) | 1.12 | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper |
| Sharpe Ratio (top-3 basket, OOS net) | 0.82 | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper |
| Sharpe Ratio (all strategies basket, OOS net) | 0.25 | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate (put ratio spread, OOS) | 54.2% | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper |
| Win Rate (strangle/straddle, OOS) | 50.8% | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown (top-3 basket, OOS) | 3.2% of underlying | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper |
| Maximum Drawdown (all strategies basket, OOS) | 5.1% of underlying | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | ~1,764 trading days in OOS (one trade/day maximum) | CLAIMED, UNVERIFIED | SSRN 4641356 (OOS period 4/2019–1/2026) |
| Average Trade Duration | ~6 hours (10:00–16:00 ET) | CLAIMED, UNVERIFIED | SSRN 4641356 |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | NOT REPORTED (conditional entry — not every day) | NOT REPORTED | — |
| Expected Shortfall ES₁% (top-3 basket) | 0.58% of underlying | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper |
| Mean Net PnL per trade (put ratio spread, OOS) | +8.5 bps of underlying | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper |
| Mean Net PnL per trade (top-3 basket, OOS) | +6.2 bps of underlying | CLAIMED, UNVERIFIED | SSRN 4641356 / GitHub annotated paper |

---

## Community Sentiment

**Academic context (not direct community critique of this paper):**
- Bogousslavsky/Muravyev (SSRN 4682388, "An Anatomy of Retail Option Trading," 2025): retail
  0DTE trades lose 4.7% relative to other option trades; retail option purchases lose 3.95%;
  naked option sales earn ~20% (only 13% of retail trades). This provides market context for
  who is on the other side of systematic short-vol strategies.
- Cboe institutional research confirms 0DTE does not destabilize markets; >95% of SPX 0DTE
  trading is in limited-risk format.
- Vilkov's related paper "0DTEs: Trading, Gamma Risk and Volatility Propagation" is R&R at
  Review of Financial Studies — suggesting the broader research program is credible enough
  for top-journal consideration.
- No independent replication or critique of the *conditional rules specifically* was found
  in accessible community forums.

**From the paper itself (honest self-assessment):**
"Disciplined 10:00 ET conditional rules...deliver economically meaningful net performance for
selected strategies and diversified baskets, pointing to selective timing opportunities rather
than to a broad unconditional edge."

---

## Why This Might Be Nothing

**1. Regime luck / low-vol environment:** The OOS period (April 2019 – January 2026) includes
the COVID crash (which is severe but brief), 2022 bond market stress, but is dominated by the
2014–2024 extended low-realized-vol regime. The conditional model's performance depends on
the features being predictive in the evaluation sample; this cannot be tested out-of-sample
on a truly independent hold-out (all available SPXW data is used).

**2. The 10:00 ET features may be spuriously predictive:** A logistic model trained on rolling
252-day windows has limited degrees of freedom. With multiple candidate features (IV level,
slope, skew proxy, lagged moments, lagged PnL), there is meaningful risk of data-snooping
within the conditional signal construction. The BH correction addresses multiple strategy
comparisons but not within-feature selection.

**3. The put ratio spread's tail risk is underweighted:** The strategy with the best
conditional results (put ratio spread, net SR 0.93) is structurally a short put beyond
breakeven. In a gap-down environment (circuit-breaker event, overnight gap below second short
strike), the theoretical loss is substantially larger than the premium received. The 3.2% max
drawdown may not capture a true 1987- or 2008-style tail event since the data starts only in
2016.

**4. Data not independently reproducible:** The strategy requires SPXW option-level data from
Cboe (proprietary) or ThetaData/Massive API (paid subscription). While the GitHub package
includes pre-built derived panels, an independent researcher cannot verify the raw data
processing chain without the original Cboe files. This is a material verification gap.

**5. Post-2022 structural break:** The paper documents that the expansion of daily SPXW
0DTE expirations in April 2022 changed microstructure (dealer gamma dynamics, bid-ask spread
behavior). The conditional model's OOS performance post-2022 may differ from the full OOS
period statistics reported.

**6. No dynamic hedging assumption:** Real practitioners trading 0DTEs frequently delta-hedge.
The study's "hold to expiration unhedged" assumption is unrealistic for institutional-size
positions and ignores intraday gamma management. The net SR of 0.82 is for the unhedged
case; a hedged version might look different.

**7. The one piece of evidence that would most change my mind:** An independent replication
by a separate research group using Refinitiv or a different data vendor, confirming the
OOS net Sharpe of 0.82 for the top-3 basket conditional strategy across the same 2019–2026
period. This would validate both the data integrity and the conditional signal structure.

---

## Strengths / Weaknesses

**Strengths:**
- Full open Python replication package on GitHub
- Explicit three-tier cost modeling with gross-to-net comparison
- Strict no-lookahead OOS protocol with multiple testing corrections
- Honest: upfront that unconditional VRP is insufficient; recommends conditional rules as
  small tactical overlay, not a core strategy
- Tail risk explicitly reported (ES₁%, max drawdown, worst-day)
- Data period spans 9.5 years including multiple volatility regimes

**Weaknesses:**
- Not peer-reviewed (working paper)
- Single market (SPXW/SPX only); no multi-index or cross-market robustness
- Raw data proprietary; independent verification requires paid subscriptions
- No dynamic hedging — implementation-unrealistic for institutional scale
- Post-2022 regime shift not fully resolved
- Feature engineering details require reading the code (not fully specified in the paper)
- No CAGR, Sortino, Calmar, or expectancy per trade reported

---

## Confidence Score

**Per-dimension breakdown:**

| Dimension | Score (0–10) | Weight | Weighted |
|-----------|-------------|--------|---------|
| Logical / economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 9 | 2 | 18 |
| Evidence auditability | 6 | 2 | 12 |
| Risk management quality | 5 | 2 | 10 |
| Cost & capacity realism | 8 | 2 | 16 |
| Honest treatment of drawdowns / failure | 8 | 1.5 | 12 |
| Robustness evidence (OOS / walk-forward) | 6 | 1.5 | 9 |
| Survived independent scrutiny | 4 | 1 | 4 |

Weighted sum: 99 / 150 × 100 = **latent 66**

**Verification cap:**
Evidence auditability = 6 → cap at 74.
min(66, 74) = **66 (capped at 66 pending peer review: SSRN working paper only; open code
read but raw data proprietary and not independently reproducible)**

**Band:** Worth Researching (60–74)

---

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Medium. The strategy requires real-time options data (IV, skew) at 10:00
  ET, pre-trained logistic model updated on a rolling window, and routing multi-leg SPXW
  orders (typically available through brokers like Interactive Brokers, Tastytrade, etc.)
- **Scalability:** High notional capacity (SPXW is the most liquid index options contract
  globally). Strategy scale is limited by one trade per day (fixed entry time), not by
  liquidity.
- **Automation feasibility:** HIGH for institutions with options data feeds. The strategy
  is fully rule-based with a fixed 10:00 ET signal window. The Python codebase provides the
  analytical framework; execution layer requires an options broker API.

---

## MQL5 / Python / Pine Feasibility

- **Python:** HIGHLY FEASIBLE. Full Python replication package exists at
  https://github.com/vilkovgr/0dte-strategies. Data ingestion via ThetaData API. Signal
  generation, OOS evaluation, and table generation all available. Requires `python ≥ 3.10`,
  see `requirements.txt`.
- **MQL5:** Not applicable. SPXW options are not tradeable through MetaTrader.
- **Pine Script (TradingView):** Not applicable. TradingView does not support SPX options
  strategy execution.

---

## Similar Strategies

- `vix-etn-dual-approach` — also exploits volatility risk premium but via VIX ETN positions
  (not options); different instrument and time horizon
- `vix-cmf-ml-walkforward` — ML prediction on VIX CMF term structure; same asset class
  (volatility), different instrument (futures vs. options)
- `industry-trend-century` — unrelated mechanism but same category (US equity systematic);
  Worth Researching band

---

## Red Flags

- **Not peer-reviewed**: SSRN working paper; status as of February 2026
- **Data requires paid subscription**: Raw Cboe data proprietary; Tier 2 replication needs
  ThetaData or Massive API
- **Post-2022 regime shift partially documented but unresolved**
- **No dynamic hedging**: Implementation-unrealistic for larger positions
- **Single-market study (SPXW only)**: No robustness across other markets or indices
- **Best strategy (put ratio spread) has meaningful tail risk** not fully captured by a
  9.5-year sample that starts post-GFC

---

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| https://github.com/vilkovgr/0dte-strategies | 2026-06-06 | Open replication package; README + annotated paper markdown |
| https://github.com/vilkovgr/0dte-strategies/blob/main/docs/paper/paper-annotated.md | 2026-06-06 | Full annotated paper in markdown; primary source for methodology and results |
| https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4641356 | 2026-06-06 | SSRN abstract page (HTTP 403 — not directly accessible) |
| https://www.researchgate.net/publication/376157104_0DTE_Trading_Rules | 2026-06-06 | ResearchGate page for publication status |
| https://www.lsu.edu/business/files/event-files/2025-finance-mardi-gras/retail_option_trading_v2.pdf | 2026-06-06 | Bogousslavsky/Muravyev "Anatomy of Retail Option Trading" — retail 0DTE losses context |

---

## Analyst Notes

**FACTS (from sources):**
- Vilkov is a Full Professor at Frankfurt School of Finance & Management (ResearchGate profile).
- "0DTE Trading Rules" posted SSRN December 2023, updated February 2026 (SSRN).
- Related paper "0DTEs: Trading, Gamma Risk and Volatility Propagation" is R&R at Review of
  Financial Studies (SSRN 4692190 abstract).
- Open Python replication package with ~3.5M option observations (GitHub README).
- OOS period April 2019 – January 2026 with strict no-lookahead expanding/rolling window
  (GitHub annotated paper).
- Unconditional annual Sharpe −0.15 to +0.32; conditional put ratio spread OOS net SR 0.93;
  top-3 basket OOS net SR 0.82 (GitHub annotated paper).
- Retail 0DTE trades lose 4.7% relative to other option trades (Bogousslavsky/Muravyev).

**ANALYSIS (my reasoning):**
- The paper's honesty about unconditional failure is a quality signal — it does not oversell.
  The conditional approach has a plausible but empirically-grounded (not structural) rationale.
- A net SR of 0.82 for the top-3 basket is attractive in the options space. However, the
  max drawdown of 3.2% of underlying is expressed as percentage of notional — for a typical
  overlay at say 2–5% NAV per position, this translates to very small absolute drawdown,
  consistent with the "small tactical overlay" recommendation.
- The put ratio spread dominance is a concern: this structure has left-tail asymmetry. The
  excellent OOS performance may partly reflect that the 2019–2026 OOS period didn't include
  a genuine 20%+ intraday SPX sell-off.
- The CAGR is NOT REPORTED, which makes it difficult to assess whether the net SR translates
  to meaningful absolute returns at implementation-realistic notional sizes.
- ASSUMPTION: Based on the ~6.2 bps mean net PnL per day for the top-3 basket × ~252 trading
  days × conditional entry frequency (unknown — not every day), the annualized return at 1×
  notional would be very small. The attractiveness is the SR, not absolute returns — this is
  a leveraged overlay, not a standalone strategy.

**ASSUMPTIONS (explicitly flagged):**
- The 252-day rolling window is assumed to be the same window used throughout all OOS
  calculations (this is stated as the minimum; expanding windows also tested).
- "Near-ATM" (|M−1| ≤ 0.01) is assumed to be the primary moneyness in the OOS conditional
  model, per the paper description.

---

## Future Research Needed

1. **Access the full SPXW data** via ThetaData to run the Tier 2 replication independently.
2. **Test post-2022 sub-period performance** of the conditional rules in isolation (only the
   post-expansion period).
3. **Investigate whether the conditional rules generalize** to other index options (QQQ, SPY,
   RUT, VIX options) or international markets (EuroStoxx 50 daily).
4. **Assess 2024–2026 performance** specifically: the 2022 bond market crisis and 2024
   election volatility may present the most relevant regime tests.
5. **Combine with the VIX ETN dual approach** (`vix-etn-dual-approach`): both exploit VRP;
   correlation between the two overlay strategies would determine diversification benefit.
