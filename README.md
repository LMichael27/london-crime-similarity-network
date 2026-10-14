# Modelling Crime Rates Through Sociodemographic Similarities

Final project for **Simulation and Modeling** at Bocconi University.

**Authors:** Matteo Roda, Michael Ladaa, Muhammad Bin Arshad, Yash Patel

📄 **[Read the full report (PDF)](submission/Report/Modelling_Crime_Rates_Report.pdf)**

---

## Overview

To what extent can sociodemographic similarities between neighbourhoods explain how crime evolves?
We study the 33 London boroughs over almost two decades (2001–2018) and model the **spread of safety**
as a contagion process on a network of boroughs.

1. **Similarity network.** Boroughs are connected by socio-economic similarity (median salary, inactivity
   rate, employment rates of white and minority groups, UK-born and non-UK-born) plus geographic
   adjacency (shared borders are always linked). A separate network is built for each period:
   2001–2006, 2007–2012 and 2013–2018.
2. **SI model.** Crime rate (crimes per 1,000 inhabitants) is split at the median into *safe* and
   *dangerous* boroughs. Safe boroughs are treated as "infected", and safety spreads to connected
   dangerous boroughs with rate **β**. The network switches over at each period boundary, and every
   borough keeps its state across the switch.
3. **Calibration.** A grid search over **11 network weightings × 20 β values = 220 configurations**, each run
   200 times, is fitted to the 3-year moving average of safe boroughs using maximum likelihood estimation.

## Key result

The **minority-employment-focused network** with **β = 0.0158** fits best (log-likelihood ≈ −16). It
reproduces the gradual shift from about 8 safe boroughs in 2005 to about 25 in 2018. Disparities in
minority employment come out as the strongest predictor of how crime changes between boroughs.

## Repository structure

```
submission/
├── Report/
│   └── Modelling_Crime_Rates_Report.pdf       # final paper
└── Code/
    ├── datacleaning1.ipynb                    # merge housing/crime data with borough shapes, EDA
    ├── datacleaning2.ipynb                    # clean labour-market data (inactivity, employment by ethnicity)
    ├── final_model.ipynb                      # network construction, SI model, calibration, results
    └── data/
        ├── cleaned_housing_data.csv.xz        # compressed (see below)
        ├── housing_in_london_{monthly,yearly}_variables.csv
        ├── London_Boroughs.gpkg
        ├── employment_by_ethnicity.csv
        ├── inactivity_in_london.csv
        ├── *.xls                               # raw London Datastore files
        └── utils.py                            # plotting helpers
```

## Getting started

```bash
git clone https://github.com/LMichael27/london-crime-similarity-network.git
cd london-crime-similarity-network/submission/Code

# The cleaned dataset is 319 MB uncompressed, which is over GitHub's file size limit
xz -dk data/cleaned_housing_data.csv.xz

pip install pandas numpy scipy matplotlib seaborn geopandas networkx xlrd jupyter
jupyter notebook final_model.ipynb
```

> The final notebook runs `from utils import *`, and `utils.py` is in `data/`. Before running it, copy the
> file next to the notebook (`cp data/utils.py .`) or add `import sys; sys.path.append("data")` at the top.

## Data sources

- [Housing in London](https://www.kaggle.com/datasets/justinas/housing-in-london) (Kaggle): yearly and monthly borough data, including crime counts
- [London Boroughs boundaries](https://data.london.gov.uk/dataset/london_boroughs) (London Datastore)
- [Economic activity, employment and unemployment rate by ethnic group and nationality](https://data.london.gov.uk/dataset/economic-activity-rate-employment-rate-and-unemployment-rate-ethnic-group-national) (London Datastore)
