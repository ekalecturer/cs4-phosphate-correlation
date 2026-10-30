# The Feasibility Stress Test — Case Study #4: Correlation vs. Independence, Abu Tartur Phosphate Mine

A Monte Carlo simulation built on the **real, fully-read data** of a published case study, demonstrating the paper's own central finding: correlation between variables matters more than the choice of distribution shape.

## The real case study

**Kamel, A., Elwageeh, M., Bondua, S., Elkarmoty, M. (2023).** *Evaluation of mining projects subjected to economic uncertainties using the Monte Carlo simulation and the binomial tree method: Case study in a phosphate mine in Egypt.* Resources Policy, 80, 103266. [DOI: 10.1016/j.resourpol.2022.103266](https://doi.org/10.1016/j.resourpol.2022.103266)

Unlike the other three case studies in this series, **the full paper was read**, not just the abstract. Every number in this repo's data files is transcribed directly from the paper's own tables.

Egypt's Abu Tartur phosphate project was valued four times, each pass adding a layer of real-world uncertainty on top of the last.

### Confirmed results (Table 8)

| Scenario | Method | Uncertain variables | Mean NPV | vs. base case | 95% CI |
|---|---|---|---|---|---|
| 1 | DCF | None | $2.26B | 0% | — |
| 2 | BDT | Ore price | $2.13B | −6% | <50% confidence |
| 3 | MCS | Price + operating cost | $2.35B | +4% | $0–$4.26B |
| 4 | MCS | Price, opex, grade, recovery, interest, inflation, tax | $3.04B | +34% | $0–$5.94B |

**The paper's own conclusion (Discussion section):** *"Correlations must be included in Monte Carlo Simulation otherwise the analysis leads to an underestimate of risk. It is relatively more important to investigate and allow correlations between the variables than to select the best fit distribution to represent the parameters."*

**A detail worth knowing:** two of the seven "uncertain" parameters in scenario 4 — income tax rate and mill recovery — are reported with 0% standard deviation in the paper's own Table 6. A variable with zero standard deviation does not vary in a simulation.

## What's in this repo

- `notebook.ipynb` — uses the paper's **real** distributions (Table 6) and **real** correlation matrix (Table 7, corrected for positive semi-definiteness) via a Gaussian copula, comparing a correlated run against an independent run to demonstrate the paper's own headline finding
- `data/technical_parameters.csv`, `data/financial_parameters.csv` — the project's real technical and financial inputs (Tables 2 and 3)
- `data/scenario_results.csv` — the four-scenario comparison table (Table 8)
- `data/scenario_1_dcf_results.csv`, `data/scenario_2_bdt_inputs.csv` — per-scenario detail
- `data/scenario_4_distributions.csv` — the real distributions used for the seven "uncertain" parameters (Table 6), including the two with 0% SD
- `data/correlation_matrix.csv` — the real 7×7 correlation matrix (Table 7)
- `data/notebook_reconstruction_output.json` — this repo's own simulation output, for comparison
- `docs/` — GitHub Pages site (landing page + rendered notebook)

## Important: what this notebook does and doesn't reproduce

The distributions and correlation matrix are the paper's real, published numbers — not invented. What isn't reproduced is the paper's exact cash-flow mechanics: the NPV formula involves depreciation, depletion allowance, royalty, and salvage value whose year-by-year schedules the paper doesn't fully disclose. This notebook uses a transparent proxy (scaling the published $2.26B base-case NPV by margin and annuity ratios) instead of guessing at that missing data. It reproduces the **direction** of the paper's core finding (correlation widens the risk spread) faithfully; it does not reproduce the **magnitude** of the reported 34% mean uplift, which likely stems from a tax-asymmetry mechanic this proxy doesn't model. Stated directly inside the notebook.

## Running it

```bash
pip install -r requirements.txt
jupyter notebook notebook.ipynb
```

## License

MIT — see `LICENSE`. Code and structure are freely reusable; the underlying research belongs to its original authors and should always be cited.

---
Built as part of [The Feasibility Stress Test](#) — real Monte Carlo and financial-engineering case studies for capex decisions, published as a LinkedIn newsletter by Eka Krisna Santoso.
