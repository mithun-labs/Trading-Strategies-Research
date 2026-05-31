# FX Carry + Value + Momentum Strategies over Their 200+ Year History

## Overview

This paper examines three canonical FX investment factors — carry, momentum (trend), and value (reversal) — over an extended 200+ year dataset going back to 1788. The central question is whether modern FX anomalies are statistically robust to long-run history or whether they are artefacts of the post-Bretton Woods sample most studies use. The main finding: **carry is robust across two centuries; momentum and reversal are period-specific.**

An additional strategy — cross-currency yield-curve flattening (long currencies with steep yield curves, short currencies with flat/inverted yield curves) — is documented as the most robust across time.

- **Author:** Joseph Chen
- **Source:** SSRN working paper (revised 2024); Quantpedia blog summary (Dec 2024)
- **Source Links:** [Quantpedia blog](https://quantpedia.com/fx-carry-value-momentum-strategies-over-their-200-year-history/) · [Quantpedia Medium](https://quantpedia.medium.com/fx-carry-value-momentum-strategies-over-their-200-year-history-a2c13cb5af1e)
- **Publication Status:** SSRN working paper — NOT peer-reviewed

---

## Asset Class / Timeframes

- **Markets:** G10 / developed market currencies (USD, EUR/DEM, GBP, JPY, CHF, AUD, CAD, NOK, SEK, NZD); expanding cross-section back to 1788
- **Timeframe:** Monthly rebalancing (inferred from typical carry/momentum literature)
- **Data period:** 1788–present (full sample); post-1973 (Bretton Woods breakdown) as sub-sample

---

## Core Logic

Four strategies are examined:

1. **Carry (short rates):** Rank currencies by overnight/short-term interest rate differential vs. USD. Long top quintile (high-yield), short bottom quintile (low-yield). Roll monthly.
2. **Carry (long bonds):** Same rank/execution but using long-term government bond yield as the signal rather than short rate.
3. **Momentum:** Rank currencies by prior 1-month return. Long top quintile (past winners), short bottom quintile (past losers). 1-month holding period.
4. **Value/Reversal:** Rank currencies by PPP deviation or recent multi-year appreciation/depreciation. Long "cheap" (depreciated), short "expensive" (appreciated).
5. **Cross-currency yield-curve flattening:** Long bonds in currencies with steep yield curves (high term premium); short bonds in currencies with flat/inverted curves. Documented as surprisingly robust.

---

## Economic Rationale

- **Carry:** The forward premium puzzle — currencies with high interest rates systematically outperform low-rate currencies, violating uncovered interest parity (UIP). This premium compensates for crash risk (sudden unwinds during global risk-off events). The rationale is falsifiable: carry should underperform during global liquidity crises / flight-to-quality episodes (e.g. 2008 GFC, 1990s EM crises, COVID March 2020).
- **Momentum:** Behavioral under-reaction story: investors are slow to update, so winners keep winning. Should fail if markets become more informationally efficient or trading costs drop sufficiently (limits to arbitrage erode).
- **Value:** PPP convergence over long horizons. Should fail when structural changes permanently alter equilibrium exchange rates, or during dominant monetary policy divergence episodes.
- **Yield-curve flattening:** Flight-to-safety mechanics; flat/inverted yield curves signal monetary tightening, reducing return on holding that currency's bonds.

The paper provides a critical falsifiability test for momentum and value: **both fail in the full 200-year sample**, which is the finding that renders the underlying edge suspect. Per the HARD RULE, a rationale that fails its own long-run test is treated as period-specific, not an economic edge.

---

## Entry Conditions

**Carry (short rates):**
- At month-end, rank all available currencies by short-term interest rate differential vs. USD
- Go long top quintile (highest yield above USD); go short bottom quintile (lowest yield)
- Equal-weight within each leg

**Carry (long bonds):**
- Same procedure; signal = long bond yield instead of short rate

**Momentum (1-1 strategy):**
- Rank currencies by 1-month prior return (in USD terms)
- Long top quintile; short bottom quintile; hold 1 month

**Yield-curve flattening:**
- Rank currencies by term spread (long bond yield minus short rate)
- Long currencies with steep curves; short currencies with flat/inverted curves
- Hold bonds (not spot FX) of the respective currencies

---

## Exit Conditions

- Monthly rebalancing; exit when rank drops out of quintile
- No explicit stop-loss described; carry strategies typically hold to the next rebalancing date

---

## Risk Management

- Long-short portfolio structure provides implicit market-neutral exposure within FX
- No explicit stop-losses or position limits described
- Carry crash risk is documented but not explicitly filtered in this historical study
- Cross-currency yield-curve flattening is the most robust strategy and least subject to crash events per the paper

---

## Indicators Used

| Indicator | Signal Type | Data Needed |
|-----------|-------------|-------------|
| Short-term interest rate | Carry signal | Central bank rate / overnight rate |
| Long-term bond yield | Carry (alt.) | Government bond yield (10yr) |
| 1-month currency return | Momentum signal | Monthly spot + interest rates |
| PPP deviation / long-run return | Value signal | Monthly spot + CPI indices |
| Yield curve slope (spread) | Flattening signal | Both short and long rate |

---

## Transaction Costs & Capacity

- **Costs modeled:** NOT REPORTED for the 200-year historical study; historical bid-ask data before 1950 is unavailable
- **Modern carry costs:** Monthly rebalancing; bid-ask spread ~2–10bps per G10 pair for institutional; manageable
- **Modern momentum costs:** Monthly 1-1 rebalancing; similar order of magnitude; Menkhoff et al. (2012) find costs significantly reduce momentum profits, particularly in less-liquid currencies
- **Capacity:** G10 carry is extremely well-studied and likely capacity-constrained at institutional scale (crowding documented pre-2008); carry unwind events are partly caused by crowding
- **Verdict:** Costs not modeled for 200-year study; modern G10 carry likely survives monthly-rebalancing costs; momentum sensitivity to costs is high (per Menkhoff et al.)

---

## Backtest Evidence

- 200+ year in-sample backtest; no distinct OOS period (the entire 200 years is the sample)
- Sub-period analysis shows carry robust pre- and post-Bretton Woods; momentum only works post-1970s
- CLAIMED, UNVERIFIED — SSRN working paper, no peer review, sources HTTP 403

---

## Forward-Test Evidence

- NOT REPORTED

---

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | 0.391 (carry / short rates, 200yr) | CLAIMED, UNVERIFIED | Chen SSRN (via Quantpedia search snippet) |
| Sharpe Ratio | 0.361 (carry / long bonds, 200yr) | CLAIMED, UNVERIFIED | Chen SSRN (via Quantpedia search snippet) |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | NOT REPORTED | NOT REPORTED | — |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED | NOT REPORTED | — |
| Average Trade Duration | ~1 month (inferred from rebalancing) | NOT REPORTED | — |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | NOT REPORTED | NOT REPORTED | — |
| Exposure Percentage | ~100% (always invested) | NOT REPORTED | — |

---

## Community Sentiment

- Carry trade as an FX strategy is one of the most well-studied anomalies in academic finance (hundreds of papers since the 1980s)
- The specific 200-year angle is novel; no independent critical review found
- Quantpedia covers this positively as a long-run validation
- Broader academic consensus: carry works in modern samples but with significant crash risk and crowding concerns; momentum in FX is more contested (partially explained by costs)

---

## Why This Might Be Nothing

### 1. Most likely benign explanation for carry edge

The carry Sharpe ratios of 0.39 and 0.36 over 200 years are **modest** — broadly consistent with crash-risk compensation, not unexploited alpha. Approximately 1/3 of carry excess returns is documented as crash risk premium (Brunnermeier/Nagel/Pedersen 2008). This means the carry "edge" may not represent exploitable alpha — it's compensation for bearing tail risk that occasionally wipes out years of gains.

**ANALYSIS:** The post-GFC period (2008–2015) shows G10 carry Sharpe collapsing from ~1.08 to ~0.25 (per separate research). The 200-year average of 0.39 may flatter the strategy's current exploitability.

### 2. Momentum and reversal failure are the key findings

The most important result is what the paper does NOT support: momentum only works in the latter half of the 200-year sample (from roughly the 1970s onward), and value/reversal only works post-Bretton Woods. These are large-sample falsifications. **Both strategies fail the full-history test, which is precisely the test that should discriminate true edges from data-mined anomalies.**

For momentum specifically: the Menkhoff et al. (2012) finding that transaction costs significantly reduce profitability — especially in less-liquid currencies — means the post-1970s momentum effect may largely be a cost-drag artefact that only appears significant when bid-ask spreads are ignored.

### 3. No cost modeling for historical data

The 200-year backtest cannot realistically model transaction costs before modern FX markets developed (post-1971). Bid-ask spreads, settlement costs, and market access conditions before the 1970s are unrecoverable. Any pre-modern carry return is a theoretical exercise, not a tradeable result.

### 4. Yield-curve flattening strategy: rules not codeable

The cross-currency yield-curve flattening strategy is described as "most robust" but the specific portfolio construction rules (which bond maturity, which position sizing, rebalancing frequency) are paywalled/inaccessible.

### 5. Data availability and survivorship bias

200-year datasets necessarily exclude currencies that collapsed (hyperinflation, default, war destruction). Long-run carry results in collapsed currencies would be extreme losses that are not in the data. This is a form of survivorship bias that systematically overstates historical carry returns.

### 6. The one piece of evidence that would change my mind

An independent peer-reviewed replication of the 200-year carry dataset with costs modeled for modern sub-periods, showing carry survives after costs and crash-risk adjustment, AND showing that momentum fails to survive the same 200-year test with costs. I do not have this.

---

## Strengths / Weaknesses

**Strengths:**
- 200+ years is an extraordinary robustness test — most FX papers use 30–40 years
- Explicitly shows that momentum and value FAIL the long-run test — intellectually honest
- Carry rationale is falsifiable and documented crash risk gives a clear failure condition
- Cross-currency yield-curve flattening is a novel finding worth investigating further

**Weaknesses:**
- No cost modeling for historical backtest
- SSRN working paper only — not peer-reviewed
- No specific implementation rules for the yield-curve flattening strategy (paywalled/inaccessible)
- The carry edge (Sharpe 0.39) is modest and largely crash-risk compensation
- Post-GFC carry performance materially weaker than historical average

---

## Confidence Score

**Scoring:**

| Dimension | Score (0-10) | Wt | Weighted |
|-----------|-------------|-----|----------|
| Logical/economic soundness (falsifiable) | 6 | 3 | 18 |
| Transparency & reproducibility | 5 | 2 | 10 |
| Evidence auditability | 4 | 2 | 8 |
| Risk management quality | 3 | 2 | 6 |
| Cost & capacity realism | 3 | 2 | 6 |
| Honest treatment of drawdowns/failure | 8 | 1.5 | 12 |
| Robustness evidence (OOS/walk-forward) | 8 | 1.5 | 12 |
| Survived independent scrutiny | 4 | 1 | 4 |
| **Totals** | | 15 | **76** |

`latent_score = round(76 / 150 × 100) = 51`

**Verification cap:** Evidence auditability = 4 → cap at 59.
`confidence = min(51, 59) = **51**`

**latent 51 (capped to 51: SSRN working paper only, not peer-reviewed; all direct sources HTTP 403)**

**Band: Experimental (40–59)**

---

## Complexity / Scalability / Automation Feasibility

- Carry: LOW complexity — rank by rate differential monthly; freely available data (central bank rates); automatable in Python/QuantLib
- Momentum: LOW complexity — rank by 1-month return; automatable
- Yield-curve flattening: LOW-MEDIUM complexity — requires both short and long rate data; bond positioning adds complexity
- Capacity: Carry is well-known and institutional-scale trading is documented; G10 pairs are highly liquid; small retail account can implement but institutional carry crowding affects timing

---

## MQL5 / Python / Pine Feasibility

- **Python:** Straightforward — FRED, ECB, BIS data APIs provide rate data; pandas for rolling rank; monthly rebalancing
- **MQL5:** Feasible for spot FX pairs; interest rate data needs manual input or secondary feed
- **Pine Script:** Monthly carry rank is feasible; swap rate data availability is the limiting factor

---

## Similar Strategies

- `fx-mean-reversion-futures-monthly` — same asset class; mean-reversion signal on FX futures; score 46
- `london-breakout-forex` — same asset class; session breakout rather than carry/momentum

---

## Red Flags

- **No cost modeling for 200-year study:** Historical bid-ask data unavailable
- **Momentum fails full-history test:** The paper's own most important result is negative for momentum; suggests data-snooping
- **Carry Sharpe is modest (0.39 over 200yr):** Post-GFC period materially weaker; crowding risk documented
- **SSRN working paper only:** Not peer-reviewed; cannot verify methodology or statistics
- **All direct sources HTTP 403:** Full paper text inaccessible

---

## Source Links

| Source | URL | Retrieved |
|--------|-----|-----------|
| Quantpedia blog summary | https://quantpedia.com/fx-carry-value-momentum-strategies-over-their-200-year-history/ | 2026-05-31 (HTTP 403) |
| Quantpedia Medium summary | https://quantpedia.medium.com/fx-carry-value-momentum-strategies-over-their-200-year-history-a2c13cb5af1e | 2026-05-31 (HTTP 403) |
| YouTube explainer | https://www.youtube.com/watch?v=I5eoOICeex0 | 2026-05-31 (HTTP 403) |

---

## Analyst Notes

**FACTS (from sources):**
- Carry Sharpe ratios: 0.391 (short rates), 0.361 (long bonds) over 200+ year history — from Quantpedia/search snippets
- Momentum: positive returns only in latter half of 200-year sample when data starts at 1788
- Value/reversal: only exists within post-Bretton Woods period; no evidence of profitability in pre-1971 data
- Cross-currency yield-curve flattening described as "most robust" throughout history
- Author: Joseph Chen; revised 2024; available on SSRN
- Carry robust except for period surrounding World Wars

**ANALYSIS (reasoning):**
- Carry Sharpe of 0.39 is consistent with long-run CTA per-market Sharpe benchmarks (0.15–0.40 range); this is not exceptional alpha but rather crash-risk compensation
- The momentum failure over full 200-year history is a strong falsification signal — this is exactly the kind of robustness test that distinguishes true anomalies from data-mined period-specific effects
- Post-GFC G10 carry underperformance (Sharpe dropped from ~1.08 pre-GFC to ~0.25 post-GFC per other research) suggests the 200-year Sharpe of 0.39 overstates current exploitability
- The 200-year Sharpe advantage of carry over momentum and reversal is the paper's key contribution; it argues for carry as the most fundamental FX risk premium

**ASSUMPTIONS:**
- Monthly rebalancing assumed based on standard carry/momentum literature
- Quintile portfolio construction assumed based on standard long-short factor construction
- SSRN ID not confirmed from available search snippets

---

## Future Research Needed

1. Access the full SSRN paper to confirm specific implementation rules, portfolio construction details, and exact sub-period breakdowns
2. Independent cost-inclusive modern carry implementation (post-2010, G10 only) to verify whether the 200-year result holds in current market microstructure
3. Compare to the Menkhoff et al. (2012) currency momentum cross-sectional approach — where carry wins 200yr but momentum barely survives costs in 48-currency sample
4. Assess yield-curve flattening strategy (cross-currency) for implementability — needs specific bond maturity rules and sizing logic
