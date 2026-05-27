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
![Policy1](Policy_AnalystER.png)
![Policy11](GabrielDDL.png)

### Localized Data Model Description

The localized data model for our policy analyst tends to focus on climate risk, climate events, and government policy in the EU countries. The country entity is in the middle because all policy, climate events, climate indicator, and risk assessment belong to a specific country and when we are collecting data for them the best way to identify them is by the country they belong to.

### Cardinality
The entity country has one-to-many relationships with climate_indicator, climate_event, risk_assessment, and policies which just basically means that one country is able to have multiple climate records, events, risk assessments, and policies over time. 


### Persona 2 - Humanitarian Coordinator
![Humanitarian](DianaER.png)
![Humanitarian1](DianaDDL.png)

### Localized Data Model Description

The localized data model for our Humanitarian Coordinator aims to focus on displacement, climate risk, and Non-Governmental Organization (NGO) support in the EU countries. The country entity is in the middle because displacement records, risk assessments, and NGO activity all connect back to a specific country where this humanitarian support may be needed.

### Cardinality

The entity country has one-to-many relationships with displacement and risk_assessment, which means that countries are able to have multiple displacement records and risk assessments over time. When it comes to the relationship between NGO and country, we label it as a many-to-many through ngo_country, meaning that one NGO is able to operate in many different countries, and one country can have multiple NGOs working there.


### Persona 3 - Climate-Displaced Student
![Student](MohammedER.png)
![Student1](MohammedDDL.png)

### Localized Data Model Description

The localized data model for our Climate-Displaced Student aims to focus on climate conditions, climate events, and climate risk in EU countries as he is just browsing through the app to gain more information on what’s going on. Similar to the other personas, the country entity is in the middle because climate indicators, climate events, and risk assessments tend to all belong to a specific country and these countries are used to identify the environmental conditions that might be contributing to climate displacement.

### Cardinality

The entity country here as one-to-many relationships with climate_indicator, climate_event, and risk_assessment that means one country can have multiple climate records, climate events, and risk assessments over time. This would allow the student persona to be able to compare climate trends and risk levels across different countries.

### Global
![Global1](GlobalER.png)
![Global](GlobalDDL.png)

## Global Data Model DDL

```sql
CREATE TABLE country (
    country_id INT PRIMARY KEY,
    country VARCHAR(100) NOT NULL,
    region VARCHAR(100),
    population INT
);

CREATE TABLE climate_indicator (
    indicator_id INT PRIMARY KEY,
    country_id INT NOT NULL,
    year INT,
    avg_temp DECIMAL(5,2),
    precipitation INT,
    flood_risk VARCHAR(50),
    wildfire_risk VARCHAR(50),
    FOREIGN KEY (country_id) REFERENCES country(country_id)
);

CREATE TABLE climate_event (
    event_id INT PRIMARY KEY,
    country_id INT NOT NULL,
    event_type VARCHAR(100),
    event_date DATE,
    severity VARCHAR(50),
    FOREIGN KEY (country_id) REFERENCES country(country_id)
);

CREATE TABLE risk_assessment (
    risk_id INT PRIMARY KEY,
    country_id INT NOT NULL,
    year INT,
    risk_score DECIMAL(5,2),
    risk_level VARCHAR(50),
    FOREIGN KEY (country_id) REFERENCES country(country_id)
);

CREATE TABLE displacement (
    record_id INT PRIMARY KEY,
    country_id INT NOT NULL,
    year INT,
    asylum_apps INT,
    FOREIGN KEY (country_id) REFERENCES country(country_id)
);

CREATE TABLE policies (
    policy_id INT PRIMARY KEY,
    country_id INT NOT NULL,
    name VARCHAR(150),
    policy_type VARCHAR(100),
    status VARCHAR(50),
    FOREIGN KEY (country_id) REFERENCES country(country_id)
);

CREATE TABLE ngo (
    ngo_id INT PRIMARY KEY,
    ngo_name VARCHAR(150) NOT NULL,
    focus_area VARCHAR(100),
    contact_email VARCHAR(150),
    website VARCHAR(200)
);

CREATE TABLE ngo_country (
    ngo_id INT NOT NULL,
    country_id INT NOT NULL,
    PRIMARY KEY (ngo_id, country_id),
    FOREIGN KEY (ngo_id) REFERENCES ngo(ngo_id),
    FOREIGN KEY (country_id) REFERENCES country(country_id)
);
```

## Data Visualization & Interpretation

### Correlation heatmap

Chosen to answer our key question, whether climate relates to displacement. Measuring displacement as asylum applications, the results showed a weak but statistically significant correlation between temperature, dry days, and asylum applications per 100k (r=0.26, p<0.05).

### Time series (asylum applications per 100k over time)

Chosen to show that displacement is not uniform across countries. Observing our plot, we can see clear spikes in 2015 for Austria, Germany and Sweden. However, no sever climate event happened, instead, that was the year of the Syrian refugee crisis. This is a clear example of how geopolitical events heavily influence displacement. This type of context is important for interpreting model errors.

### Country comparison bar chart

Chosen to easily interpret asylum rates per population. This graph shows how small countries by population like Cyprus, Malta and Luxemburg show disproportionately high asylum rates. Normalizing by population gives us a more meaningful cross-country comparisons.

---

## ML Model - Initial Implementation

### What was implemented

We implemented a linear regression model trained on 2010-2018 data and was tested on 2019-2023, predicting asylum applications using climate, population and economic variables.

## Results

- R^2 = 0.43 - explains 43% of variance.
- MAE = 22,837 applications - moderate error given scale of data and un-predictable causation.

### Difficulties

- From our EAS analysis, we've determined that asylum applications are heavily influenced by geopolitical events (Syria 2015, Ukraine 2022) that climate or economic variables can't predict.
- With only 378 rows in our cleaned dataset, it limits our model complexity. Therefore, deep models would overfit such a small dataset.

## Tasks remaining

- Integrade another additional feature that can help support model accuracy
- Build K-NN classifier using risk labels derived from asylum per 100k
- Add cross-validation given the small dataset size

