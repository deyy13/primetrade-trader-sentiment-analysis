# Primetrade.ai Round-0 Assignment
## Trader Performance vs Market Sentiment Analysis

## Objective
This project analyzes the relationship between Bitcoin market sentiment (Fear/Greed Index) and trader behavior/performance using Hyperliquid historical trading data.

---

## Dataset
- historical_data.csv
- fear_greed_index.csv

---

## Methodology

### Data Preparation
- Cleaned missing values and duplicates
- Converted timestamps into daily date format
- Merged sentiment and trader datasets by date

### Features Engineered
- Daily PnL
- Win Rate
- Average Trade Size
- Trade Frequency
- Trader Segmentation (Frequent vs Infrequent)

---

## Key Insights
### 1. Fear Periods
Fear periods showed wider PnL volatility and stronger upside opportunities.

### 2. Greed Periods
Greed periods showed larger downside outliers, suggesting overconfidence risk.

### 3. Trader Segmentation
Frequent and infrequent traders showed distinct behavioral and profitability patterns.

---

## Strategy Recommendations
### Fear Rule:
Use selective entries with tighter risk controls during Fear periods.

### Greed Rule:
Reduce overtrading and aggressive positioning during Greed periods.

### Segment Rule:
Apply stricter controls for frequent traders; selective opportunities for infrequent traders.

---

## Files
- trader_sentiment_analysis.ipynb

---

## Tools Used
- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook
