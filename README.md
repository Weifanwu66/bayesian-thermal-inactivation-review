# Bayesian Thermal Inactivation Review

This repository contains the data, fitted Bayesian models, analysis outputs, and Quarto workflow for a systematic review of viral thermal inactivation. The analysis evaluates infectivity- and PCR-based outcomes as functions of treatment time, temperature, virus type, food matrix, and RT-qPCR pretreatment.

## Repository structure

- `Bayesian_Thermal_Inactivation_WW_v2.qmd`: Main data-processing, modeling, model-comparison, diagnostic, and visualization workflow.
- `data/raw_data/`: Source data used by the analysis.
- `data/cleaned_data/`: Prepared R data objects used for model fitting and visualization.
- `models/infectivity/`: Saved Bayesian models for infectivity outcomes.
- `models/pcr/`: Saved Bayesian models for PCR outcomes.
- `result/`: Tables, figures, posterior predictive checks, diagnostics, and model summaries.

## Study purpose and modeling strategy

The analysis quantifies viral inactivation as a function of treatment time, temperature, virus type, and food matrix using experimental observations compiled from multiple studies. Particular attention is given to shellfish, especially oysters, because the structure and composition of shellfish tissue may protect viruses during heating. Infectivity-based outcomes and RT-qPCR-based genome measurements are analyzed separately and then compared under matched experimental conditions.

Primary inactivation kinetics are evaluated with first-order and Weibull models. The first-order model assumes that log10 reduction increases linearly with time. The Weibull model allows curvature: a shape parameter of 1 represents log-linear behavior, a value below 1 represents tailing, and a value above 1 represents shouldering. The models are fit in a Bayesian hierarchical framework with between-study variation represented by study-level random effects. Candidate models are compared using leave-one-out cross-validation, and model adequacy is evaluated using MCMC diagnostics and posterior predictive checks.

## Data structure

The raw workbook contains experimental observations extracted from the literature. The principal variables used in the analysis are:

| Variable | Description | Values or units |
|---|---|---|
| `study_id` | Identifier for the study or article from which the observation was extracted | DOI link or study identifier |
| `virus_type` | Virus used in the thermal inactivation experiment | MNV-1, Tulane virus, HAV, FCV-F9, MS2, GA, and HuNoV GII |
| `food_matrix` | Food or medium in which the virus was suspended | Oysters, mussels, spinach, strawberry, milk, water, PBS, etc. |
| `food_category` | Broad classification of the experimental matrix | Shellfish, produce, dairy, or simple matrix |
| `temperature` | Heat-treatment temperature | °C |
| `time_min` | Duration of heat treatment when the sample was collected | Minutes |
| `detection_method` | Laboratory method used to measure the virus | RT-qPCR, plaque assay, or TCID50 |
| `assay_type` | Biologically meaningful grouping of detection methods | PCR or infectivity |
| `pretreatment` | Sample pretreatment used with PCR measurements | PMA, RNase, PGM, none, or other |
| `log_conc` | Observed virus concentration | log10 units |
| `LOD` | Assay limit of detection | log10 units |
| `log_reduction` | Reported reduction in virus concentration | log10 units |
| `note` | Censoring or other experimental notes | Free text |

## Data preprocessing

When a log reduction was not reported directly, it was calculated relative to the concentration at the earliest time point within each study, virus, matrix, temperature, detection-method, and pretreatment group. Observations reported below the concentration detection limit were represented as right-censored log reductions for modeling. In the cleaned files, `cens = 0` denotes an observed reduction and `cens = 1` denotes a right-censored reduction whose true value exceeds `log_red_obs`.

PCR pretreatments PMA, RNase, and PGM were pooled as `yes`; measurements without pretreatment were classified as `no`; and infectivity observations were classified as `not_applicable`. Experimental study-temperature combinations were retained only when they contained at least three distinct time points, providing enough temporal resolution to estimate inactivation dynamics.

## Cleaned RDS files

| File | Contents |
|---|---|
| `data/cleaned_data/cleaned_pooled_data.rds` | Complete cleaned dataset containing both infectivity and RT-qPCR observations. |
| `data/cleaned_data/clean_inf.rds` | Cleaned subset used for the infectivity analysis. |
| `data/cleaned_data/clean_pcr.rds` | Cleaned subset used for the RT-qPCR analysis. |
| `data/cleaned_data/plot_dat_pcr.rds` | RT-qPCR dataset prepared for plotting and residual diagnostics. |

The first three cleaned objects share these analysis columns:

| Column | Description |
|---|---|
| `log_red_obs` | Observed log10 reduction or the bound used for a censored observation |
| `cens` | Censoring indicator: `0` = observed; `1` = right-censored |
| `matrix_group` | Three-level matrix grouping: `shellfish`, `simple_matrix`, or `other_food` |
| `matrix_group2` | More detailed grouping used for selected exploratory comparisons, including shellfish, dairy, vegetable, berry, and simple matrix |
| `pcr_pretreat` | Pooled pretreatment indicator: `yes`, `no`, `other`, or `not_applicable` |

## Reproducing the analysis

1. Clone the repository and open the project directory in RStudio or another Quarto-compatible environment.
2. Install the required R packages and CmdStan.
3. Render `Bayesian_Thermal_Inactivation_WW_v2.qmd` from the repository root.

## License

The research materials in this repository are available under the [Creative Commons Attribution 4.0 International License](LICENSE). Reuse is permitted with appropriate attribution. Any manuscript published by *Food Research International* remains subject to the publishing agreement and license selected for that article.

## Citation

Wu, W., Havelaar, A. H., & Montazeri, N. (2026). Thermal inactivation kinetics of foodborne viruses across food matrices: A Bayesian meta-analysis and predictive framework. Food Research International, 120750. https://doi.org/10.1016/j.foodres.2026.120750
