# Bayesian Thermal Inactivation Review

This repository contains the data, fitted Bayesian models, analysis outputs, and reproducible Quarto workflow for a systematic review of viral thermal inactivation. The analysis evaluates infectivity- and PCR-based outcomes as functions of treatment time, temperature, virus type, food matrix, and PCR pretreatment.

## Repository structure

- `Bayesian_Thermal_Inactivation_WW_v1.qmd`: Main data-processing, modeling, model-comparison, diagnostic, and visualization workflow.
- `data/raw_data/`: Source data used by the analysis.
- `data/cleaned_data/`: Prepared R data objects used for model fitting and visualization.
- `models/infectivity/`: Saved Bayesian models for infectivity outcomes.
- `models/pcr/`: Saved Bayesian models for PCR outcomes.
- `result/`: Tables, figures, posterior predictive checks, diagnostics, and model summaries.

## Requirements

The workflow is written in R and rendered with Quarto. It uses the following principal packages:

- `brms`
- `cmdstanr`
- `rstan`
- `posterior`
- `tidyverse`
- `readxl`
- `skimr`
- `here`
- `RColorBrewer`
- `patchwork`

CmdStan must be installed before fitting or loading models that require the CmdStan backend:

```r
cmdstanr::install_cmdstan()
```

## Reproducing the analysis

1. Clone the repository and open the project directory in RStudio or another Quarto-compatible environment.
2. Install the required R packages and CmdStan.
3. Render `Bayesian_Thermal_Inactivation_WW_v1.qmd` from the repository root.

```bash
quarto render Bayesian_Thermal_Inactivation_WW_v1.qmd
```

Saved model objects are included to support review of fitted models without repeating every computationally intensive fit. The Quarto file documents preprocessing, model specifications, model comparison, convergence assessment, posterior predictive checks, and output generation.

## Data note

The raw dataset includes study identifiers and experimental observations compiled from the literature. Consult the source studies identified in the dataset when reusing or interpreting individual observations.
