# ICT Silver Bullet — Forex Killzone Entry Model

## Overview

A time-restricted intraday entry model developed by Michael J. Huddleston ("ICT" /
Inner Circle Trader) and disseminated through his YouTube channel. The strategy enters
long or short during one of three one-hour "killzone" windows each trading day by
combining a **liquidity sweep**, a **Market Structure Shift (MSS)**, and a **Fair Value
Gap (FVG)** entry on low timeframes. Promoted as a high win rate (70–80%), minimum 1:2
R:R scalping approach for major forex pairs.

**Source type: YouTube / community write-ups — lowest trust tier per research protocol.**

## Asset Class / Timeframes

- **Primary markets:** Forex (EURUSD, GBPUSD primary; GBPJPY, AUDUSD secondary)
- **Also applied to:** XAUUSD, NAS100, US30 (claimed to transfer)
- **Execution timeframe:** 1-min, 3-min, or 5-min chart
- **Bias / context timeframe:** 15-min chart
- **Signal timeframe:** 1-min or 3-min for MSS and FVG detection

## Core Logic

The strategy operates exclusively during three one-hour "Silver Bullet" windows (all EST):

| Window | Times (EST) | Session Context |
|--------|------------|-----------------|
| Window 1 | 03:00 AM – 04:00 AM | London Open Killzone |
| Window 2 | 10:00 AM – 11:00 AM | New York AM Killzone |
| Window 3 | 02:00 PM – 03:00 PM | New York PM Killzone |

No trades are taken outside these windows. The rationale: institutional algorithmic
activity peaks during these periods, creating predictable liquidity sweeps.

**Sequence of events required for a valid setup:**

1. **Mark liquidity** on the 15-min chart before the session: identify buy-side
   liquidity (BSL = recent swing highs where stop-losses cluster above) and sell-side
   liquidity (SSL = recent swing lows where stops cluster below).
2. **Liquidity sweep occurs:** During the killzone window, price extends through
   the BSL (triggering long stops) or SSL (triggering short stops) then reverses.
3. **Market Structure Shift (MSS):** On the 1–3 min chart, after the sweep, price
   creates a **lower high** (bearish reversal = MSS bearish) or **higher low** (bullish
   reversal = MSS bullish). This confirms the sweep was institutional and directional.
4. **Fair Value Gap (FVG) forms:** After the MSS candle, a 3-candle imbalance appears —
   the wick of candle 1 and candle 3 do not overlap, leaving a "gap" in price delivery.
   For longs: bullish FVG (gap above a bullish candle sequence). For shorts: bearish FVG.
5. **Entry:** Price retraces into the FVG zone. Enter at or near the 50–70% level of
   the FVG (the "Optimal Trade Entry" or OTE, per ICT terminology).

## Economic Rationale

Institutional trading desks and algorithmic systems concentrate activity during session
overlaps and market opens — this is well-documented in market microstructure research.
Retail stop-loss orders cluster above recent highs and below recent lows. An institution
needing to fill a large order may benefit from an initial move that triggers retail stops
and provides liquidity for the real fill.

**The stated mechanism:** During killzones, institutions sweep liquidity (triggering
retail stops), then execute their real directional trade. The FVG is the price imbalance
created by the institutional entry. Retail traders follow institutions into the FVG.

**Falsifiability test:**
- Should FAIL outside the three killzone windows (testable: same setup outside hours should underperform)
- Should FAIL on days without clear buy/sell-side liquidity defined (news events, flat sessions)
- Should FAIL when the "sweep" is actually a genuine breakout, not a stop hunt

**Critical weakness in the rationale:** The "smart money manipulation" narrative is
partially unfalsifiable. If the setup fails, it can be attributed to "different institutional
intent" or "this wasn't a real sweep." This is a known failure mode for ICT-family strategies:
the conceptual framework can explain any outcome post-hoc, making it difficult to rule out.

## Entry Conditions

**Bearish Silver Bullet (short):**
1. In a killzone window
2. Price sweeps above a recent swing high (BSL sweep) on the 15-min chart
3. On 1–3 min chart: a lower high forms after the sweep (MSS bearish)
4. A bearish FVG appears on 1–3 min chart
5. Price retraces up into the FVG
6. Short entry at 50–70% of the FVG zone

**Bullish Silver Bullet (long):**
1. In a killzone window
2. Price sweeps below a recent swing low (SSL sweep) on the 15-min chart
3. On 1–3 min chart: a higher low forms after the sweep (MSS bullish)
4. A bullish FVG appears on 1–3 min chart
5. Price retraces down into the FVG
6. Long entry at 50–70% of the FVG zone

## Exit Conditions

- **Stop Loss:** Above the high of the FVG candle formation (short) or below the low
  of the FVG candle formation (long). Typically 5–15 pips on EURUSD 1-min chart.
- **Take Profit:** The nearest liquidity pool on the opposite side, or a prior FVG on
  the opposite side, or a 1:2 minimum R:R fixed target.
- **Time stop:** If price has not moved in the trader's favour within the killzone window,
  exit or move stop to break-even.

## Risk Management

- **Stop loss:** Specifically defined (FVG candle extreme)
- **R:R minimum:** 1:2 stated; some practitioners target 1:3
- **Position sizing:** NOT REPORTED in primary sources. Conventional 1–2% account risk
  per trade assumed by community practitioners.
- **Max trades per session:** Typically 1 trade per killzone window (first valid setup)
- **Max daily drawdown / account-level risk:** NOT REPORTED
- **Regime exit:** NOT REPORTED. No defined conditions for suspending the strategy.

## Indicators Used

- Rolling swing highs/lows on 15-min chart (liquidity identification — visual)
- Market Structure Shift (price action — no indicator; requires visual identification)
- Fair Value Gap (3-candle imbalance — detectable algorithmically; Python packages exist)
- No external indicators (moving averages, RSI, MACD not part of the core setup)

## Transaction Costs & Capacity

**Transaction costs:**
- EURUSD retail spread: 0.6–1.5 pips (variable by broker/session)
- Target: 20–30 pips → spread represents ~2–7% of gross profit per trade (manageable)
- Commission: ~$5–8 round trip per $100K notional (ECN/STP broker)
- Slippage: minimal on EURUSD/GBPUSD at typical retail sizes ($10K–$500K)
- Swap: intraday only (no overnight hold) → no swap cost
- **Conclusion:** Transaction cost structure is not strategy-killing for EURUSD/GBPUSD at
  1-min/3-min timeframes with 20–30 pip targets. However, costs are NOT explicitly modeled
  in any accessible community backtest.

**Capacity:** EURUSD/GBPUSD can absorb retail sizes without slippage impact. At
institutional sizes, the setup logic (following institutions) becomes self-contradictory —
if the trade size is large enough to move the market, the setup premise breaks.

## Backtest Evidence

CLAIMED, UNVERIFIED — all performance data comes from YouTube videos, community posts,
and practitioner write-ups. No independent academic or auditable study found.

**Community backtest (Threads.com, @iambriancharles):**
- Market: EURUSD
- Duration: 10 trading days
- Win rate: 62.5% (5/8 trades)
- Profit: +24.71R → $6,179.09 on $25,000 account (implies ~1% risk per trade)
- Source: Claimed, Unverified; 10 days is far too short for statistical significance

**YouTube 5-year backtest playlist:**
- Creator: unnamed YouTube channel
- Available at: `youtube.com/playlist?list=PLrdp4pxxUFnx9230uH8hQQaFQHgmd3upD`
- Contents: NOT ACCESSIBLE in this research session
- 5 years of data theoretically provides statistical power, but video backtests are
  susceptible to confirmation bias (setups that fail may not be included)

**Consensus from community sources:**
- "Selectively profitable" — some months exceptional, others disastrous
- July 2023 NAS100: described as exceptional; other months of 2023 described as poor
- Pre-2023 results "vary widely" from post-2023 results — possible regime dependency
- Success varies by individual trader (discretionary MSS identification)

## Forward-Test Evidence

NOT REPORTED. ICT (Michael Huddleston) himself:
- Failed his own 2016 public challenge ($10,000 → $1,000,000 goal: account blown)
- Blew his account at the 2024 Robbins World Cup of Trading

This is the most material piece of contradicting evidence available: the strategy's
creator cannot demonstrate it works in live, audited, public competition conditions.

## Performance Metrics

| Metric | Value | Status | Source |
|--------|-------|--------|--------|
| Sharpe Ratio | NOT REPORTED | NOT REPORTED | — |
| Sortino Ratio | NOT REPORTED | NOT REPORTED | — |
| Calmar Ratio | NOT REPORTED | NOT REPORTED | — |
| Profit Factor | NOT REPORTED | NOT REPORTED | — |
| Win Rate | 70–80% (promoted); 62.5% (one 10-day community backtest) | CLAIMED, UNVERIFIED | YouTube / community |
| Expectancy per Trade | NOT REPORTED | NOT REPORTED | — |
| Maximum Drawdown | NOT REPORTED | NOT REPORTED | — |
| CAGR | NOT REPORTED | NOT REPORTED | — |
| Annual Return | NOT REPORTED | NOT REPORTED | — |
| Number of Trades | NOT REPORTED (full-sample) | NOT REPORTED | — |
| Average Trade Duration | Intraday; <60 min per trade | CLAIMED, UNVERIFIED | Community description |
| Recovery Factor | NOT REPORTED | NOT REPORTED | — |
| Risk-Reward Ratio | 1:2 minimum stated | CLAIMED, UNVERIFIED | ICT / community |
| Exposure Percentage | Very low (3 hrs/day of eligibility) | NOT REPORTED | — |

**On the 70–80% win rate claim (mandatory scrutiny — near/at 80% threshold):**

A 70–80% win rate at 1:2 R:R would produce an expectancy of:
- At 70% WR: (0.70 × 2R) − (0.30 × 1R) = 1.4R − 0.3R = +1.1R per trade [ANALYSIS]
- At 80% WR: (0.80 × 2R) − (0.20 × 1R) = 1.6R − 0.2R = +1.4R per trade [ANALYSIS]

This is a very high expectancy for a discretionary scalping strategy. The only community
backtest found (62.5% WR over 10 days) is below the 70% lower bound of the claim. The
discrepancy and the subjective nature of setup identification (hindsight FVG marking is
easier than real-time) suggest the promoted win rate is likely inflated.

**FVG fill-rate as partial independent evidence:**
- One statistical study: 76% of EURUSD FVGs fill within 2 days (500-instance study,
  3 months). [CLAIMED, UNVERIFIED — source: search engine snippet]
- Edgeful data (not directly accessible): 66.3% win rate on long FVG trades, 1.5% avg
  return per trade. [CLAIMED, UNVERIFIED — snippet only; methodology unclear]
- These figures support that FVGs have *some* predictive value for price retracement, but
  this is not the same as the full Silver Bullet setup (which adds killzone timing, MSS,
  and liquidity sweep requirements).

## Community Sentiment

Highly **polarized** — one of the most controversial educators in retail forex.

**Supporters (largely on YouTube, TikTok, Discord):**
- Large following; hundreds of thousands of subscribers; active community
- Practitioners share live trade examples with positive results
- Community-built tools: multiple free TradingView indicators, MT4/MT5 indicators,
  Python packages (joshyattridge/smart-money-concepts, smtlab/smartmoneyconcepts)
- Some traders report consistent profitability using ICT concepts

**Critics (ForexPeaceArmy, Reddit r/Forex, YouTube):**
- ICT himself failed public trading challenges: 2016 ($10K→$1M) AND 2024 Robbins Cup
- ForexPeaceArmy: polarized reviews; multiple allegations of fraud/misrepresentation
- YouTube video titled "Michael Huddleston aka ICT - Trading's Biggest Fraud"
- Poll (post-mentorship): 92% of students reported still confused and feeling hopeless
- Zero confirmed profitable students with audited track records
- Comments disabled on most ICT YouTube videos (prevents scrutiny)
- Critics argue ICT rebrands standard price-action concepts with proprietary vocabulary
- Suspected alias: early online presence as "Bobi Abonacci" (unverified)

**ForexFactory Silver Bullet threads:**
- Multiple active threads (1256782, 1291560, 1343550): active community discussion
- Some practitioners claim consistent profitability; others report it doesn't work
- Accessible summaries show no consensus on profitability

## Why This Might Be Nothing

### 1. Hindsight selection bias (most likely explanation for the high win rate)

FVGs and liquidity sweeps are identified visually on historical charts. In hindsight,
it is always possible to find a "valid" setup before every profitable move. Real-time
setup identification is far harder: multiple FVGs may form during a killzone, and which
one to trade requires judgment calls that are easy to make correctly in retrospect but
difficult in real time. A 70–80% win rate on retrospectively-selected setups provides
very little information about forward-looking performance.

### 2. Creator cannot demonstrate the edge in live competition

Michael Huddleston failed his 2016 public $10K challenge AND blew his 2024 Robbins
World Cup account. These are the only audited, live-trading performance records available.
Both are negative. This is the single most important data point in this evaluation.

### 3. Regime dependency / 2023 optimism

Pre-2023 results "vary widely" from 2023 results. The strategy became massively popular
in 2022–2023. If the performance metrics driving the 70–80% win rate narrative are
primarily based on 2022–2023 data, they may reflect a regime-specific pattern rather
than a persistent edge. The 2022 "volatility regime" (high-impact Fed policy moves,
sharp intraday reversals) may be unusually favorable for sweep-and-reverse strategies.

### 4. The "Market Structure Shift" is inherently discretionary

On a 1-minute chart, every small consolidation produces a "higher low" or "lower high."
The MSS identification is a post-hoc rationalization applied to whichever candle pattern
preceded the profitable move. Without a fully mechanical definition of MSS, the strategy
cannot be backtested objectively — and the claimed win rates cannot be independently verified.

### 5. The cost case

At claimed 70%+ win rate with 1:2 R:R, the strategy would be enormously profitable even
after realistic broker costs on EURUSD. If it genuinely worked at this rate, it would be
arbitraged by algorithmic traders within months. The absence of documented profitable
live traders (despite millions of followers and years of publication) is the most
direct evidence that the win rate is inflated or the edge does not persist.

### 6. What would change my mind

A 12-month audited live trading account (verified by a third-party broker statement or
a reputable prop-firm payout record) showing 70%+ win rate on 200+ trades with 1:2+ R:R,
from a trader who can describe their setups mechanically, would significantly change this
assessment. No such record has been found.

## Strengths / Weaknesses

**Strengths:**
- Session-timing constraint is well-founded: institutional activity genuinely peaks during
  killzone hours; this is supported by market microstructure research
- Entry precision: SL inside FVG candle extreme is tight, enabling high R:R setups
- No overnight risk (intraday only)
- Simple instrument list (EURUSD, GBPUSD) with low spread and deep liquidity
- Large active community producing open-source tools (Python, TradingView, MT4)
- Strategy is free and fully documented on YouTube

**Weaknesses:**
- Creator has no verified live track record; failed two public competitions
- Win rate likely inflated by hindsight setup selection
- "Market Structure Shift" identification is subjective → hard to backtest mechanically
- "Selectively profitable": known regime dependency
- Pre-2023 results vary widely (possible overfitting to recent data)
- No independent academic study
- Community highly polarized; fraud allegations documented
- 92% student confusion post-mentorship undermines pedagogical effectiveness

## Confidence Score

**Dimension breakdown:**

| Dimension | Score | Weight | Weighted |
|-----------|-------|--------|----------|
| Logical/economic soundness (falsifiable) | 4 | 3 | 12 |
| Transparency & reproducibility | 5 | 2 | 10 |
| Evidence auditability | 2 | 2 | 4 |
| Risk management quality | 5 | 2 | 10 |
| Cost & capacity realism | 3 | 2 | 6 |
| Honest treatment of drawdowns/failure | 2 | 1.5 | 3 |
| Robustness (OOS/walk-forward) | 3 | 1.5 | 4.5 |
| Survived independent scrutiny | 2 | 1 | 2 |

**Total weighted: 51.5 / 150**
**Latent score: round(51.5/150 × 100) = 34**
**Evidence auditability = 2 (≤4) → Verification cap: 59**
**Confidence: min(34, 59) = 34 — LOW CONFIDENCE**

*latent 34 (capped to 34 pending verification: YouTube strategy; creator failed audited
public competitions; no independent academic study; hindsight selection bias structurally
embedded in setup identification; all performance claims CLAIMED, UNVERIFIED)*

**Rationale for Low Confidence despite high win rate claims:**
The verification cap would allow a score up to 59, but the latent score itself lands at 34
because: (1) creator's live failures directly contradict the edge claim; (2) MSS
identification is inherently discretionary, disqualifying objective reproducibility;
(3) regime dependency and pre-2023 variation undermine robustness; (4) no drawdown
data means risk-adjusted performance is unknowable; (5) 92% student confusion after
mentorship is strong base-rate evidence against the strategy having a systematic edge.

**Mandatory Scrutiny — Win Rate near 80% threshold:**
The promoted 70–80% win rate triggers mandatory discussion (near >80% threshold):
A win rate this high, sustained over hundreds of trades at 1:2 R:R, would yield
extraordinary risk-adjusted returns. The only audited data point (creator's live trading)
contradicts this — both public attempts resulted in blown/near-blown accounts. The most
plausible explanation is hindsight bias in setup labeling, not a genuine 70–80% forward-looking
win rate.

## Complexity / Scalability / Automation Feasibility

**Complexity:** Medium. Rules are defined but "Market Structure Shift" identification
on 1-min chart requires discretion in real time. The killzone timing and FVG detection
are fully mechanical.

**Automation feasibility:** Partial.
- FVG detection: automatable (Python packages exist; 3-candle pattern)
- Killzone timing: trivially automatable
- Liquidity sweep detection: automatable (price exceeds prior N-hour swing high/low)
- MSS identification: NOT reliably automatable without human judgment — this is the
  bottleneck. Multiple Python projects attempt it but results are subjective.

**Scalability:** Not scalable beyond retail sizes for the intended mechanism (following
institutional order flow). At $5M+ notional, the trader becomes the institution.

## MQL5 / Python / Pine Feasibility

- **Python:** Partially implementable. `joshyattridge/smart-money-concepts` and
  `smtlab/smartmoneyconcepts` PyPI packages provide FVG and swing high/low detection.
  Full mechanical Silver Bullet backtest would require additional MSS logic.
- **Pine Script (TradingView):** Multiple free community indicators exist for Silver
  Bullet timing windows and FVG detection. Visual strategy implementation is well-supported.
- **MQL5:** Free MT4/MT5 indicators exist (TFlab Silver Bullet indicator, per ForexFactory).
  Full EA automation is hampered by subjective MSS component.

## Similar Strategies

- `london-breakout-forex` — Also exploits London session timing; simpler rule set (Asian
  range breakout); score 47. Shares the session-timing economic rationale but without the
  "smart money" interpretation layer.

## Red Flags

- **Creator failed both audited public trading challenges (2016, 2024)** — contradicts edge claim
- Win rate near 80% mandatory scrutiny triggered: probable hindsight bias
- No verified profitable students with documented track records
- 92% post-mentorship student confusion (poll data)
- "Market Structure Shift" identification is discretionary → not mechanically backtestable
- Pre-2023 results "vary widely" — regime dependency
- Comments disabled on most YouTube videos — suppresses community scrutiny
- Alleged fraud (YouTube video, ForexPeaceArmy complaints, forum accusations)
- Multiple ICT concept rebranding allegations
- No independent academic study of the Silver Bullet setup specifically

## Source Links

| Source | Retrieval Date |
|--------|---------------|
| [ICT Silver Bullet — HowToTrade.com (HTTP 403)](https://howtotrade.com/trading-strategies/ict-silver-bullet/) | 2026-05-31 |
| [ICT Silver Bullet PDF — HowToTrade.com (HTTP 403)](https://howtotrade.com/wp-content/uploads/2024/04/ICT-Silver-Bullet-Trading-Strategy.pdf) | 2026-05-31 |
| [innercircletrader.net Silver Bullet guide (HTTP 403)](https://innercircletrader.net/tutorials/ict-silver-bullet-strategy/) | 2026-05-31 |
| [ICT Silver Bullet — EBC Financial Group (HTTP 403)](https://www.ebc.com/forex/what-is-the-ict-silver-bullet-meaning-rules-and-examples) | 2026-05-31 |
| [ICT Silver Bullet — FXOpen (HTTP 403)](https://fxopen.com/blog/en/what-is-the-ict-silver-bullet-strategy-and-how-does-it-work/) | 2026-05-31 |
| [ICT Silver Bullet — LuxAlgo](https://www.luxalgo.com/blog/ict-silver-bullet-setup-trading-methods/) | 2026-05-31 |
| [ICT Kill Zones — Zaye Capital (search snippet)](https://zayecapitalmarkets.com/kill-zone-in-ict-trading/) | 2026-05-31 |
| [High Win Rate ICT Silver Bullet — ForexFactory (HTTP 403)](https://www.forexfactory.com/thread/1256782-high-win-rate-ict-silver-bullet-advanced) | 2026-05-31 |
| [ICT Silver Bullet 5-Year Backtest Playlist — YouTube](https://www.youtube.com/playlist?list=PLrdp4pxxUFnx9230uH8hQQaFQHgmd3upD) | 2026-05-31 (content not retrieved) |
| [Is ICT Trading Legit? — Phidias Prop Firm](https://phidiaspropfirm.com/education/is-ict-legit) | 2026-05-31 |
| [ICT Inner Circle Trader Review — ForexPeaceArmy (HTTP 403)](https://www.forexpeacearmy.com/forex-reviews/13001/inner-circle-trader-ICT) | 2026-05-31 |
| [Michael Huddleston aka ICT - Trading's Biggest Fraud — YouTube](https://www.youtube.com/watch?v=KuT-sN4FJ90) | 2026-05-31 |
| [Community Backtest Result — Threads.com (HTTP 403)](https://www.threads.com/@iambriancharles/post/DNFXN3exYvC/) | 2026-05-31 |
| [FVG Best Practices / Edgeful data (HTTP 403)](https://www.edgeful.com/blog/posts/fair-value-gap-best-practices-guide) | 2026-05-31 |
| [smart-money-concepts Python package — GitHub](https://github.com/joshyattridge/smart-money-concepts) | 2026-05-31 |

## Analyst Notes

**FACTS (from sources):**
- Three killzone windows: 03:00–04:00 AM, 10:00–11:00 AM, 02:00–03:00 PM EST
- Full rule set: liquidity sweep → MSS on 1–3 min → FVG entry → SL at FVG extreme → TP at opposite liquidity
- Claimed win rate: 70–80%; minimum R:R 1:2
- One community backtest: 62.5% WR over 10 days on EURUSD, +24.71R ($25K account)
- ICT failed 2016 $10K→$1M challenge; blew 2024 Robbins World Cup account
- ForexPeaceArmy: polarized reviews; fraud allegations documented
- 92% student confusion post-mentorship (poll)
- Pre-2023 results "vary widely" from post-2023
- "Selectively profitable" — inconsistent month-to-month
- Python packages exist for FVG/SMC indicator detection
- YouTube 5-year backtest playlist exists but content not retrieved

**ANALYSIS (my reasoning):**
- Implied expectancy at 70% WR / 1:2 RR = +1.1R per trade — extraordinary for a
  discretionary scalping setup; inconsistent with creator's live-trading failures [ANALYSIS]
- MSS identification on 1-min chart is the structural weakness: it is impossible to
  separate a valid MSS from random price oscillation in real time without rules that
  are inevitably discretionary and hindsight-biased [ANALYSIS]
- The FVG fill-rate data (76% fill within 2 days; 66.3% long FVG win rate from Edgeful)
  suggests the underlying concept (price filling imbalances) has some statistical support,
  but the Silver Bullet specifically requires killzone timing + liquidity sweep + MSS in
  sequence — none of the partial studies confirm this full combination works systematically
  [ANALYSIS]
- Session timing (killzone hours) is independently supported by microstructure research
  (higher institutional activity → higher mean-reversion probability after fakeouts) —
  this is the strongest element of the economic rationale [ANALYSIS]

**ASSUMPTIONS (not confirmed):**
- Backtests assume no slippage and retail spreads
- "Market Structure Shift" in all community descriptions is visually defined, not coded

## Future Research Needed

1. **Mechanical backtest of the full setup** (algorithmic, no discretion): Code killzone
   filter + FVG detection + liquidity sweep detection + fixed MSS definition on EURUSD
   historical 1-min data 2015–2025; compare to random entries in the same windows
2. **Out-of-sample validation:** Test the pre-2022 period (to check the "results vary
   widely" claim) and compare against 2022–2025
3. **Transaction-cost-inclusive performance:** Add realistic spread (1.2 pips EURUSD)
   and commission to any backtest
4. **Session-timing isolation study:** Does killzone-only entry with random direction
   (no setup filter) still beat non-killzone entry? This would isolate the timing
   effect from the setup selection
5. **Audited live track record:** Search for prop-firm verified traders using Silver
   Bullet for 6+ months with 200+ trades and full position logs
