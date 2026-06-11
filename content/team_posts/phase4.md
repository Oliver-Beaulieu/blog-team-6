---
title: "Project - Phase IV: TERRA"
date: 2026-06-11
draft: false
description: "Tracking European Climate Risk & Refugee Asylum — our Phase 4 final report"
slug: "phase4post"
tags: ["project", "phase4", "climate", "ml"]
authors:
  - "yadiel_cruz"
  - "hamza"
  - "oliver"
  - "james"
showAuthorsBadges: false
showTableOfContents: true
---
# TERRA: Tracking European Climate Risk & Refugee Asylum

<!--
OUTLINE — Phase 4 (final) team post. Fill in each section below.
Requirements this post must cover:
  1. Fundamental understanding of our ML models + how they fit the software architecture
  2. Verification of ALL model assumptions + predictive checks (work NOT exposed in the web app)
  3. Final software architecture
  4. Final version of the database model
Keep the same casual-but-technical voice as phases 2 & 3. Drop in screenshots with ![alt](file.png).
-->


## Overview / What Changed Since Phase 3

<!--
- 2-3 sentences framing this as the final phase.
- Quick recap: TERRA predicts asylum applications (Model 1, linear regression) and
  flags top-30% asylum-pressure countries (Model 2, logistic regression).
- Headline changes since Phase 3 (final tweaks to data model, finished architecture,
  added the assumption/diagnostic work that lives outside the app).
-->


## Software Architecture

The system is a straightforward four-layer pipeline, and everything below the front end runs in Docker (docker-compose):

Our web-app is built in a four-part pipeline, with evertyhing running in docker:

  1. Data ingestion: pulls raw data from the World Bank (GDP, unemployment, population, urbanization), Eurostat (asylum applications), and Open-Meteo (climate variables). These are stored in as `datasets/raw/*.csv`.
  2. Cleaning & merge: notebooks `01_data_ingestion` and `02_data_cleaning` combine country codes and years and produce a single
  `datasets/processed/merged_data.csv` (one row per country-year).
  3. SQL database (MySQL, terra_db): the merged data lands in `country_year_data`, and the rest of the schema (countries, risk assessments, NGOs, personas, savedviews, etc.) hangs off of it. The trained model parameters are also stored here too.
  4. REST API (Flask): a set of blueprints (country_bp, ngo_bp, risk_bp, prediction_bp, terra_model_bp, view_bp, policy_bp, user_bp, climate_bp) registered in `rest_entry.py`.
  5. Streamlit front end: persona pages that talk to the API over HTTP (http://web-api:4000) and never touch the database directly.


## Final Database Model

<!--
- State that this is the FINAL version and call out the diff vs Phase 3.
- ER diagram + relational model screenshots: ![GlobalER](GlobalER.png) / ![DDL](GlobalDDL.png)
- Note any last changes: new columns (e.g. asylum_lag1 if persisted), constraints,
  the country_year_data consolidation, the persona-support tables (saved_views,
  watchlist, country_summary_report, ngo/ngo_country).
- Paste the final DDL in a ```sql block if it changed from Phase 2/3.
-->



## ML Models — Fundamental Understanding

Model 1 - Linear Regression (predict asylum_applications)

Linear regression fits a straight-line relationship between a set of input features. It chooses coefficients that minimize squared error. In our case, asylum applications is the target; a count that that can be from a few hundred to thousands, so regression is better than a classification model here.

- Certain features ended up being pruned with VIF: percip_total (VIF ~= 23.6), evapotrans_total (VIF ~= 13.2), population (VIF ~= 7986) and urban_pct (VIF ~= 338), since they proved to be more of redundant aggregates that were collinear with country identity. 

- We decided to exclude calendar year as a feature since the scaler is fit on 2010-2018, so future year made our predictions wildly inaccurate. We only used year to split train/test.

Model 2 - Multivariate Linear Regression

Our multivariate linear regression model predicts three climate targets: heatwave_days, precip_days_heavy, and dry_days from gdp_per_capita, unemployment_rate, population, urban_pct, and asylum_applications. 

## Model Assumptions & Predictive Checks (not shown in the web app)



<!--
THIS IS THE KEY NEW DELIVERABLE. Document the validation we did behind the scenes.
For each check: what it tests, how we tested it, what we found, and whether it passed.
Include the diagnostic plots (these are the ones NOT surfaced in the Streamlit app).

### Linear Regression (Model 1) assumptions
- Linearity            → residuals-vs-fitted plot. ![ResidVsFitted](resid_vs_fitted.png)
- Independence of errors→ relevant w/ time-series + lag term; Durbin-Watson note.
- Homoscedasticity     → spread of residuals; scale-location plot.
- Normality of residuals→ Q-Q plot / histogram. ![QQ](qq_plot.png)
- No multicollinearity → VIF table for the features. ![VIF](vif_table.png)
- Predictive check     → cross-validation on the small (378-row) dataset; actual-vs-predicted
                         on held-out years; MAE / R^2 restated.

### Logistic Regression (Model 2) assumptions
- Linearity of the logit, independence, no perfect separation, adequate sample size.
- No multicollinearity → VIF again.
- Predictive checks     → confusion matrix, classification report, ROC curve / AUC,
                          calibration (predicted prob vs observed frequency).
  ![Confusion](confusion_matrix.png) ![ROC](roc_curve.png)

### What the diagnostics told us
- Honest summary: which assumptions held, which were violated, and how that caps confidence
  (e.g. geopolitical shocks like Syria 2015 / Ukraine 2022 break independence/linearity;
  Sweden post-2015 overestimation; small-n limits).
-->

## Final App Pages / Screenshots

<!--
- Show the finished app. Reuse/refresh persona screenshots from Phase 3 if pages changed.
- Landing, Gabriel (Policy Analyst), Diana (Humanitarian Coordinator), Mohammed (Student),
  Prediction page.
-->

## Reflection / What's Next

<!--
- What we'd improve with more time/data (more rows, event features for geopolitical shocks,
  regularization, richer calibration).
- Wrap-up of the project arc across all four phases.
-->