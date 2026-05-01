# Primetrade.ai — Round-0 Assignment
## Trader Performance vs Market Sentiment Analysis

---

## Objective

Analyze how Bitcoin market sentiment (Fear/Greed Index) relates to trader behavior and performance on Hyperliquid, and uncover patterns that inform smarter trading strategies.

---

## Datasets

| Dataset | Source | Rows | Columns |
|---|---|---|---|
| `historical_data.csv` | [Hyperliquid via Google Drive](https://drive.google.com/file/d/1IAfLZwu6rJzyWKgBToqwSmmVYU6VbjVs/view?usp=sharing) | ~200,000+ | account, symbol, execution price, size, side, time, closedPnL, leverage, etc. |
| `fear_greed_index.csv` | [Bitcoin Fear/Greed via Google Drive](https://drive.google.com/file/d/1PgQC0tO8XN-wqkNyghWc_-mnrYv_nhSf/view?usp=sharing) | ~365+ | timestamp, classification (Fear/Greed) |

> **Note:** `historical_data.csv` is not committed to this repo due to file size. Download it from the link above and place it in the root folder before running the notebook.

---

## Setup & How to Run

### 1. Clone the repo
```bash
git clone https://github.com/deyy13/primetrade-trader-sentiment-analysis.git
cd primetrade-trader-sentiment-analysis
```

### 2. Install dependencies
```bash
pip install pandas numpy matplotlib seaborn jupyter
```

### 3. Download the data
- Download `historical_data.csv` from the link above
- Place it in the root folder (same level as the notebook)
- `fear_greed_index.csv` is already included in the repo

### 4. Run the notebook
```bash
jupyter notebook trader_sentiment_analysis.ipynb
```
Run all cells top to bottom. Charts will be saved to the `charts/` folder automatically.

---

## Methodology

### Data Preparation
- Loaded both datasets and documented rows, columns, missing values, and duplicates
- Converted Unix timestamps in Fear/Greed data to readable dates (`unit='s'`)
- Converted IST timestamps in trader data and extracted date-only field
- Merged both datasets on `date` (inner join) — daily granularity
- Coerced `Closed PnL` and `Size USD` to numeric, handled nulls

### Key Metrics Engineered
- **Daily PnL** per account and in aggregate
- **Win rate** (% of trades with positive closed PnL)
- **Average trade size** (USD)
- **Trade frequency** per trader and per day
- **Leverage distribution** (median per trader, distribution overall)
- **Long/Short ratio** by sentiment day
- **Drawdown proxy** (5th percentile PnL per sentiment group)

### Trader Segments Created
| Segment | Definition |
|---|---|
| Frequent vs Infrequent | Split at median trade count per account |
| High vs Low Leverage | Split at median leverage per account |
| Consistent Winners vs Inconsistent | Win rate > 55% AND ≥ 10 trades |

---

## Key Insights

### Insight 1 — Fear Days Show Higher PnL Volatility
Fear periods exhibit wider PnL dispersion (higher std deviation) and stronger upside outliers. This suggests that while Fear days carry more risk, they also contain larger profit opportunities for disciplined traders. The drawdown proxy (5th percentile PnL) is also more extreme during Fear, confirming higher tail risk in both directions.

### Insight 2 — Greed Days Produce More Consistent But Smaller Wins
Greed periods show a narrower PnL distribution with notable negative outliers — likely driven by overconfident positioning as markets become crowded. Win rate tends to be slightly higher on Greed days, but average PnL per winning trade is lower, pointing to reduced alpha.

### Insight 3 — Long/Short Bias Shifts With Sentiment
Traders skew more heavily Long during Greed days and more evenly split during Fear days. This momentum-chasing behavior can amplify losses when Greed peaks coincide with reversals.

### Insight 4 — High Leverage Traders Underperform Low Leverage Peers
High leverage traders (above median leverage) show lower win rates and higher PnL variance — particularly on Fear days where volatility is already elevated. Low leverage traders produce more stable, consistent returns.

### Insight 5 — Consistent Winners Show Sentiment Adaptability
Consistent Winners (win rate > 55%, ≥ 10 trades) maintain positive average PnL across both Fear and Greed days. Inconsistent traders, by contrast, perform relatively better on Greed days but suffer disproportionately on Fear days — suggesting they rely on trend-following rather than edge.

---

## Strategy Recommendations

### Strategy 1 — Fear Day Playbook (Segment: Low Leverage + Consistent Winners)
> **Rule:** During Fear days, cap leverage at or below your median historical leverage. Consistent Winners should maintain their normal trade frequency; Inconsistent traders should reduce position sizes by 30–50% and focus only on high-conviction setups.
>
> **Rationale:** Fear days carry higher tail risk (more extreme drawdowns). Low leverage traders consistently survive and profit from the volatility spikes that wipe out high-leverage peers.

### Strategy 2 — Greed Day Playbook (Segment: Frequent Traders)
> **Rule:** During Greed days, frequent traders should impose a maximum daily trade cap (e.g., no more than 1.5x their average daily trades). Avoid adding leverage above your median threshold even when trades are going well. Monitor long/short ratio — if >70% long, reduce exposure.
>
> **Rationale:** Greed periods attract momentum-chasing and overtrading. Frequent traders are most at risk of compounding small losses through excessive activity. The data shows Greed days carry the worst negative outliers — likely caused by crowded longs reversing sharply.

---

## Output Files

All charts and tables are in the `charts/` folder:

| File | Description |
|---|---|
| `leverage_distribution.png` | Overall and Fear vs Greed leverage histograms |
| `long_short_ratio.png` | Long vs Short trade count by sentiment |
| `winrate_drawdown.png` | Win rate and drawdown proxy by sentiment |
| `behavior_by_sentiment.png` | Avg leverage and position size by sentiment |
| `all_segments.png` | Win rate across all 3 trader segments |
| `performance_by_sentiment.csv` | Full PnL, win rate, drawdown stats table |
| `leverage_segment_performance.csv` | High vs Low leverage performance |
| `consistency_segment_performance.csv` | Consistent vs Inconsistent by sentiment |
| `long_short_ratio.csv` | L/S counts and ratio by sentiment |
| `behavior_by_sentiment.csv` | Leverage and position size behavior table |

---

## Tools Used

- Python 3.10+
- pandas, numpy
- matplotlib, seaborn
- Jupyter Notebook

---

## Repository Structure

```
primetrade-trader-sentiment-analysis/
├── trader_sentiment_analysis.ipynb   # Main analysis notebook
├── fear_greed_index.csv              # Fear/Greed dataset (included)
├── historical_data.csv               # Trader data (download separately)
├── charts/                           # All output charts and tables
│   ├── leverage_distribution.png
│   ├── long_short_ratio.png
│   ├── winrate_drawdown.png
│   ├── behavior_by_sentiment.png
│   ├── all_segments.png
│   └── *.csv
└── README.md
```
