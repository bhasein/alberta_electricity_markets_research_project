# Alberta Electricity Market Research

An independent research project examining electricity prices, scarcity, generation, weather, and market structure in Alberta.

The project combines historical data from the Alberta Electric System Operator (AESO) with spatial ERA5 weather data. A Python pipeline cleans, audits, and combines the source data into a one-row-per-hour analytical dataset covering more than 96,000 hours.

## Research questions

The project investigates:

- What physical conditions make Alberta’s electricity system scarce?
- How do demand, renewable generation, outages, imports, and thermal availability affect prices?
- How do natural-gas costs and the AESO offer stack contribute to price formation?
- How do weather and calendar patterns affect load and renewable output?
- Which relationships remain stable across years and changing market regimes?

## Notebooks

| Notebook | Focus |
|---|---|
| `01_Market_Orientation.ipynb` | Market structure, historical trends, and price regimes |
| `02_Scarcity_&_System_Tightness.ipynb` | Net load, thermal headroom, outages, imports, and scarcity events |
| `03_Fuel_Economics_&_Price_Formation.ipynb` | Natural-gas costs, merit orders, and offer-stack conditions |
| `04_Weather_Demand_&_Renewables.ipynb` | Temperature, demand, wind, solar, and extreme weather |
| `05_Temporal_Price_Dynamics.ipynb` | Calendar effects, price persistence, volatility, and market memory |
| `06_Market_State_Modeling.ipynb` | Chronological model validation, calibration, ablation, and regime stability |

The first five notebooks develop the economic and physical evidence. Notebook 6 tests whether those relationships remain informative across different years and market regimes.

## Main findings

- Thermal headroom and net load are strong indicators of physical system tightness.
- Outages matter most when the system is already operating with limited spare capacity.
- Imports often respond to scarcity rather than reliably predicting it.
- Natural-gas costs help explain ordinary price levels but not most extreme-price events.
- Exhaustion of inexpensive offered supply is closely associated with severe prices.
- Cold, low-wind conditions create the strongest compound pressure on net load.
- Recent price behaviour and offer-stack headroom provide the most consistently useful information across evaluation years.

These results describe historical market states and relationships. They should not be interpreted as causal estimates or operational price forecasts.

## Data pipeline

The pipeline processes:

- AESO pool prices and system load;
- generation and available capability;
- outages and intertie conditions;
- merit-order and system-marginal-price data;
- natural-gas prices;
- regional load;
- ERA5 weather data;
- calendar and lagged market features.

The final analytical dataset is written to:

```text
data/master/master_hourly.parquet
```

All datasets are aligned to an hourly UTC timestamp and pass source-specific audits before being included in the master table.

## Quick start

Create the environment:

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
pip install -e . --no-deps
```

Run the complete pipeline:

```bash
python src/run_pipeline.py
```

Force every stage to rebuild:

```bash
python src/run_pipeline.py --overwrite
```

Run the tests:

```bash
python -m unittest discover -s tests -v
```

Launch the notebooks:

```bash
jupyter lab
```

## Repository structure

```text
.
├── data/
│   ├── raw/                  # Original source data
│   ├── preprocessing/        # Cleaned source-level datasets
│   ├── feature_engineering/  # Engineered hourly features
│   ├── master/               # Final analytical dataset
│   └── audits/               # Data-quality evidence
├── notebooks/                # Research notebooks
├── src/                      # Download, preprocessing, and feature pipelines
├── tests/                    # Pipeline and data-contract tests
├── requirements.txt
└── README.md
```

## Data availability

Raw AESO and ERA5 data are not distributed with this repository. They must be downloaded separately and placed under `data/raw/` before rebuilding the full pipeline.

Generated data products are also excluded from Git because of their size. The source code, pipeline structure, audits, and notebooks document how the analytical dataset is constructed.

## Scope

This is an independent historical research project. It is not affiliated with AESO and does not provide trading recommendations, operational forecasts, or investment advice.
