# Trader Performance vs Market Sentiment
### Primetrade.ai — Data Science Intern Assignment (Round 0)

---

## Overview
This project analyzes how Bitcoin market sentiment (Fear/Greed Index) relates to trader behavior and performance on Hyperliquid, using 211,224 trades across 32 accounts over 479 days in 2024.

---

## Datasets
| Dataset | Rows | Columns | Source |
|---|---|---|---|
| Hyperliquid Historical Trades | 211,224 | 16 | Hyperliquid DEX |
| Bitcoin Fear & Greed Index | 2,644 | 4 | Alternative.me |

---

## Setup & How to Run

### Requirements
```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

### Run the Notebook
```bash
git clone <your-repo-url>
cd primetrade-analysis
# Place both CSV files in the root folder:
#   historical_data.csv
#   fear_greed_index.csv
jupyter notebook primetrade_analysis.ipynb
```

---

## Methodology

1. **Data Cleaning**: Parsed `Timestamp IST` with `dayfirst=True`, verified zero nulls in both datasets, removed no rows (clean data).
2. **Alignment**: Merged both datasets on `date` (daily granularity) → 211,218 matched rows across 479 overlapping dates.
3. **Feature Engineering**:
   - `is_win`: Closed PnL > 0
   - `is_long` / `is_short`: Mapped from Direction field
   - `sentiment_simple`: Collapsed 5 classes → Fear / Neutral / Greed
   - `size_segment`, `freq_segment`, `winner_segment`: Trader segmentation via quartile splits + win rate logic
4. **Analysis**: Grouped by sentiment classification, computed PnL stats, win rates, trade frequency, long/short ratio, and position sizes.

---

## Key Insights

### 1. Fear Days Are Surprisingly Profitable
Traders earn **higher avg PnL on Fear days ($54.29)** than on Greed days ($42.74). Fear creates contrarian buying opportunities.

### 2. Extreme Greed = Short Opportunity
The Long/Short ratio **drops below 1.0** during Extreme Greed — experienced traders go net-short when sentiment peaks. Win rate also peaks at **46.5%** on Extreme Greed days.

### 3. Panic Trading Hurts Returns
Extreme Fear triggers **4× more trades per day** (1,528 vs 350) yet produces the **lowest win rate (37%)**. Overtrading in panic markets destroys alpha.

### 4. Small Size + High Frequency Wins
Consistent Winners average **$2,602 trade size** vs $6,418 for mixed traders, with a **63% win rate**. Frequent traders generate **3.4× more total PnL** than infrequent ones.

### 5. Elite Traders Are Naturally Contrarian
L/S ratio = **2.10 during Extreme Fear** (buy the dip) vs **0.72 during Extreme Greed** (fade the rally). Smart money consistently opposes crowd sentiment.

---

## Strategy Recommendations

### Strategy 1: Fear-Day Long Bias Rule
> *"During Fear/Extreme Fear days, conservative traders should increase long exposure by 20–30%. Avoid aggressive short-selling — it coincides with maximum volume but minimum win rate."*

### Strategy 2: Greed-Day Caution Rule
> *"On Extreme Greed days, shift bias toward SHORT or reduce position size by 25%. Frequent traders especially benefit from this contrarian discipline."*

### Strategy 3: Consistent-Winner Sizing Rule
> *"Cap individual trade size at <$3,000 USD equivalent. Scale frequency over size. High-size traders show 8% lower win rates — discipline beats aggression."*

---

## Files
```
├── primetrade_analysis.ipynb   ← Full analysis notebook
├── README.md                   ← This file
├── fig1_sentiment_analysis.png ← 4-panel sentiment chart
├── fig2_segments.png           ← Trader segmentation chart
├── fig3_timeseries.png         ← Daily PnL vs FG time series
```

---

## Results Summary Table

| Sentiment | Avg PnL | Win Rate | Avg Trades/Day | L/S Ratio |
|---|---|---|---|---|
| Extreme Fear | $34.54 | 37.1% | 1,529 | 2.10 |
| Fear | $54.29 | 42.1% | 680 | 1.74 |
| Neutral | $34.31 | 39.7% | 562 | 1.66 |
| Greed | $42.74 | 38.5% | 261 | 0.72 |
| Extreme Greed | $67.89 | 46.5% | 351 | 0.95 |
