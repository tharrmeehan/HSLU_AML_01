# HSLU AML 01 — Household Energy Consumption Analysis

**Applied Machine Learning and Predictive Modelling 1 · FS 2026 · Hochschule Luzern**

Team repository: [raphaelspgll/HSLU_AML_01](https://github.com/raphaelspgll/HSLU_AML_01)

## Project

This group project studies daily electricity consumption of Swiss households
with heat pumps using HEAPO data. It includes regression and classification
workflows; results and limitations are documented in the team report.

- Dataset: [HEAPO: Household Energy and Smart Meter Data](https://doi.org/10.5281/zenodo.15056919) (CC BY 4.0).
- Tharrmeehan's contributions: [linear regression and SVM analyses](https://github.com/raphaelspgll/HSLU_AML_01/pull/7) and [per-household SVM threshold](https://github.com/raphaelspgll/HSLU_AML_01/pull/24).
- The tracked daily artifact and filtered modelling data make replay possible
  without downloading raw HEAPO files or re-running data preparation.

## Quick replay

From the repository root, use base R to inspect the saved comparison without fitting:

```sh
Rscript --vanilla -e 'print(readRDS("models/cv_class_results.rds"))'
```

This reproduces access to the saved team CV result; it does not retrain or
rerun cross-validation. To fit a model, open `HSLU_AML_01.Rproj`, restore the library with `renv::restore()`, then run the corresponding script in `R/`.

The saved results and report are team outputs. They should not be interpreted
as owner-only work or as evidence from a household-independent or future-time
holdout.

## Data and labels

The modelling subset is randomly sampled at household level after filtering
the 2022-01-01–2024-02-29 window and requiring at least 95% daily coverage.
The NN and SVM workflows calculate each household's q75 from the full
modelling subset before making row-level folds, then apply that household's
threshold to its rows. This is not a threshold estimated within each
training fold. The folds split rows, not households or time, so they do not
establish generalisation to unseen households or future dates.

The binomial workflow consumes the existing dataset-wide median-derived
label; the NN/SVM workflows use per-household q75 labels. Those classification
results are not directly comparable as-is. Neural-network saved CV results
are replayable, but its script also creates a random row-fold “test” split
from the same dataset; do not describe that score as an independent held-out
test result.

The tracked artifacts include `data_processed/heapo/heapo_dataset_daily.rds`,
`heapo_modelling.rds/.csv`, and Poisson counts. `renv.lock` and `renv/` record
the project environment. See the [team report](report/report.Rmd) for the
methods and diagnostics. The saved model and CV outputs are unchanged.

## Authors

Emanuel Lemma, Raphaël Spagolla and Tharrmeehan Krishnathasan.
