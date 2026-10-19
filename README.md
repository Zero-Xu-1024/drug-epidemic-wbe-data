# Source data for a drug-epidemic model for China

These data support **A drug-epidemic model for China with dual treatment pathways, relapse, and wastewater observations**. The files contain the reported national administrative indicators, numerical results and plotted curves, and observations and estimates from the synthetic wastewater experiments.

The national indicators cover 2008–2019. The 2012–2019 comparison is retained to diagnose the consequences of the original uniform annual-entry mapping. The reported quantities include administrative orders, reporting arrivals, and treatment stocks, and their pathway coverage also differs from the model. Minimizing the numerical discrepancy between these numbers and model annual-entry integrals does not provide an empirical calibration of the national drug-use process. The numerical minimizers and intervention scenarios are not estimates or projections of national outcomes. The wastewater experiments describe a hypothetical catchment with specified model and emission coefficients. Their observations are simulated rather than collected from a wastewater monitoring site.

## Files and units

All CSV files use UTF-8, comma delimiters, decimal points, and a header row. Empty cells mean that the field does not apply to that record. Numerical values retain the precision of the calculations. Row counts exclude headers.

| File | Rows | Contents | Paper figures and tables |
|---|---:|---|---|
| `administrative_observations.csv` | 12 | Annual reported values, source definitions, and public source links | Table 2; reported indicators in Figure 5 and Table S4 |
| `numerical_results.csv` | 432 | Numerical settings, table values, annual predictions, and generating means | Table 3 and Tables S1–S7; Figures 3 and 5; Sections S6–S7 |
| `figure_curves.csv` | 9,062 | Source points for the trajectory plots | Figures 2B, 4, and 6 |
| `wbe_recovery_runs.csv` | 450 | Estimates from each synthetic recovery run | Figure 2A and Tables S8–S9 |
| `wbe_synthetic_observations.csv` | 100 | The shared noisy observations used by the recovery experiments | Sections S7.1–S7.4 |

### Administrative observations

`year` identifies the reporting year. `mandatory_reported_10k` and `community_reported_10k` are the source totals divided by 10,000. Their underlying units are persons or person-times as described by `mandatory_definition` and `community_definition`; the two columns should be read with those definitions. `source_url` identifies the public report or reproduction. `source_note` records qualifications needed to interpret the value. `used_in_fit` is a retained column name and is true for the 2012–2019 diagnostic comparison. It does not identify a homogeneous series suitable for empirical fitting.

The 2008 totals cover 1 June–31 December. The 2011 indicator definitions remain unverified. The available 2015 transcription describes current treatment stocks; its original page and stock date remain unconfirmed. Community totals can include post-isolation rehabilitation as well as detoxification. These distinctions are retained in the data.

### Numerical results

This is a long table with columns `block`, `case`, `entity`, `metric`, `value`, and `unit`. The first four columns identify a quantity; `value` is its numerical value. National state variables are in 10^4 persons, annual model flows in 10^4 events, and rates in year^-1. Local wastewater experiment states are in persons.

| `block` | Rows | Interpretation |
|---|---:|---|
| `reference_parameters` | 13 | Reference model parameters; `entity` gives the parameter |
| `fit_bounds` | 12 | Lower and upper bounds for the six rates in the diagnostic optimization |
| `initial_states` | 48 | Six components of eight initial-state or equilibrium vectors |
| `wbe_settings` | 6 | Local recruitment, emission coefficients, removal, initial wastewater mass, and relative noise scale |
| `elasticities` | 39 | Thirteen signed elasticities at three relative perturbations |
| `annual_predictions` | 64 | Community (`C`) and mandatory (`M`) annual integrals for 2012–2019 under four parameter/initial-state cases |
| `hidden_stock_profile` | 48 | Six diagnostic minimizers, the normalized numerical-discrepancy objective, and R0 for each of six assumed hidden initial stocks |
| `intervention_endpoints` | 84 | Six state values and R0 for six strategies under each parameter set at the start of 2030 |
| `structural_endpoints` | 33 | R0, I1(2030), and I3(2030) for the eleven structural alternatives |
| `population_totals` | 2 | Reference-case N(2019) and N(2020) |
| `convergence_checks` | 12 | The normalized endpoint error and maximum absolute state derivative for six long-time simulations |
| `local_jacobian` | 7 | Six singular values and the condition number of the parameter-bound-scaled residual Jacobian |
| `wbe_generating_means` | 64 | The mean and standard deviation of each of the 32 primary synthetic observations |

In `hidden_stock_profile`, `case` is the multiplier of the assumed I3(0)=1049. Its six rates and fixed reference parameters define the corresponding diagnostic minimizer. Case 1 supplies the conditional minimizer of the diagnostic objective in Table S3. In `annual_predictions`, the cases are `reference` and `hidden_factor_1`, `hidden_factor_2`, or `hidden_factor_8`. The reference and case-1 predictions also supply Table S4. Diagnostic residuals in percent are 100 × (model integral − reported number) / reported number. Both numbers use the tabulated 10^4 scaling, which does not equate their measurement types.

For `elasticities`, `E_h1percent`, `E_h01percent`, and `E_h001percent` correspond to h=10^-2, 10^-3, and 10^-4. Figure 3 uses h=10^-4. Each elasticity is theta/R0 times the partial derivative of R0 with respect to theta; it is dimensionless.

The `initial_states` cases are `calibration_2012`, `scenario_reference_2020`, `scenario_conditional_2020`, `convergence_initial_1`, `convergence_initial_3`, `equilibrium_subcritical`, `equilibrium_supercritical`, and `wbe_generating`. The `calibration_2012` and `scenario_conditional_2020` names are retained identifiers for the assumed diagnostic initial state and the diagnostic-parameter scenario state, respectively; they do not imply empirical calibration. Convergence initial state 2 equals `calibration_2012`. The subcritical convergence case uses the reference parameters, and the supercritical case changes only beta2 to 0.058028 year^-1.

For intervention results, `case` is `reference` or `conditional`; `conditional` denotes the diagnostic parameter set. These are deterministic model scenarios with prescribed rates and states. Within each case, every strategy starts from the corresponding 2020 initial state. Strategy 1 halves beta1 and beta2; Strategy 2 doubles gamma2; Strategy 3 doubles alpha1 and gamma1; Strategy 4 halves r2; Strategy 5 halves p. Other parameters are unchanged. Percentage changes are 100 × (strategy endpoint / its case's Baseline endpoint − 1).

The structural alternatives start from `scenario_reference_2020` and use the reference parameters with the indicated added pathway or rate. `Cessation sigma=...` transfers I1 to R; `Cessation to S sigma=...` transfers I1 to S. Mild initiation adds beta_mild S I1/N. `Erlang2 entry_fraction=0.5` splits each treatment stock equally between two serial stages; `entry_fraction=1` places it entirely in the first stage. Stage progression and completion rates are 2r1 and 2r2, with the competing exits retained. Changes in Table S7 are calculated against the `Original` row.

### Trajectory curves

`figure` identifies the figure, `case` the parameter case, and `series` the initial state, strategy, or observation design. `time` is in model years for Figures 2B and 4 and in calendar years for Figure 6.

- Figure 2B contains 161 points per series, from 0 to 8 years at increments of 0.05. `I3` is the true trajectory for `series=truth` and the median recovered trajectory for each observation design. `I3_q025` and `I3_q975` are the pointwise 2.5th and 97.5th percentiles across 100 noise realizations, in persons.
- Figure 4 contains 1,001 points for each of six trajectories, from 0 to 5000 years. `I3` is in 10^4 persons. `scaled_error` is max_j |X_j − X_j*| / (1 + |X_j*|) in the stated state units. The log-scale plot clips this quantity at 10^-12 for display; the CSV retains its calculated value.
- Figure 6 contains 201 points per strategy and parameter case, from 2020 to 2030. `I1` is in 10^4 persons. The other trajectory fields are empty.

### Synthetic observations and recovered values

`wbe_synthetic_observations.csv` has one row for each `replicate` from 0 to 99. Columns `C_01`–`C_08` and `M_01`–`M_08` are the eight annual cumulative entry counts for the community and mandatory pathways, in events, over (0,1], …, (7,8]. `L_01`–`L_16` are wastewater outflow mass loads L(t)=kappa W(t), in mg/year, at t=0.25, 0.75, …, 7.75 years. `C_repeat_01`–`C_repeat_08` and `M_repeat_01`–`M_repeat_08` are independent repeat measurements of those same annual counts.

The three observation designs use the data as follows:

| `method` | Observations |
|---|---|
| `admissions_only` | The first C and M columns: 16 values |
| `admissions_repeated` | The first and repeat C and M columns: 32 values |
| `admissions_plus_wbe` | The first C and M columns plus L: 32 values |

All designs share the first administrative observations within a replicate. Independent Gaussian noise has standard deviation 5% of its generating mean. The 32 primary means and standard deviations are in `numerical_results.csv`, block `wbe_generating_means`; the repeated administrative means and standard deviations equal the first 16. The primary NumPy default generator uses seed 2601465 and the repeat generator seed 2601466, with NumPy 2.3.3. Each replicate draws 32 primary normal errors and 16 repeat normal errors.

In `wbe_recovery_runs.csv`, `experiment=known_rates` has 100 replicates × 3 methods. It estimates I1(0) and I3(0), with beta2 fixed; `beta2_per_year` is therefore empty. `experiment=joint_rate` has 50 replicates × 3 methods and also estimates beta2. It uses replicates 0–49 of the same observations. There are 450 fitting results but only 100 distinct noise realizations.

`I1_initial_persons` and `I3_initial_persons` are the estimated initial counts. `beta2_per_year` is the estimated rate in the joint experiment. `success` records whether the numerical optimizer met a termination criterion, and `nfev` is its number of function evaluations. Initial-state factors have bounds [0.05,10]; the joint beta2 factor has bounds [0.2,5]. Bounds are relative to the generating values. The true beta2 is 0.029014 year^-1; the true initial states are recorded in the `wbe_generating` vector.

For an estimate x_hat and generating value x, the signed relative error is 100 × (x_hat/x − 1). RMSE is the square root of the mean squared signed error, MARE is the median absolute signed error, and the reported quantiles use linear interpolation. These calculations reproduce Figure 2A and Tables S8–S9. Figure 2B propagates each recovered initial state through the deterministic model before taking pointwise quantiles. Its bands describe variation across the specified noise realizations.

The local generating population is 100,000 persons. The population total can vary during fitting because the two estimated states vary independently and the other four states stay fixed. The known emission coefficients are eta1=1 and eta3=4 mg/(person year), kappa=365.25 year^-1, W(0)=0 mg, and the relative Gaussian noise scale is 0.05. Numerical propagation uses LSODA with relative and absolute tolerances 2×10^-8. Other rates follow the reference set; the local recruitment is supplied in `wbe_settings`.

## Data sources

The administrative source for each reporting year is linked in `administrative_observations.csv`. These links identify the NNCC report or its public institutional/report reproduction.

The demographic model parameters Lambda=400 (10^4 persons/year), mu=0.007 year^-1, and mu_d=0.025 year^-1 follow Table 3 of H. Tang, M. Li, X. Yan, Z. Lu, and Z. Jia, *Modeling the dynamics of drug spreading in China*, International Journal of Environmental Research and Public Health 18 (2021), 288, [doi:10.3390/ijerph18010288](https://doi.org/10.3390/ijerph18010288).

The initial population scale N(0)=100403 (10^4 persons) follows the population section of the [National Bureau of Statistics of China, Statistical communiqué on the 2012 national economic and social development](https://www.stats.gov.cn/english/NewsEvents/201302/t20130222_26962.html), published 22 February 2013. The source population totals are for the end of 2012. Their use as a beginning-of-2012 scale, the compartmental partition, and the model's demographic balance are specified assumptions. The hypothetical catchment preserves those initial proportions and uses the corresponding scaled recruitment.

## Revision note

The 20 October 2026 update revises this README to match the diagnostic scope and updated article title. All five CSV files are byte-for-byte unchanged from repository snapshot `8667ff149e025a42b5c0413fb7f0e5d5f4ad0c2b`. Retained column and case names preserve compatibility with that snapshot. The repository contains source data and numerical outputs; it does not include executable calculation scripts. The procedures and numerical settings are reported in the manuscript.
