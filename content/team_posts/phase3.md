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

## Updates/Modifcations to Data Model and ML

Since the second phase, we've added a lag term `asylum_lag1` to our initial model. After doing this, we saw a moderate increase in R^2 predictions (from 43% to 49%). We added standard scaler fitting to training data, applied to both train and test. Now trains on X_train_scaled and predicts on X_test_scaled. 

### Data Model Changes
In Phase II, our data model was very general and just focused on the main entities we thought TERRA would need like countries, climate indicators, climate events, displacement records, risk assessments, policies, NGOs, and relationship between NGOs and countries which gave us a good starting structure for the most part it was a concept for our database design. 

For Phase III, we updated the model so it better matches our Streamlit app, REST API routes, and machine learning work. One major change was replacing the separate climate_indicator and displacement tables with a broader country_year_data table which would help store the GDP per capita, unemployment rate, population, urban percentage, asylum applications, average temperature, heatwave days, precipitation, dry days, and evapotranspiration. Also helped make the model easier to connect to the prediction model because the ML features are stored in one place by country and year.

Something that was also changed was the app focused tables to help user personas. The roles and users tables support the personas and then the saved_views and saved_view_country tables support Gabriel’s saved comparison views while the watchlist table helps Mohammed’s follow countries information. Furthermore, country_summary_report table helps Diana’s exported country summaries.

## List of Tables

### Sourced or Generated Data

Our ML data originates from public API's (Open-Metro, Eurostat, and WorldBank) collected and merged into a single dataset of 378 rows covering EU member states from 2010-2023.For Phase 3, we migrated this data into an SQL database, storing training data, and generated model artifacts into one database. This means the app will load the pre-trained weights rather than regenerating weights every run cycle. 

### Features of the ML
Our second ML model is a logistic regression classification model that predicts whether a country falls in the top 30% for asylum applications. First it takes the cleaned data from merged_data.csv. Then it creates a binary target variable, where 1 is for high asylum applications and 0 is for not high. After it is trained and scaled properly it outputs the accuracy, a classification report, and how each feature was weighted. We chose these features because it would be helpful for persona 2 (Diana Willis) because it would help her quickly identify countries that she can help.

### Issues with ML Exploration
There were some issues with creating the second ML model. At first we created a logistic regression classification model that predicted a climate stress score and used that score to calculate whether the risk in the area was low, medium, or high. However, the output said that the accuracy was 92%, and it turned out that there was label leakage. Because of that we decided to make another logistic regression classification model that tries to predict whether a country falls in the top 30% for asylum applications by using the data we cleaned, as well as providing a coefficent table which tells you which categories mattered the most for the prediction. The accuracy is 77%, which while much better than the first model, is still a bit lower than we would like.

## Pages

### Landing Page:
Main TERRA home page where users choose which persona to enter the app as.
![Landing](LandingPage.png)

### Diana Home:
This shows Diana’s Humanitarian Coordinator dashboard including risk priority, NGO support, and countries that may need humanitarian attention.
![DianaHome](DianaHome.png)

### Diana Add NGO:
This screen allows Diana to add a new NGO record, including organization name, country, founding year, focus area, and website.
![DianaNGO](DianaNGO.png)

### Diana Priority Countries:
This page ranks countries by risk level, displacement pressure, NGO coverage, and recommended action.
![DianaPriority](DianaPriority.png)

### Gabriel Home:
Gabriel’s Policy Analyst dashboard focuses on country comparison, risk classification, prediction model results, saved views, and reports.
![GabrielHome](GabrielHome.png)

### Prediction Model Screenshot and Features:
This page shows the implemented ML functionality. In the screenshot, the model predicts 140k asylum applications for the entered France 2019 values.

For Model 1, we used a linear regression model to predict asylum applications. The features in our model are year, gdp_per_capita, unemployment_rate, population, urban_pct, temp_mean, heatwave_days, precip_total, precip_days_heavy, dry_days, and evapotrans_total. 

We chose these features because we are trying to see how climate risk and country conditions relate to asylum trends across Europe. The economic and demographic features like GDP, unemployment rate, population, and urban percent would help represent a country’s overall context but the climate variables like average temperature, heatwave days, precipitation, heavy precipitation days, dry days, and evapotranspiration, help capture environmental stressors that may be connected to displacement pressure give us the broader conditions of a countries weather events. 
![Model](Model1Page.png)

## Proposed REST API Matrix:
Proposed REST API routes for the project including GET, POST, PUT, and DELETE routes. The routes connect back to our app pages and user stories including country comparison, NGO management, risk classification, etc.
![RESTAPI](RestAPI.png)

