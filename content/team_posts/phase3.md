---
title: "Project - Phase III: TERRA"
date: 2026-06-04
draft: false
description: "Tracking European Climate Risk & Refugee Asylum — our Phase 3 design report"
slug: "phase3post"
tags: ["project", "phase3", "climate"]
authors:
  - "yadiel_cruz"
  - "hamza"
  - "oliver"
  - "james"
showAuthorsBadges: false
showTableOfContents: true
---
# TERRA: Tracking European Climate Risk & Refugee Asylum

Everything under here is a work in progress

### Updates/Modifcations to Data Model

Since the second phase, we've added a lag term `asylum_lag1` to our initial model. After doing this, we saw a moderate increase in R^2 predictions (from 43% to 49%). We added standard scaler fitting to training data, applied to both train and test. Now trains on X_train_scaled and predicts on X_test_scaled. 

### List of Tables

### Sourced or Generated Data

Our ML data originates from public API's (Open-Metro, Eurostat, and WorldBank) collected and merged into a single dataset of 378 rows covering EU member states from 2010-2023.For Phase 3, we migrated this data into an SQL database, storing training data, and generated model artifacts into one database. This means the app will load the pre-trained weights rather than regenerating weights every run cycle. 

### Features of the ML


### Issues with ML Exploration
There were some issues with creating the second ML model. At first we created a logistic regression classification model that predicted a climate stress score and used that score to calculate whether the risk in the area was low, medium, or high. However, the output said that the accuracy was 92%, and it turned out that there was label leakage. Because of that we decided to make another logistic regression classification model that tries to predict whether a country falls in the top 30% for asylum applications by using the data we cleaned, as well as providing a coefficent table which tells you which categories mattered the most for the prediction. The accuracy is 77%, which while much better than the first model, is still a bit lower than we would like.

### REST API Matrix

### Screenshots

