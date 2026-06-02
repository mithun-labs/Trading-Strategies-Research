# Implementation Candidate: TSMOM China Commodity Futures

**Strategy slug:** `tsmom-china-commodity-futures`
**Score:** 60 (Worth Researching) | latent 60 | cap at 74 (Evidence auditability = 6)
**Added:** 2026-06-02

## Why It Qualifies

- Latent score ≥ 60 (exactly 60)
- TSMOM logic is inspectable and codeable from Moskowitz et al. (2012) — standard sign-based
  vol-managed time-series momentum formula
- Peer-reviewed source (Journal of Futures Markets 2025)

## Implementation Notes

**Important caveat:** This strategy is designed for **Chinese domestic commodity futures**
(SHFE, DCE, CZCE), which are not directly accessible to most international investors.
For non-Chinese investors, a proxy implementation using LME/CME commodity futures with
the same TSMOM logic can test whether the mechanism generalizes.

### Basic TSMOM Python Implementation

```python
import pandas as pd
import numpy as np

def tsmom_portfolio(returns_df, lookback=1, target_vol=0.40):
    """
    Basic time-series momentum portfolio.
    returns_df: monthly returns for each commodity futures (columns = contracts)
    lookback: J-month lookback period (best = 1 in Chinese markets)
    target_vol: annualized target vol per contract
    """
    signals = np.sign(returns_df.shift(lookback))
    realized_vol = returns_df.rolling(12).std() * np.sqrt(12)  # annualized
    weights = signals * (target_vol / realized_vol)
    weights = weights.divide(weights.abs().sum(axis=1), axis=0)  # equal-risk normalize
    portfolio_return = (weights * returns_df).sum(axis=1)
    return portfolio_return
```

### Data Sources

- **Chinese domestic (recommended for research):**
  - Tushare (free API, covers major SHFE/DCE/CZCE futures)
  - Wind Financial Terminal (institutional, comprehensive)
  - JoinQuant or RiceQuant (Chinese quant platforms)
- **International proxy (for non-Chinese implementation):**
  - CME Group: copper (HG), gold (GC), crude oil (CL), corn (ZC), soybeans (ZS), wheat (ZW)
  - LME: copper, aluminum, zinc
  - ICEX / DGCX for regional exposure

### Key Parameters

- Lookback J = 1 month (best documented for China; Moskowitz uses 12 months for US/global)
- Holding period K = 1 month
- Vol target: calibrate to 15–20% annualized portfolio vol
- Contract rolling: use continuous front-month contracts with calendar roll

### Gate for Advancement

The 4 enhanced strategies in Zheng et al. (2025) are paywalled. Before live implementation:
1. Obtain full paper to access enhanced strategy rules
2. Replicate basic 1-month TSMOM on Chinese data to confirm Cho et al. (2019) Sharpe
3. Run OOS test on 2020–2024 sub-period
4. Apply realistic transaction costs per Chinese exchange fee schedule
5. Test with minimum 12-contract diversification before scaling to 64

## Risk Warnings

- Chinese market accessibility restrictions are the primary barrier
- Regulatory intervention risk in Chinese commodity markets is HIGH
- Basic strategy Sharpe ~0.07 (Liu et al. 2023) is low; enhanced version performance is paywalled
- Not suitable for live trading without paper access and complete rule verification
