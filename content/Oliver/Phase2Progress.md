---
title: "Oliver Beaulieu"
date: 2026-05-26
draft: false
description: "Phase 2 discussion and contributions"
tags: ["reflection", "sql", "europe"]
authors:
  - "oliver_beaulieu"
showAuthorsBadges: false
---

## Individual Contribution

**Data Ingestion (1. OpenMetro)**

Created a Jupyter notebook that led the full collection of climate data across all 27 EU member states. I built in rate limit protection to the script through trial and error, and saved to `open_metro_raw.csv` to be cleaned and merged later.

**Data Ingestion (2. Eurostat)**

Created a Jupyter notebok to collect asylum applicant statistics. Handled dataset filtering, did minor country code remapping (EL -> GR), and saved date to `eurostat_raw.csv`.

**Data Ingestion (3. WorldBank)**

Created a Jupyter notebok to collect socioeconomic indicators (GDP, unemployment, population, urban percentage) and saved to `worldbank_raw.csv`.


**EDA & Visualization**
Produced all plots and EDA statistical analysis exported to `../outputs/`. Ran Pearson correlation test to confirm statistical significance within our data (r=0.26, p<0.05) 

**Model Documentation** 
Wrote analysis of our initial linear regression model and what its test outputs mean for our model's accuracy. 

## Dialogue Update

Today, I enjoyed a two part workshop with OASC (Open Agile Smart Cities). They introduced lots of unique concepts that I havn't heard of until today. For example, Local Digital Twins act as a simulation of real world interaction to create smart cities. They discussed many of the downfalls of IT in the government, such as high IT costs and vendor lock-in's. In addition, the concept of mission cities was new to me. The fact that many European cities have acheived climate neutrality is amazing to me. 