---
title: "Project - Phase I: TERRA"
date: 2026-05-18
draft: false
description: "Tracking European Climate Risk & Refugee Asylum — our Phase 1 design report"
slug: "phase1post"
tags: ["project", "phase1", "climate"]
authors:
  - "yadiel_cruz"
  - "hamza"
  - "oliver"
  - "james"
showAuthorsBadges: false
showTableOfContents: true
---
# TERRA: Tracking European Climate Risk & Refugee Asylum

## Project Description

Climate change is a current, real threat: from rising sea levels to temperatures higher than ever, and even increased extreme weather events like wildfires and flooding. It is affecting people all over the world, destroying peoples’ houses and displacing them from their homes, including those in the countries in the European Union. However, no website or app is currently available to track both disasters related to climate change and displacement data caused by it.

This is why we created/This is why we want to create TERRA. TERRA (Tracking European Climate Risk & Refugee Asylum) is an app that’s driven by data and designed in a way that helps visualize/analyze displacement trends related to climate change across Europe. Our app will aim to combine climate indicators, disaster records, economic data, and displacement statistics from international datasets which are public and with a focus on all 27 EU member states. 

Our users would be able to explore interactive maps, compare countries, and view climate stressors like floods and wildfires to help monitor displacement trends over time. It will also use machine learning models to help classify climate displacement risk levels and understand countries with similar situations.

Overall, our goal with TERRA is to provide a better understanding of how climate events may influence displacement and humanitarian pressures across Europe.

## User Personas

### Persona 1 — Gabriel Diallo, Policy Analyst

**Description:** 
Gabriel is a 31 year old policy analyst that is from Brussels who researches climate adaptation and migration policy. He needs data that’s data in order to support policy presentations and recommendations to his team.

**User Stories:**

      1. As Gabriel, I want to compare displacement trends between countries so that I can identify regions that are in high risk zone
      2. As Gabriel, I want charts that are downloadable so that I can present them in reports and presentations.
      3. As Gabriel, I want to understand major climate stressors by country so that I can support discussions with my team
      4. As Gabriel, I want historical trends which will help me analyze changes over time.
      5. As Gabriel, I want countries grouped by similarity so that I can identify common patterns.

### Persona 2 — Diana Willis, Humanitarian Coordinator

**Description:** 
Diana is a 38 year old humanitarian coordinator that works with non profits in order to support the displaced populations across Europe. She needs a tool that will help monitor risk and prioritize resources.

**User Stories:**

      1. As Diana, I want a list of countries so that I can monitor locations important to my work.
      2. As Diana, I want a climate displacement risk classifications so that I can prioritize resources.
      3. As Diana, I want historical displacement data so that I can understand the trends long term.
      4. As Diana, I want country summaries so that I can quickly review conditions and understand where to direct resources to.

### Persona 3 — Mohammed Sako, Climate-Displaced Student

**Description:**
Mohammed is a 21 year old student at KU Leuven who is from a flood affected area in Southern Europe. He ended up relocating after many climate related disasters impacted his area. Due to his experiences he wants accessible information about climate risks and displacement trends affecting areas similar to his. 

**User Stories:**

      1. As Mohammed, I want to look at a map of Europe that shows climate displacement risks so that I can quickly identify regions facing challenges similar to mine.
      2. As Mohammed, I want to click on a country and see which climate events (floods, wildfires, droughts, etc.) are affecting them.
      3. As Mohammed, I want to explore the historical trends so that I can understand how conditions have changed over time in different regions.
      4. As Mohammed, I want information displayed through visuals and charts that are simple without worrying about a lot of technical knowledge
 


## Candidate Data Sources

### Source 1 — Open-Meteo6

**Purpose:** Open-Meteo will be our primary climate data source as it provides historical and forecast weather information globally which includes temperature, precipitation, wind speed, and climate related indicators.

**Link:** https://open-meteo.com/ 

**API Access:** Open Meteo is free, publicly accessible and doesn’t require an API key.

**ML Data Contents:** Open-Meteo has a lot of historical and forecast observations through hourly and daily climate records including features like temperature max/mins, precipitation totals, wind speed, etc.

**Utility to Personas:** Supports Mohammed since it would help him understand the climate conditions affecting areas similar to his, Gabriel would be able to accurately understand climate across countries, and Diana would monitor environmental conditions that may contribute to humanitarian pressures.


### Source 2 — Eurostat

**Purpose:** Eurostat is our displacement and migration data source as it has information related to asylum applications, refugee statistics, and population.

**Link:** https://ec.europa.eu/eurostat/en/ 

**API Access:** Eurostat is publicly accessible and has free access to APIs without paywalls.

**ML Data Contents:** Eurostat contains observations across all EU member states and includes asylum applications, migration statistics, refugee data, etc which would provide data that’s good for machine learning as a displacement classification model.

**Utility to Personas:** This supports Mohammed by helping him explore displacement across Europe, Gabriel through policy analysis and migration comparisons, and Diana by identifying areas experiencing humanitarian pressure.

### Source 3 — World Bank API

**Purpose:** The World Bank provides socioeconomic context for since climate events don't fully explain the trends so economic and development indicators can help explain differences between countries.

**Link:** https://datahelpdesk.worldbank.org/knowledgebase/topics/125589 

**API Access:** The World Bank API is accessible and free to use.

**ML Data Contents:** The World Bank has historical observations across more than 200 countries and thousands of indicators including GDP per capita, unemployment rates, population size, etc which would be good for our model

**Utility to Personas:** This supports Mohammed by giving context about climate vulnerability, Gabriel by supporting policy and economic analysis, and Diana by helping identify pressures that might also influence displacement.

### Source 4 — GDELT Project

**Purpose:** GDELT will help us track news and events to help identify climate related disasters and humanitarian developments across Europe.

**Link:** https://www.gdeltproject.org/ 

**API Access:** GDELT is accessible, and updated without paywalls.

**ML Data Contents:** GDELT has features like event descriptions, locations, timestamps which would be useful as we plan to focus on climate related events like floods, droughts, within Europe.

**Utility to Personas:** This supports Mohammed by highlighting ongoing climate events, Gabriel by monitoring climate trends and events, and Diana by tracking where to prioritize resources.

