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

## Overview / What Changed Since Phase 3

Phase 4 represents the final stage of TERRA in which our team focused on making this into a more complete full-stack application. Since Phase III, we have continued changing the connection between our data, machine learning models, database, REST API, and Streamlit front end so the app could better support the persona's that we came up with.

In our app we use two machine learning models to help users understand climate risk and displacement pressure across Europe. Model 1 is a linear regression model that predicts asylum applications while Model 2 is a multi linear regression model that predicts a countries heatwave days, heavy precipitation days, and dry days.

Since Phase III, our biggest changes were finalizing the software architecture and also cleaning up the database model to then improve how the app connects through Docker. We also continued improving the persona pages, API routes, and database structure so that TERRA full works all around and every UI is complete as in phase III only 1/3 of the pages were complete.


## Software Architecture

The system is a straightforward four-layer pipeline, and everything below the front end runs in Docker (docker-compose):

Our web-app is built in a four-part pipeline, with evertyhing running in docker:

  1. Data ingestion: Raw data is used from the World Bank (GDP, unemployment, population, urbanization), Eurostat (asylum applications), and Open-Meteo (climate variables). These were then stored in `datasets/raw/*.csv`.
  2. Cleaning & merge: The notebooks `01_data_ingestion` and `02_data_cleaning` combined country codes and years to then create `datasets/processed/merged_data.csv` (one row per country-year).
  3. SQL database: The MYSQL database which is terra_db stores the cleaned data inside of `country_year_data`, and the rest of the schema (countries, risk assessments, NGOs, personas, savedviews, etc all connect to this structure as a whole. Adittionally, the trained model parameters are also stored in the database.
  4. REST API (Flask): The Flask REST API works as the layer between the database and the front end and is what helps connect it. This section is organized into blueprints (country_bp, ngo_bp, risk_bp, prediction_bp, terra_model_bp, view_bp, policy_bp, user_bp, climate_bp) that are all reffered to in `rest_entry.py`.
  5. Streamlit front end: The front end contains all of our unqiue pages for different personas which communicate with the API (http://web-api:4000).The front end doesnt connect directly to the database which helps make the app more organized easy to work with.


## Final Database Model 
### Description 
Our final model is organized around country and country-year data because TERRA mostly focuses on comparing climate risk, asylum trends, policies, and humanitarian support across different countries. The country table stores basic country information but the country_year_data stores yearly data from our csv like asylum applications, GDP, unemployment, population, urbanization, and climate-related variables which we then were able to connect these to risk, policy, and humanitarian information.

The climate_event table stores climate events connected to countries, while risk_assessment stores risk scores and risk levels. The policies and policy_flag tables help support the Policy Analyst by putting gov information and allows to place policy flags. The WorldNGOs, Projects, and Donors tables support the Humanitarian Coordinator by storing information about organizations. The database also includes user tables which tables help make TERRA come off as interactive.

### TERRA MYSql
```sql
CREATE DATABASE IF NOT EXISTS terra_db;

USE terra_db;

-- TERRA Database

-- 1. ROLES - Defines user roles in the app.
CREATE TABLE IF NOT EXISTS roles (
    role_id INT NOT NULL AUTO_INCREMENT,
    role_name VARCHAR(50) NOT NULL UNIQUE,
    description VARCHAR(255),
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (role_id)
);

-- 2. USERS - App users.
CREATE TABLE IF NOT EXISTS users (
    user_id INT NOT NULL AUTO_INCREMENT,
    role_id INT NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE,
    display_name VARCHAR(100),
    password_hash VARCHAR(255),
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,
    created_by INT,
    updated_by INT,
    PRIMARY KEY (user_id),
    FOREIGN KEY (role_id)     REFERENCES roles(role_id)      ON DELETE RESTRICT  ON UPDATE CASCADE,
    FOREIGN KEY (created_by)  REFERENCES users(user_id)      ON DELETE SET NULL  ON UPDATE CASCADE,
    FOREIGN KEY (updated_by)  REFERENCES users(user_id)      ON DELETE SET NULL  ON UPDATE CASCADE
);

-- 3. COUNTRY - Central entity
CREATE TABLE IF NOT EXISTS country (
    country_id INT NOT NULL AUTO_INCREMENT,
    country_name VARCHAR(100) NOT NULL,
    country_code CHAR(2) NOT NULL UNIQUE,
    region VARCHAR(100),
    population INT,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,
    created_by INT,
    updated_by INT,
    PRIMARY KEY (country_id),
    FOREIGN KEY (created_by)  REFERENCES users(user_id)      ON DELETE SET NULL  ON UPDATE CASCADE,
    FOREIGN KEY (updated_by)  REFERENCES users(user_id)      ON DELETE SET NULL  ON UPDATE CASCADE
);

-- 4. COUNTRY_YEAR_DATA
-- Cleaned yearly data used by the app and ML models.
CREATE TABLE IF NOT EXISTS country_year_data (
    data_id INT NOT NULL AUTO_INCREMENT,
    country_id INT NOT NULL,
    year INT NOT NULL,

    gdp_per_capita DECIMAL(12,2),
    unemployment_rate DECIMAL(5,2),
    population INT,
    urban_pct DECIMAL(6,3),

    asylum_applications INT,

    temp_mean DECIMAL(5,2),
    heatwave_days INT,
    precip_total DECIMAL(10,2),
    precip_days_heavy INT,
    dry_days INT,
    evapotrans_total DECIMAL(10,2),

    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,
    created_by INT,
    updated_by INT,

    PRIMARY KEY (data_id),
    UNIQUE KEY uq_country_year (country_id, year),
    FOREIGN KEY (country_id)  REFERENCES country(country_id) ON DELETE CASCADE   ON UPDATE CASCADE,
    FOREIGN KEY (created_by)  REFERENCES users(user_id)      ON DELETE SET NULL  ON UPDATE CASCADE,
    FOREIGN KEY (updated_by)  REFERENCES users(user_id)      ON DELETE SET NULL  ON UPDATE CASCADE
);

-- 5. CLIMATE_EVENT
CREATE TABLE IF NOT EXISTS climate_event (
    event_id INT NOT NULL AUTO_INCREMENT,
    country_id INT NOT NULL,
    event_type VARCHAR(100),
    event_date DATE,
    severity VARCHAR(50),
    event_description TEXT,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,
    created_by INT,
    updated_by INT,
    PRIMARY KEY (event_id),
    FOREIGN KEY (country_id)  REFERENCES country(country_id) ON DELETE CASCADE   ON UPDATE CASCADE,
    FOREIGN KEY (created_by)  REFERENCES users(user_id)      ON DELETE SET NULL  ON UPDATE CASCADE,
    FOREIGN KEY (updated_by)  REFERENCES users(user_id)      ON DELETE SET NULL  ON UPDATE CASCADE
);

-- 6. RISK_ASSESSMENT - Country risk scores.
CREATE TABLE IF NOT EXISTS risk_assessment (
    risk_id INT NOT NULL AUTO_INCREMENT,
    country_id INT NOT NULL,
    year INT NOT NULL,
    risk_score DECIMAL(5,2),
    risk_level VARCHAR(50),
    label_method VARCHAR(150),
    notes TEXT,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,
    created_by INT,
    updated_by INT,
    PRIMARY KEY (risk_id),
    UNIQUE KEY uq_risk_country_year (country_id, year),
    FOREIGN KEY (country_id)  REFERENCES country(country_id) ON DELETE CASCADE   ON UPDATE CASCADE,
    FOREIGN KEY (created_by)  REFERENCES users(user_id)      ON DELETE SET NULL  ON UPDATE CASCADE,
    FOREIGN KEY (updated_by)  REFERENCES users(user_id)      ON DELETE SET NULL  ON UPDATE CASCADE
);

-- 7. POLICIES - Gabriel's policy analysis work.
CREATE TABLE IF NOT EXISTS policies (
    policy_id INT NOT NULL AUTO_INCREMENT,
    country_id INT NOT NULL,
    name VARCHAR(150),
    policy_type VARCHAR(100),
    status VARCHAR(50),
    description TEXT,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,
    created_by INT,
    updated_by INT,
    PRIMARY KEY (policy_id),
    FOREIGN KEY (country_id)  REFERENCES country(country_id) ON DELETE CASCADE   ON UPDATE CASCADE,
    FOREIGN KEY (created_by)  REFERENCES users(user_id)      ON DELETE SET NULL  ON UPDATE CASCADE,
    FOREIGN KEY (updated_by)  REFERENCES users(user_id)      ON DELETE SET NULL  ON UPDATE CASCADE
);

-- 8. POLICY_FLAG - Supports Gabriel flagging countries for policy review.
CREATE TABLE IF NOT EXISTS policy_flag (
    flag_id INT NOT NULL AUTO_INCREMENT,
    country_id INT NOT NULL,
    user_id INT NOT NULL,
    flag_status VARCHAR(50) NOT NULL,
    flag_note TEXT,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (flag_id),
    FOREIGN KEY (country_id)  REFERENCES country(country_id) ON DELETE CASCADE   ON UPDATE CASCADE,
    FOREIGN KEY (user_id)     REFERENCES users(user_id)      ON DELETE CASCADE   ON UPDATE CASCADE
);

-- 9. SAVED_VIEWS - Saved country comparison views for Gabriel.
CREATE TABLE IF NOT EXISTS saved_views (
    view_id INT NOT NULL AUTO_INCREMENT,
    user_id INT NOT NULL,
    view_name VARCHAR(150) NOT NULL,
    year_from INT,
    year_to INT,
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (view_id),
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE ON UPDATE CASCADE
);

-- 10. SAVED_VIEW_COUNTRY - Table for saved views and countries.
CREATE TABLE IF NOT EXISTS saved_view_country (
    view_id INT NOT NULL,
    country_id INT NOT NULL,
    PRIMARY KEY (view_id, country_id),
    FOREIGN KEY (view_id)     REFERENCES saved_views(view_id) ON DELETE CASCADE  ON UPDATE CASCADE,
    FOREIGN KEY (country_id)  REFERENCES country(country_id)  ON DELETE CASCADE  ON UPDATE CASCADE
);

-- 11. WATCHLIST - Countries Mohammed wants to follow.
CREATE TABLE IF NOT EXISTS watchlist (
    watchlist_id INT NOT NULL AUTO_INCREMENT,
    user_id INT NOT NULL,
    country_id INT NOT NULL,
    added_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (watchlist_id),
    UNIQUE KEY uq_watchlist_user_country (user_id, country_id),
    FOREIGN KEY (user_id)     REFERENCES users(user_id)      ON DELETE CASCADE   ON UPDATE CASCADE,
    FOREIGN KEY (country_id)  REFERENCES country(country_id) ON DELETE CASCADE   ON UPDATE CASCADE
);

-- 12. COUNTRY_SUMMARY_REPORT - Stores exported country summaries for Diana.
CREATE TABLE IF NOT EXISTS country_summary_report (
    report_id INT NOT NULL AUTO_INCREMENT,
    country_id INT NOT NULL,
    user_id INT NOT NULL,
    report_title VARCHAR(150),
    report_text TEXT,
    export_format VARCHAR(20),
    generated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (report_id),
    FOREIGN KEY (country_id)  REFERENCES country(country_id) ON DELETE CASCADE   ON UPDATE CASCADE,
    FOREIGN KEY (user_id)     REFERENCES users(user_id)      ON DELETE CASCADE   ON UPDATE CASCADE
);

-- 13. NGO tables - Used by Diana's humanitarian coordinator routes.
CREATE TABLE IF NOT EXISTS WorldNGOs (
    NGO_ID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(255) NOT NULL,
    Country VARCHAR(100) NOT NULL,
    Founding_Year INTEGER,
    Focus_Area VARCHAR(100),
    Website VARCHAR(255),
    Notes TEXT
);

CREATE TABLE IF NOT EXISTS Projects (
    Project_ID INT AUTO_INCREMENT PRIMARY KEY,
    Project_Name VARCHAR(255) NOT NULL,
    Focus_Area VARCHAR(100),
    Budget DECIMAL(15, 2),
    NGO_ID INT,
    Start_Date DATE,
    End_Date DATE,
    FOREIGN KEY (NGO_ID) REFERENCES WorldNGOs(NGO_ID)
);

CREATE TABLE IF NOT EXISTS Donors (
    Donor_ID INT AUTO_INCREMENT PRIMARY KEY,
    Donor_Name VARCHAR(255) NOT NULL,
    Donor_Type ENUM('Individual', 'Organization') NOT NULL,
    Donation_Amount DECIMAL(15, 2),
    NGO_ID INT,
    FOREIGN KEY (NGO_ID) REFERENCES WorldNGOs(NGO_ID)
);

-- 14. MODEL 1 - Asylum applications model parameters.
CREATE TABLE IF NOT EXISTS model1_params (
    sequence_number INT,
    feature_names   TEXT,
    beta_vals       TEXT
);

-- 15. MODEL 1 - StandardScaler parameters.
CREATE TABLE IF NOT EXISTS model1_scaler (
    sequence_number INT,
    feature_means   TEXT,
    feature_stds    TEXT
);

-- 16. MODEL 2 - Climate variables model parameters.
CREATE TABLE IF NOT EXISTS model2_params (
    sequence_number       INT,
    target_names          TEXT,
    numeric_feature_names TEXT,
    intercepts            TEXT,
    coef_matrix           TEXT
);

-- 17. MODEL 2 - StandardScaler parameters.
CREATE TABLE IF NOT EXISTS model2_scaler (
    sequence_number INT,
    feature_means   TEXT,
    feature_stds    TEXT
);

-- 18. MODEL 2 - OneHotEncoder categories.
CREATE TABLE IF NOT EXISTS model2_encoder (
    sequence_number INT,
    categories      TEXT
);
```


## ML Models — Fundamental Understanding

Model 1 - Linear Regression (predict asylum_applications)

Linear regression fits a straight-line relationship between a set of input features. It chooses coefficients that minimize squared error. In our case, asylum applications is the target; a count that that can be from a few hundred to thousands, so regression is better than a classification model here.

- Certain features ended up being pruned with VIF: percip_total (VIF ~= 23.6), evapotrans_total (VIF ~= 13.2), population (VIF ~= 7986) and urban_pct (VIF ~= 338), since they proved to be redundant aggregates that were collinear with a countries identity. 

- We decided to exclude calendar year as a feature since the scaler is fit on 2010-2018, so future years made our predictions wildly inaccurate. We only used year to split train/test.


Model 2 - Multivariate Linear Regression

Multivariate linear regression expands on a regular linear model to predict multiple outcomes at the same time, in our case three climate targets: heatwave_days, precip_days_heavy, and dry_days from gdp_per_capita, unemployment_rate, population, urban_pct, and asylum_applications. Each target variable shares the same set of predictors, which allows our model to capture correlated climate patterns based on heatwaves, heavy preciptation, and dry days.

- The model standardizes the numeric features (GDP per capita, unemployment rate, population, urban_pct, asylum_applications) and one hot encodes categorical features (country code). This makes it so that it is comparable across scales and prevents bias towards high magnitude variables.
- We decided to not use year during training to prevent leakage as the scaler was fit on data from 2010-2018. We only used it during the train/test splitting to preserve chronological integrity.

## Model Assumptions & Predictive Checks

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

- Linearity: Residual plots were generated for all three targets (heatwave_days, precip_days_heavy, dry_days). Residuals showed generally random scatter around zero, meaning that lineraity holds.
- Independance of errors: Since our model uses a random 80/20 split, residual independence is assumed, as no explicit autocorrelation tests are performed.
- Homoscedasticity: Plots mostly showed constant variance. However, there was a slight funneling for dry days, indicating mild heteroscedasticity.
- No multicollinearity: Our model uses standardized numeric features and one hot encoded country codes. Feature magnitudes are controlled through scaling.

Predictive Checks (Model 2)

- Train/test split: Our model uses a random 80/20 split via train_test_split, meaning predictive performance is evaluated on a randomly held out subset.
- Performance: Predictive checks included R², RMSE, and MAE per target. Average R² was 0.72 across the three targets. RMSE and MAE remained consistent between folds and aligned with test‑set performance and confirming model stability.
- Actual vs predicted: Comparisions between predicted and actual values show that the model captures general trends for all three targets pretty accurately, although heavy preciptation days has slightly wider dispersion.

### What the diagnostics told us
- Honest summary: which assumptions held, which were violated, and how that caps confidence
  (e.g. geopolitical shocks like Syria 2015 / Ukraine 2022 break independence/linearity;
  Sweden post-2015 overestimation; small-n limits).
-->

## Final App Screens

### Landing Page
Main TERRA home page where users choose which persona to enter the app as.
![Landing Page](LandingFinal.png)

### Policy Analyst Interface
This page shows the final Policy Analyst experience, focused on country comparison, policy insights, and risk analysis.
![Policy Analyst](PolicyFinal.png)

### Humanitarian Coordinator Interface
This page shows the final Humanitarian Coordinator experience, focused on NGOs, country needs, and humanitarian decision making.
![Humanitarian Coordinator](HumanitarianFinal.png)

### Student Interface
This page shows the student home page designed to help explore climate and displacement information in an accessible way.
![Student Interface](StudentFinal.png)

### Model 1 Prediction Page
This page shows our first machine learning model, which predicts asylum applications based on country-year features.
![Model 1](HumanModel1.png)

### Risk / Model 1 Page
This page shows a risk-focused model output or supporting prediction interface used in the app.
![Risk Model](RiskModel1.png)

### Model 2 Classification Page
This page shows our second machine learning model, which flags whether a country falls into the top 30% for asylum pressure.
![Model 2](Model2.png)

## Reflection / What's Next

Across all four phases our app, TERRA grew from an initial idea into a full-stack application which connects climate data, asylum trends, policy information, humanitarian support, and machine learning. We first started by figuring out a problem and developing who could benefit from this. Based on that we then built our data model, created the database and API structure, and connected everything through the Streamlit app. This process as a whole helped us understand how much planning is needed to make the data, backend, front end, and models work together in an app.

With more time, we believe that we would have improved TERRA by adding more years of data, and more detailed climate event features. We would also want to include major events or shocks that may affect asylum applications, since we realized that displacement isn't only caused by climate or economic conditions. Overall, TERRA gave us a strong foundation but in future versions and with more time it could become more accurate, and more useful for policy and humanitarian decision-making.