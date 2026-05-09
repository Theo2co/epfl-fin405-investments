# Investments Project — FIN-405

EPFL · Spring 2025 · Prof. Pierre Collin-Dufresne

Construction and evaluation of systematic investment strategies for a U.S. investor with exposure to international equity and currency markets.

## Authors (Group 10)

- DECAUX Théodore — 329169
- IMBERT Carlota — 324405
- ROBERT Adelaïde — 346471
- VELAY Mahé — 345882

## Strategies

| Code | Strategy | Idea |
|------|----------|------|
| **DIV** | International Diversification (Hedged Risk Parity) | Risk-parity portfolio across 6 foreign markets, FX-hedged via covered interest parity |
| **MOM** | Equity Index Momentum | Long-short on cumulative returns from t-12 to t-1 (skipping the most recent month) |
| **REV** | Long-Term Reversal | Long-short on cumulative returns from t-60 to t-12 (mean-reversion) |
| **CARRY** | Currency Carry | Long high-yielding currencies, short low-yielding ones (3M interbank rates) |
| **DOLLAR** | Long USD Basket | Equal-weight short on EUR, GBP, JPY, CHF, AUD |
| **STRAT** | Risk-parity blend of the four overlays | MOM + REV + CARRY + DOLLAR weighted by 1/σ |
| **FUND** | Final MVE portfolio | T-Bill + b·(DIV − T-Bill) + c·STRAT, targeting 15% annualized volatility |

## Key results

- **DIV (Hedged RP)**: 7.15% annualized return, 16.92% volatility, Sharpe 0.42
- **CARRY**: Sharpe 0.40, t-stat 1.92 (borderline significant)
- **MOM**: Sharpe -0.12 — no exploitable momentum at the index level
- **REV**: Sharpe 0.28, p = 0.25
- **FUND (Fama-French 5 regression)**: β_MKT = 0.74 (***), β_HML = 0.26 (**), R² = 0.65, α not statistically significant — consistent with the semi-strong form of the EMH

See `Project_Investments.pdf` for the full report.

## Methodology

The whole analysis is designed to avoid **look-ahead bias**: at each date `t`, parameters (weights, volatilities, covariances) are estimated on a 60-month rolling window ending at `t-1`, and applied to the return at `t+1`. FX hedging follows covered interest rate parity using 3-month interbank rates.

## Repository structure

```
.
├── FINAL_CATM.ipynb        # Main notebook — full analysis from Q3 to Q9
├── Project_Investments.pdf # Final report
├── Data/                   # ⚠️ Add locally (see below)
│   ├── world_indices.csv
│   ├── crsp_us.csv
│   └── fred_series.csv
├── requirements.txt
├── .gitignore
└── README.md
```

## Data

The notebook expects three files in a `Data/` folder at the project root:

- `world_indices.csv` — monthly returns by country (`date`, `country`, `mportret`)
- `crsp_us.csv` — monthly U.S. market returns (`date`, `ret_us`)
- `fred_series.csv` — 3M interbank rates and FX rates (FRED)

These files are **not included** in the repo (see `.gitignore`) as they come from licensed databases (CRSP, FRED). Place them in `Data/` before running the notebook.

## Installation

```bash
python -m venv .venv
source .venv/bin/activate    # or .venv\Scripts\activate on Windows
pip install -r requirements.txt
jupyter lab FINAL_CATM.ipynb
```

## Course

[FIN-405 — Investments](https://edu.epfl.ch/coursebook/en/investments-FIN-405), EPFL, Master in Financial Engineering.
