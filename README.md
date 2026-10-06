# XLSTAT-project1
# Predicting Flourishing – Logistic Regression (Excel / XLSTAT)

This project studies which individual characteristics predict a high positivity ratio, using a survey of 248 individuals. The analysis was done in Excel with XLSTAT.

## Data

Each row is one individual. The variables are age, education level (`NIVEAUETUDE`), sex, family situation (`SITUFAM`), two mental health categories (`MH_Prof`, `MH_Priv`), two mental health scores (`MHCB_total`, `MHCC_total`), the positivity ratio (`PsurN`) and a flow score (`Flux_Total`).

The target variable `Positivity` is binary: it equals 1 when the positivity ratio is at least 3 (`PsurN >= 3`) and 0 otherwise. Only 20 individuals out of 248 (8%) are "positive", so the classes are strongly imbalanced.

## Process

1. **Descriptive statistics** on all quantitative and categorical variables (sheet `data exploration`).
2. **Bivariate tests** comparing positive and non-positive individuals on each variable (one sheet per variable). Age, `MHCB_total`, `MHCC_total`, `Flux_Total`, `MH_Prof` and `MH_Priv` differ significantly between the two groups. Education level and family situation do not, and sex is borderline (p = 0.054).
3. **Logistic regressions (logit)** with stepwise variable selection, adding one variable at a time:

| Model | Variables kept | AIC | Nagelkerke R² | AUC |
|---|---|---|---|---|
| M1 | MHCC_total | 117.2 | 0.23 | 0.80 |
| M2 | MHCC_total, SEX | 114.3 | 0.27 | 0.84 |
| M3 | MHCC_total, Flux_Total, SEX | 110.9 | 0.31 | 0.86 |
| Quadratic | MHCC_total², Flux_Total², SEX | 110.7 | 0.31 | 0.86 |
| Interactions | Flux_Total, SEX × MHCC_total, SEX | 107.7 | 0.34 | 0.87 |

4. **Robustness checks**: squared terms test for non-linear effects, and interaction terms test whether the effect of the mental health score depends on sex.

## Results

M3 is the main model: P(Positivity = 1) = 1 / (1 + e^-(−13.99 + 0.110 × MHCC_total + 0.113 × Flux_Total + 1.34 × SEX_1)).
All three variables are significant at 5%. Each extra point of `MHCC_total` multiplies the odds of being positive by about 1.12, and each extra point of flow by about 1.12. Adding squared terms barely improves the fit, so the effects are close to linear. The interaction model fits slightly better (lowest AIC, AUC 0.87), which suggests the effect of the mental health score differs by sex.

## Limits

The sample is small and very imbalanced (only 20 positive cases), so the coefficients are imprecise and the models were not tested on a separate sample. The cut-off at `PsurN >= 3` is a modelling choice, and the results are correlations, not causal effects.

## Files

`Dataset_-_Final_Project_1.xlsx` holds the raw data, the prepared data, and one sheet per test or model. Opening the XLSTAT outputs requires Excel; the add-in is not needed just to read them.
