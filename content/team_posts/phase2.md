---
title: "Project - Phase II: TERRA"
date: 2026-05-24
draft: false
description: "Tracking European Climate Risk & Refugee Asylum — our Phase 2 design report"
slug: "phase2post"
tags: ["project", "phase2", "climate"]
authors:
  - "yadiel_cruz"
  - "hamza"
  - "oliver"
  - "james"
showAuthorsBadges: false
showTableOfContents: true
---
# TERRA: Tracking European Climate Risk & Refugee Asylum


## ER Diagrams and Relational Models

### Persona 1 - Policy Analyst
![Policy1](GabrielER.png)

### Localized Data Model Description

The localized data model for our Policy Analyst focuses on country comparison, yearly country data, risk assessments, government policies, saved views, and policy flags. The country entity is in the middle since his analysis depends on looking at specific EU countries and as a result of this country then connects to yearly data, risk assessments, policies, saved views, and policy flags which all helps the analyst review country conditions, track policy information, and save countries for later comparison.

### Cardinality

Country has one to many relationships with many entities which means that one country could have multiple yearly records, risk assessments, policies, and flags. Users also has a one to many relationship with saved_view because one user can create many saved views. Country_year_data in this case could be seen as a weak entity because it depends on country and year



### Persona 2 - Humanitarian Coordinator
![Humanitarian](DianaER.png)

### Localized Data Model Description

The localized data model for our Humanitarian Coordinator mostly focuses on country risk, yearly country data, NGO support, and country summary reports. Once again, country is in the center because Diana needs to identify which EU countries may need humanitarian attention. In her model NGO is here because Diana needs to understand which organizations are operating in different countries. Finally, Country_summary_report is included as it helps report summaries of country conditions, risk, and humanitarian needs which she may need.

### Cardinality

Country here still has a one to many relationship with different entities while user has a one to many relationship with country_summary_report because one user can generate many reports. NGO and country have a many to many relationship because one NGO can operate in many countries and one country can have many NGOs. 


### Persona 3 - Climate-Displaced Student
![Student](MohammedER.png)

### Localized Data Model Description

The localized data model for our Climate-Displaced Student is quite simple as it focuses on watched countries, yearly country data, climate events, and risk assessments. Similarly, to our other personas country is in the center because he is using the app to learn about specific EU countries. In his model climate_event and risk_assessment would help him understand what climate issues are happening and how risky a country may be.

### Cardinality

Users and countries have a many to many relationship through watches since one user can watch many countries and one country can be watched by many users. Countries have one to many relationships here with country_year_data, climate_event, and risk_assessment but if you look at country_year_data is treated as a weak entity because it depends on country and year.

### Global
![Global1](GlobalER.png)
![Global](GlobalDDL.png)

## Global Data Model DDL

```sql
CREATE DATABASE IF NOT EXISTS terra_db;

USE terra_db;

-- TERRA Database
-- 1. ROLES
CREATE TABLE IF NOT EXISTS roles (
    role_id INT NOT NULL AUTO_INCREMENT,
    role_name VARCHAR(50) NOT NULL UNIQUE,
    description VARCHAR(255),
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,
    PRIMARY KEY (role_id)
);

-- 2. USERS
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
    FOREIGN KEY (role_id) REFERENCES roles(role_id),
    FOREIGN KEY (created_by) REFERENCES users(user_id),
    FOREIGN KEY (updated_by) REFERENCES users(user_id)
);

-- 3. COUNTRY
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
    FOREIGN KEY (created_by) REFERENCES users(user_id),
    FOREIGN KEY (updated_by) REFERENCES users(user_id)
);

-- 4. COUNTRY_YEAR_DATA
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
    FOREIGN KEY (country_id) REFERENCES country(country_id),
    FOREIGN KEY (created_by) REFERENCES users(user_id),
    FOREIGN KEY (updated_by) REFERENCES users(user_id)
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
    FOREIGN KEY (country_id) REFERENCES country(country_id),
    FOREIGN KEY (created_by) REFERENCES users(user_id),
    FOREIGN KEY (updated_by) REFERENCES users(user_id)
);

-- 6. RISK_ASSESSMENT
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
    FOREIGN KEY (country_id) REFERENCES country(country_id),
    FOREIGN KEY (created_by) REFERENCES users(user_id),
    FOREIGN KEY (updated_by) REFERENCES users(user_id)
);

-- 7. POLICIES
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
    FOREIGN KEY (country_id) REFERENCES country(country_id),
    FOREIGN KEY (created_by) REFERENCES users(user_id),
    FOREIGN KEY (updated_by) REFERENCES users(user_id)
);

-- 8. POLICY_FLAG
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
    FOREIGN KEY (country_id) REFERENCES country(country_id),
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);

-- 9. SAVED_VIEWS
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
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);

-- 10. SAVED_VIEW_COUNTRY
CREATE TABLE IF NOT EXISTS saved_view_country (
    view_id INT NOT NULL,
    country_id INT NOT NULL,
    PRIMARY KEY (view_id, country_id),
    FOREIGN KEY (view_id) REFERENCES saved_views(view_id),
    FOREIGN KEY (country_id) REFERENCES country(country_id)
);

-- 11. NGO
CREATE TABLE IF NOT EXISTS ngo (
    ngo_id INT NOT NULL AUTO_INCREMENT,
    ngo_name VARCHAR(150) NOT NULL,
    focus_area VARCHAR(100),
    contact_email VARCHAR(150),
    website VARCHAR(200),
    created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,
    created_by INT,
    updated_by INT,
    PRIMARY KEY (ngo_id),
    FOREIGN KEY (created_by) REFERENCES users(user_id),
    FOREIGN KEY (updated_by) REFERENCES users(user_id)
);

-- 12. NGO_COUNTRY
CREATE TABLE IF NOT EXISTS ngo_country (
    ngo_id INT NOT NULL,
    country_id INT NOT NULL,
    operating_status VARCHAR(50) DEFAULT 'Active',
    support_notes TEXT,
    PRIMARY KEY (ngo_id, country_id),
    FOREIGN KEY (ngo_id) REFERENCES ngo(ngo_id),
    FOREIGN KEY (country_id) REFERENCES country(country_id)
);

-- 13. WATCHLIST
CREATE TABLE IF NOT EXISTS watchlist (
    watchlist_id INT NOT NULL AUTO_INCREMENT,
    user_id INT NOT NULL,
    country_id INT NOT NULL,
    added_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (watchlist_id),
    UNIQUE KEY uq_watchlist_user_country (user_id, country_id),
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (country_id) REFERENCES country(country_id)
);

-- 14. COUNTRY_SUMMARY_REPORT
CREATE TABLE IF NOT EXISTS country_summary_report (
    report_id INT NOT NULL AUTO_INCREMENT,
    country_id INT NOT NULL,
    user_id INT NOT NULL,
    report_title VARCHAR(150),
    report_text TEXT,
    export_format VARCHAR(20),
    generated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (report_id),
    FOREIGN KEY (country_id) REFERENCES country(country_id),
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```

## Data Visualization & Interpretation

### Correlation heatmap

Chosen to answer our key question, whether climate relates to displacement. Measuring displacement as asylum applications, the results showed a weak but statistically significant correlation between temperature, dry days, and asylum applications per 100k (r=0.26, p<0.05).

![Correlation Heatmap](correlation_heatmap.png)

### Time series (asylum applications per 100k over time)

Chosen to show that displacement is not uniform across countries. Observing our plot, we can see clear spikes in 2015 for Austria, Germany and Sweden. However, no sever climate event happened, instead, that was the year of the Syrian refugee crisis. This is a clear example of how geopolitical events heavily influence displacement. This type of context is important for interpreting model errors. 

![Time Series](time_series.png)

### Country comparison bar chart

Chosen to easily interpret asylum rates per population. This graph shows how small countries by population like Cyprus, Malta and Luxemburg show disproportionately high asylum rates. Normalizing by population gives us a more meaningful cross-country comparisons. 

![Country Averages](country_averages.png)

### Actual vs predicted

Used to visualize how model performs against real data from focus countries (Germany, Greece, Sweden, France). We've chosen to represent Germany and France because they are the largest and mose easy to predict based on past data. In addition, we've included Sweden, which gets heavily overestimated due to Sweden limiting asylums in 2015. This is not currently captured by our dataset. 

![Actual vs Predicted](actual_vs_predicted.png)

---

## ML Model - Initial Implementation

### What was implemented

We implemented a linear regression model trained on 2010-2018 data and was tested on 2019-2023, predicting asylum applications using climate, population and economic variables. 

## Results

- R^2 = 0.49 - explains 49% of variance, improved from 0.43 before suggestions to add lag term and standardizing inputs.
- MAE = 20,546 applications - improved from 22,837 predicted applicates after adding model improvements (lag, standardization).

### Difficulties

- From our EAS analysis, we've determined that asylum applications are heavily influenced by geopolitical events (Syria 2015, Ukraine 2022) that climate or economic variables can't predict.
- With only 378 rows in our cleaned dataset, it limits our model complexity. Therefore, deep models would overfit such a small dataset.
- Sweden has consistant overestimation, however, this is because of post-2015 changes to asylum applications. 

## Tasks remaining

- Build Logistic Regression classifier using risk labels derived asylum "pressure" above 30% median for each country. 
- Add cross-validation given the small dataset size

## Wireframing
### Home
![Home](home.png)

### Policy Analyst
![PolicyGP1](GP1.png)
![PolicyGP2](GP2.png)

Persona 1 - 
The first wireframe for the Policy Analyst focuses on the Compare page, which allows the user to select multiple countries, in this case Germany, France, Greece, and Spain, and view their asylum application trends over time from 2018 to 2023. The page shows a multi line graph where each country is color coded for easy comparison and a legend on the left shows which color represents which country. At the bottom of the page there are export options including Save, CSV, PNG, and PDF Summary so that the analyst is able to quickly take the visual and use it in reports or briefings.

### Humanitarian Coordinator
![HumanDP1](DP1.png)
![HumanDP2](DP2.png)

Persona 2 -
The second wireframe is the Saved Views page, which lets the Policy Analyst store specific comparison setups for future use. The wireframe shows three saved views: Southern Frontline, Central Europe, and the Baltic States. Each saved view has a swap and update option which would allow the analyst to swap out countries or update the date range without having to start a new comparison from scratch.


### Mohammed
![MohammedMP1](MP1.png)
![MohammedMP2](MP2.png)

Persona 3 -
The first wireframe for the Humanitarian Coordinator is the Europe Risk Map page, which gives a broad overview of risk levels across European countries through a grid of country codes . The countries are color coded based on their current risk level so the coordinator is able to quickly see where attention is needed. When a country is clicked, in this case Greece, a detail panel appears at the bottom showing key stats such as asylum applications being 42,300, top stress factors which are wildfires/heat, and active NGOs being 7. There is also a Manage NGOs button which gives the coordinator quick access to the NGO management features.