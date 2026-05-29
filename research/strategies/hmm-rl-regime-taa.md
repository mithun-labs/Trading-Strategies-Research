# HMM + RL Regime Tactical Asset Allocation

## Overview

A multi-asset tactical allocation strategy that uses a Hidden Markov Model (HMM) to classify
daily market regimes and either (a) applies fixed regime-conditioned allocation rules or
(b) trains a Reinforcement Learning (RL) policy on top of the HMM regime signal. Allocates
dynamically between US equities (SPY), long-term Treasuries (TLT), and gold (GLD).

Two related sources were used for this research:

1. **Academic preprint** — "Regime-Based Portfolio Allocation Using Hidden Markov Models and
   Reinforcement Learning" (Verma, Putri, Lesupi; arXiv:2605.27848, May 2026; SSRN:5785443,
   Nov 2025). Uses a **3-state** HMM + RL policy.

2. **Independent practitioner implementation** — "Regime-Based Portfolio Allocation: A Hidden
   Markov Model Approach to Tactical Asset Rotation" (Ejike Uchenna Splendor; GitHub:
   I-am-Uchenna/regime-allocation-strategy). Uses a **2-state** HMM without RL; provides
   specific performance metrics and explicit methodology — but with acknowledged in-sample optimism.

Both sources study the same mechanism; the metrics below are from the practitioner's open code
(where specific numbers are available). Metrics from the Verma et al. paper are NOT REPORTED
due to HTTP 403 blockade.

## Asset Class / Timeframes

- **Assets:** US Equities (SPY), Long-Term Treasuries (TLT), Gold (GLD) — all exchange-traded
  ETFs on major US exchanges
- **Timeframe:** Daily signal; daily rebalancing (full portfolio realignment)
- **Market:** Multi-asset US ETF portfolio

## Core Logic

### Step 1 — Regime Detection via HMM

A Gaussian HMM is fitted to daily VIX changes (Δ VIX) using the Baum-Welch (EM) algorithm:

**Practitioner implementation (2-state, open code):**
- Low Volatility state (50.26% frequency): mean Δ VIX = −0.074, 96.0% persistence
- High Volatility state (49.74% frequency): mean Δ VIX = +0.221, 88.3% persistence
- State decoded daily via Viterbi algorithm

**Verma et al. paper (3-state):**
- Low-volatility, transitional, and high-volatility regimes
- BIC-selected three-state Gaussian HMM on daily return + volatility features
- RL policy trained as a second layer on top of HMM state signal

### Step 2 — Regime-Conditioned Allocation

**Practitioner 2-state rules:**
| State | Allocation | Rationale |
|-------|-----------|-----------|
| Low Volatility (Δ VIX declining) | 100% SPY | Best regime for equities (SPY: 29.87% avg annual return, Sharpe 2.66 in this state) |
| High Volatility (Δ VIX rising) | 100% TLT | Defensive; bonds outperform equities in risk-off regimes (TLT: 25.21% avg annual return, Sharpe 1.27 in this state) |
| Gold (GLD) | 0% | GLD excluded — does not lead in either state |

**Verma et al. paper (3-state + RL):** Specific allocation rules NOT REPORTED (HTTP 403
blocked). ANALYSIS: With a third "transitional" state, a partial allocation blending SPY,
TLT, and GLD is likely applied during transitional periods.

### Step 3 — Execution

- 1-day execution lag implemented to prevent lookahead bias
- Daily rebalancing: full capital redeployment when state changes
- Transaction costs: NOT MODELED in either source (practitioner estimates 50–200 bps annually)

## Economic Rationale

Volatility regimes in financial markets are persistent — this is one of the most robust
stylized facts in financial economics (Engle 1982 ARCH; Hamilton 1989 HMM on business cycles).
The key relationships that motivate this strategy:

1. **Low-vol regimes → Equity outperformance:** In stable, low-VIX environments, equities
   compound without major drawdowns. Regime persistence means once identified, low-vol states
   tend to continue for days to weeks.
2. **High-vol regimes → Flight to safety:** During market stress, the standard negative
   equity-bond correlation (stocks fall → bonds rise) creates a natural hedge in long-duration
   Treasuries (TLT).
3. **VIX as a regime signal:** Daily Δ VIX captures regime transitions earlier than price-based
   signals alone, because VIX responds to options market information about forward-looking
   uncertainty.

**When the edge should disappear (falsifiability):**
- When equity-bond correlation turns positive (as in 2022, when inflation-driven rate hikes
  caused both equities AND long-duration bonds to fall simultaneously — TLT lost ~30% in 2022
  while equities fell ~20%); a VIX-based model would have correctly identified "high volatility"
  and rotated into TLT — which was the WRONG move in an inflationary bear market
- When VIX spikes happen faster than the HMM's 1-day detection lag allows reaction (e.g.,
  sharp intraday crashes, overnight gap-downs)
- When regime transitions become more frequent (reducing the benefit of high state persistence)
- When TLT loses its negative-equity-correlation property during structurally high inflation

The 2022 scenario is the critical failure mode. An inflationary regime creates a bond-equity
correlation reversal not captured by volatility-based regime detection alone.

## Entry Conditions

- Enter SPY: on close of day when HMM state = Low Volatility (Δ VIX regime negative)
- Enter TLT: on close of day when HMM state = High Volatility (Δ VIX regime positive)
- 1-day execution lag applied (signal at T, execution at T+1 open or close)
- Full capital redeployment (binary switching, not gradual)

## Exit Conditions

- Exit current position when HMM flips to the opposite state (daily assessment)
- Daily rebalancing: position held until state changes

## Risk Management

NOT REPORTED from the paper (HTTP 403 blocked). From the practitioner implementation:
- No explicit stop-loss
- No maximum single-day loss limit
- Risk control is entirely via regime detection: the model switches to TLT when stress detected
- Portfolio is fully invested at all times (no cash allocation)

ANALYSIS: This is a weak risk management framework. During the period between signal (rising VIX)
and execution (next day close), a fast-moving crash can produce substantial losses before
the portfolio switches to TLT. In March 2020, VIX went from 30 to 82 in roughly 3 weeks;
a regime model with 1-day lag would have incurred 10–20% losses before the protective
switch completed. Additionally, in 2022, the "protective" TLT allocation would have been
the source of losses rather than the protection.

## Indicators Used

- **VIX daily change (Δ VIX)** — primary regime signal
- **Gaussian HMM** (Baum-Welch EM, Viterbi decoding) — regime classifier
- **BIC** — model selection criterion (2-state vs. 3-state)
- For the Verma et al. paper: RL policy (algorithm NOT REPORTED) trained on HMM regime signal

## Transaction Costs & Capacity

**Transaction costs:** NOT MODELED in either source.
- Practitioner implementation explicitly acknowledges 50–200 bps annual impact estimate
- Daily rebalancing generates high turnover: regime switches roughly every 2–10 days on average
  (implied by state frequency and persistence: low-vol 96.0% persistence → mean stay ≈ 25 days;
  high-vol 88.3% persistence → mean stay ≈ 8.5 days)
- ETF transaction costs are lower than futures (no roll yield, ~0.01–0.05% bid-ask spread for
  liquid ETFs like SPY and TLT); however, commissions and slippage on frequent full-portfolio
  switches add up

ANALYSIS: Estimated annual cost drag at 50–200 bps (practitioner's own estimate). If true
Sharpe pre-cost is 1.22, a 150 bps annual drag on 14.27% volatility reduces Sharpe by
approximately 0.105 → net Sharpe ≈ 1.11. At 200 bps drag → net Sharpe ≈ 1.08. These are
still above the SPY benchmark Sharpe (0.463), but the gap narrows meaningfully.

**Capacity:** ETF trading in SPY, TLT, GLD can absorb large AUM (all are among the most liquid
ETFs globally). Capacity is not a constraint for this strategy.

## Backtest Evidence

**Practitioner 2-state implementation (CLAIMED, UNVERIFIED from open code):**
- 5,323 trading days from November 2004 to January 2026
- **Full-sample HMM parameter estimation** — the practitioner explicitly acknowledges
  "mild in-sample optimism" from this approach; rolling window reestimation not performed

**Verma et al. paper (3-state + RL):** Specific backtest evidence NOT REPORTED (HTTP 403).
Data period stated as 2004–2025 daily.

## Forward-Test Evidence

NOT REPORTED in either source.

## Performance Metrics

**Source note:** All metrics below are from the **practitioner's independent open-code
implementation** (2-state HMM, no RL), NOT from the Verma et al. paper. The paper's
specific metrics are NOT REPORTED due to HTTP 403 blockade. The practitioner explicitly
acknowledges full-sample estimation bias (in-sample optimism).

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 1.220 | CLAIMED, UNVERIFIED (in-sample optimism acknowledged; practitioner implementation, not paper) | GitHub: I-am-Uchenna/regime-allocation-strategy |
| Sortino Ratio | 1.672 | CLAIMED, UNVERIFIED | GitHub: I-am-Uchenna/regime-allocation-strategy |
| Calmar Ratio | 0.993 | CLAIMED, UNVERIFIED | GitHub: I-am-Uchenna/regime-allocation-strategy |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | −19.54% | CLAIMED, UNVERIFIED (full-sample HMM; acknowledged in-sample optimism) | GitHub: I-am-Uchenna/regime-allocation-strategy |
| CAGR | 19.41% | CLAIMED, UNVERIFIED | GitHub: I-am-Uchenna/regime-allocation-strategy |
| Annual Volatility | 14.27% | CLAIMED, UNVERIFIED | GitHub: I-am-Uchenna/regime-allocation-strategy |
| Annual Return | 19.41% (21-year average Nov 2004–Jan 2026) | CLAIMED, UNVERIFIED | GitHub |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | ~8–25 days (ANALYSIS from state persistence) | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% (always in SPY or TLT) | NOT REPORTED | — |
| Benchmark: SPY Sharpe | 0.463 | CLAIMED, UNVERIFIED | GitHub (same period) |
| Benchmark: SPY CAGR | 10.80% | CLAIMED, UNVERIFIED | GitHub (same period) |
| Benchmark: SPY Max DD | −55.19% | CLAIMED, UNVERIFIED | GitHub (same period) |
| Benchmark: Equal-Weight Sharpe | 0.660 | CLAIMED, UNVERIFIED | GitHub (same period) |

**Internal consistency check (ANALYSIS):** CAGR 19.41% / Volatility 14.27% → implied Sharpe ≈
(0.1941 − risk-free) / 0.1427. At 2.6% risk-free rate (reasonable for 2004–2026 average):
(0.1941 − 0.026) / 0.1427 ≈ 1.18. Stated Sharpe 1.220 implies ~2.2% risk-free rate. ✓ Consistent.
Calmar check: CAGR / Max DD = 19.41% / 19.54% = 0.993. ✓ Consistent.
No automatic scrutiny triggers: Sharpe 1.22 < 3 threshold; Max DD −19.54% > −5% threshold;
CAGR 19.41% < 50% threshold.

## Community Sentiment

The concept has substantial academic precedent (Hamilton 1989, Turner et al. 1989, Ang/Timmermann
2012). This specific paper is new (arXiv May 2026) with no community discussion yet.

The practitioner GitHub repo represents an independent reimplementation that:
- Validates the core mechanism (HMM on VIX → multi-asset rotation outperforms buy-and-hold)
- But also explicitly surfaces the key limitation: full-sample HMM = in-sample optimism
- Recommends rolling window reestimation as future work — not yet done

## Why This Might Be Nothing

**1. Full-sample HMM estimation = in-sample optimism.** The HMM parameters (transition
probabilities, emission means/covariances) are estimated on the ENTIRE 21-year dataset
(2004–2025), then the same model is used to "backtest" over that same period. This is the
classic in-sample overfitting problem. The model already "knows" about the 2008 GFC, the
2020 COVID crash, and the 2022 inflation shock when estimating its parameters. In a true
walk-forward reestimation, the model would frequently misclassify regimes during transitions
because its parameters were estimated without knowledge of subsequent events. The practitioner
explicitly acknowledges this: "HMM parameters estimated on full sample creates mild in-sample
optimism." This is not "mild" — it is the primary structural flaw.

**2. The 2022 inflationary regime breaks the core assumption.** The strategy's defensive leg
(TLT in high-vol regimes) is predicated on the negative equity-bond correlation that has
prevailed since the 1990s. In 2022, this correlation turned strongly positive: the Fed
hiked rates aggressively, causing TLT to lose ~30% (one of its worst years in history)
while equities also fell ~20%. A VIX-based model detecting "high volatility" → "move to TLT"
would have incurred large losses in the defensive asset. If the 21-year backtest includes 2022
(it does: Nov 2004–Jan 2026), the full-sample HMM already "knows" about 2022 and may have
learned to stay in SPY longer or use GLD during that period — which is information unavailable
at the time of the 2022 trading decisions in real-time.

**3. Transaction costs not modeled.** Daily full-portfolio switches between SPY and TLT
generate meaningful transaction costs. The practitioner's own estimate is 50–200 bps annually.
Even at the low end (50 bps), this materially compresses the net Sharpe ratio over 21 years.

**4. The RL component (from the paper) is a black box.** The Verma et al. paper adds an RL
policy on top of the HMM, but the RL algorithm, reward function, training methodology, and
OOS testing procedure are not accessible. RL systems are known to overfit to their training
environment, and without explicit OOS testing, the RL policy's contribution is unverifiable.

**5. What would change this assessment:** A rolling-window walk-forward implementation with
rolling HMM reestimation (e.g., 5-year expanding window), explicit transaction cost modeling,
and out-of-sample performance reports including the 2022 period without look-ahead. If the
strategy's Sharpe remains above 0.8 after these adjustments, it would represent a genuine edge
for a TAA strategy.

## Strengths / Weaknesses

**Strengths:**
- Economic rationale is well-established: volatility regime persistence is one of the most
  robust findings in financial economics
- Open-source practitioner code enables independent inspection (though uses simpler 2-state model)
- 21-year data period covers multiple distinct market regimes (dot-com bust, GFC, QE, COVID, inflation)
- ETF-based implementation has no capacity constraints and low execution complexity
- Drawdown reduction from -55% (SPY) to -20% (regime strategy) is practically significant

**Weaknesses:**
- Full-sample HMM estimation creates in-sample optimism; no rolling/walk-forward shown
- No transaction cost modeling
- 2022 bond-equity correlation break is the strategy's Achilles heel — not addressed
- Binary allocation (100% SPY or 100% TLT) is naive — no intermediate sizing
- Paper's RL component is inaccessible; no OOS results from the paper
- GLD excluded despite including it in the asset universe

## Confidence Score

| Dimension | Weight | Score (0–10) | Product | Notes |
|-----------|--------|--------------|---------|-------|
| Logical/economic soundness (falsifiable) | 3 | 7 | 21 | Mechanism well-grounded; failure conditions named (2022 bond-equity correlation break, regime lag); passes falsifiability test |
| Transparency & reproducibility | 2 | 5 | 10 | Related open code (practitioner's 2-state implementation) accessible and readable; paper rules partially described; RL details opaque |
| Evidence auditability | 2 | 4 | 8 | Preprint (arXiv/SSRN), not peer-reviewed; practitioner open code validates 2-state variant but uses different (simpler) model than paper; full-sample HMM acknowledged |
| Risk management quality | 2 | 4 | 8 | Regime-conditioned rotation is a form of dynamic risk management; no explicit stop-loss; no cash buffer; 1-day lag is vulnerable to fast crashes |
| Cost & capacity realism | 2 | 3 | 6 | Costs explicitly not modeled; practitioner estimates 50–200 bps annual drag; capacity is not a constraint (ETFs); costs noted as limitation |
| Honest treatment of drawdowns / failure | 1.5 | 5 | 7.5 | Max DD −19.54% reported; practitioner explicitly acknowledges full-sample in-sample bias; 2022 correlation break not addressed in available sources |
| Robustness (OOS / walk-forward / multi-market) | 1.5 | 2 | 3 | 21-year data is long, but full-sample HMM = not truly OOS; no rolling window reestimation; no multi-market validation; single 3-asset universe |
| Survived independent scrutiny | 1 | 3 | 3 | Practitioner independently reimplemented and found similar results + named limitations; no peer review; no academic community discussion yet |

**Weighted sum:** 21 + 10 + 8 + 8 + 6 + 7.5 + 3 + 3 = 66.5
**Max possible:** 15 × 10 = 150
**Latent score:** round(66.5/150 × 100) = **44**
**Evidence auditability = 4 → cap at 59**
**Capped confidence: min(44, 59) = 44**

`latent 44 (capped to 44 pending verification: arXiv preprint only; full-sample HMM acknowledged in-sample optimism; no OOS results; RL component inaccessible; 2022 bond-equity correlation break unaddressed)`

**Band: 44 — EXPERIMENTAL**

## Complexity / Scalability / Automation Feasibility

- **Complexity:** Low-medium — Gaussian HMM fitting is a well-solved problem in Python
  (`hmmlearn` library); Viterbi decoding is standard; RL adds complexity
- **Scalability:** Excellent — ETFs (SPY, TLT, GLD) accommodate very large AUM
- **Automation:** Feasible — daily VIX data, EOD rebalancing; straightforward Python pipeline
  with `hmmlearn`, `pandas`, and a brokerage API

## MQL5 / Python / Pine Feasibility

- **Python:** Highly feasible — `hmmlearn` + `pandas` + IBKR/Alpaca API; practitioner's open
  code is a working template (2-state version); 3-state HMM + RL requires additional `stable-baselines3`
- **Pine Script:** Feasible for the signal generation (VIX change → regime classifier can be
  approximated), but TradingView cannot execute ETF switching; signal generation only
- **MQL5:** Feasible for SPY/TLT/GLD as CFDs, but not native to MT4/MT5's primary FX focus;
  HMM library would need to be custom-coded or ported

## Similar Strategies

- `regime-switching-jump-model-equity` — same regime-detection theme but different mechanism
  (Statistical Jump Model with jump penalty λ on equity index returns → binary 0/1 equity
  timing); peer-reviewed (Journal of Asset Management 2024); open code (`jumpmodels` PyPI).
  Key differences: (1) equity-only timing vs. multi-asset SPY/TLT/GLD rotation; (2) jump
  model on returns vs. HMM on VIX changes; (3) JM paper is peer-reviewed, HMM+RL is preprint.
  Prefer `regime-switching-jump-model-equity` for single-asset regime timing.
- `intraday-momentum-spy` — also uses SPY but purely equity-based, no regime detection

## Red Flags

- **Full-sample HMM = in-sample optimism** (explicitly acknowledged by independent implementer)
- **No transaction cost modeling** (estimated 50–200 bps annual impact)
- **2022 bond-equity correlation break** — the core defensive assumption (TLT protects during stress)
  failed catastrophically in 2022; not addressed in accessible sources
- **RL black box** — paper's RL component is inaccessible; overfitting risk in RL is well-documented
- **Preprint only** — not peer-reviewed; no professional validation

## Source Links

| Source | Retrieved | Notes |
|--------|-----------|-------|
| [arXiv:2605.27848](https://arxiv.org/abs/2605.27848) | 2026-05-29 | Primary paper; HTTP 403 blocked |
| [SSRN:5785443](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5785443) | 2026-05-29 | SSRN version; HTTP 403 blocked |
| [GitHub: I-am-Uchenna/regime-allocation-strategy](https://github.com/I-am-Uchenna/regime-allocation-strategy) | 2026-05-29 | Independent practitioner implementation (2-state HMM, no RL); ACCESSIBLE; specific metrics obtained |
| [Medium: Ejike Uchenna Splendor](https://medium.com/@Splendor001/regime-based-portfolio-allocation-a-hidden-markov-model-approach-to-tactical-asset-rotation-4ff3fdf6f9f8) | 2026-05-29 | Related practitioner writeup; HTTP 403 blocked |

Primary paper sources all returned HTTP 403. Metrics from practitioner's open GitHub code.

## Analyst Notes

**FACTS (from sources):**
- arXiv:2605.27848 — Verma, Putri, Lesupi (May 27, 2026); 3-state HMM + RL; SPY/TLT/GLD;
  daily data 2004–2025; both HMM and RL policies outperform SPY; RL achieves highest Sharpe
- SSRN:5785443 — same paper (Nov 2025 version); SSRN submission date Nov 15, 2025
- GitHub (I-am-Uchenna): 2-state Gaussian HMM; VIX changes; 100% SPY (low vol) / 100% TLT (high vol);
  Sharpe 1.220, CAGR 19.41%, Max DD −19.54%, Sortino 1.672, Calmar 0.993, Volatility 14.27%
  (Nov 2004–Jan 2026); full-sample HMM acknowledged; costs not modeled; GLD excluded

**ANALYSIS (my reasoning):**
- The 2022 inflation regime is the critical failure case: VIX rising → model moves to TLT →
  TLT loses ~30% in 2022. If this is not explicitly addressed in the paper, the claimed Sharpe
  may reflect the model learning (in-sample) to handle 2022 by staying in SPY longer than
  the naive state-switch rule would dictate
- CAGR 19.41% with 14.27% vol → Sharpe 1.22 at ~2.2% risk-free rate: internally consistent ✓
- Rolling-window reestimation would likely reduce Sharpe to 0.7–0.9 after realistic costs
  (ASSUMPTION based on typical regime model degradation from in-sample to OOS)

**ASSUMPTIONS (flagged):**
- ASSUMED: The practitioner's 2-state implementation is representative of the paper's mechanism
  even though it uses a simpler model (2 states vs. 3, no RL)
- ASSUMED: The Verma et al. paper's RL component adds value over the base HMM (as claimed)
- ASSUMED: Rolling-window OOS would produce Sharpe 0.7–0.9 vs. in-sample 1.22 (not confirmed)

## Future Research Needed

1. Implement rolling-window HMM reestimation (e.g., 5-year expanding window) and measure
   true OOS performance, especially during 2022 when bond-equity correlation reversed
2. Model transaction costs explicitly: how much does daily rebalancing compress net returns?
3. Obtain full Verma et al. paper to understand: RL algorithm, reward function, OOS testing
   protocol, and whether the 3-state model outperforms the 2-state practitioner implementation
4. Test whether adding a "transitional" third state avoids TLT exposure during inflationary
   high-vol regimes (2022) — perhaps routing transitional states to GLD or cash
5. Compare to `regime-switching-jump-model-equity` on the same asset universe
