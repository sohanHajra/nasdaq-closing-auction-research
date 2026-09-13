# Nasdaq Closing Auction Forecasting Research

A research-engineering project that studies how pre-close auction information can be used to forecast the final Nasdaq closing-cross price.

The project is structured around a realistic quantitative-research workflow: inspect the market-data schema, build a leakage-safe target, audit data quality, engineer economically interpretable auction features, compare simple and nonlinear models, and evaluate the final specification on a chronological holdout.

## What this project demonstrates

- Market-microstructure feature engineering around indicative auction prices, imbalance, paired volume, and quoted liquidity
- Fixed-horizon modeling at 120, 60, 30, 10, 5, and 1 seconds before the close
- Stock-day target construction with snapshot-level data-quality checks
- Chronological train / validation / test splitting instead of random row splits
- Baseline comparison using midpoint, Reference price, and Near price
- Linear regression, Ridge, and XGBoost model comparison
- Validation-first model selection with a frozen final test set
- Robustness checks by trading date and held-out error distribution

## Repository structure

```text
.
├── notebooks/
│   └── closing_auction_forecasting.ipynb
├── data/
│   └── README.md
├── requirements.txt
├── .gitignore
└── README.md
```

## Data

The source dataset used in the original research is **not included or redistributed** in this repository.

The notebook expects a local archive at:

```text
data/nasdaq_closing_auction.tar
```

containing daily `*.csv.gz` files with auction snapshots. The public notebook has all execution outputs cleared.

This keeps the repository focused on the research process and code while respecting data-redistribution constraints.

## Research design

### Target

For each pre-close observation, the target is the final closing-cross displacement relative to the contemporaneous bid-ask midpoint:

$$
y_t = 10{,}000\left(\frac{C}{M_t}-1\right),
\qquad
M_t = \frac{bid_t + ask_t}{2}.
$$

The target is measured in basis points so errors are comparable across securities with different price levels.

### Features

The notebook evaluates:

- Near-price displacement
- Far-price displacement
- Reference-price displacement
- Signed imbalance normalized by ADV
- Signed imbalance normalized by contemporaneous auction size
- Paired auction volume
- Quoted spread
- Top-of-book imbalance
- Intraday price context

### Evaluation

The modeling sample is restricted to fixed time-to-close horizons. Dates are split chronologically into training, validation, and final test periods.

Model complexity is introduced gradually:

1. Market-price baselines
2. Linear regression
3. Ridge regression
4. Gradient-boosted trees

The final test set is not used for feature selection, hyperparameter tuning, or model architecture decisions.

## Notes

This repository is a **public portfolio adaptation** of a larger private research exercise. It contains my code and methodology only; it does not include the original task materials, recruiter/process documents, source-data download links, or the underlying dataset.

## Author

**Sohan Hajra**  
B.S. Mathematics & Computer Science, University of Illinois Urbana-Champaign  
Quantitative Research · Research Engineering · Systematic Trading
