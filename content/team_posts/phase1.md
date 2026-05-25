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
Gabriel is a 31 year old policy analyst at a European Union ministry in Brussels who is working on analyzing and proposing climate adaptation and migration policy. His job tends to involve minotring the displacement trends in all the member states to then flag countries and prepare recommendations based on his observations for the senior decision makers. He needs to be able to get data that’s organized so that he support his policy presentations and provide some recomendation to his team.

**User Stories:**

- As a polciy analyst, I want to flag a country for policy review with a status and also create a short note so that I am able to track which states need closer attention. 
- As a polciy analyst, I want to be able to update the status of an existing policy so that I can keep our records up to date with how situations are being handled.
- As a polciy analyst, I want to delete a policy flag when a country may not even need a review so that my list stays relevant to who needs the most attention. 
- As a polciy analyst, I want historical trends so that I can compare annunal asylum applications across EU countries and identify which ones have a high displacement rate.
- As a polciy analyst, I want to download charts and country comparison maps where countries are grouped by similarity based on there trends and risk levels as a png so that I can properly put them into my presentations.

### Persona 2 — Diana Willis, Humanitarian Coordinator

**Description:** 
Diana is a 38 year old humanitarian coordinator who works with non profits in order to support the displaced populations across Europe. She needs help finding a tool that will help monitor risk and prioritize resources so that she is able to make resourceing descions quickly and not have to look through large amounts of da

**User Stories:**

- As a Humanitarian Coordinator, I want to look at a risk classification list based on low, moderate, and high for each EU country thats color coded so I can figure out where help is needed urgently.
- As a Humanitarian Coordinator, I want to add, update and delete an NGO entry to a countries profile so that I can track which organizations are active in certain areas, while also being able to keep certain contact information up to date and if organizations are no longer operating Id like to be able to remove them.
- As a Humanitarian Coordinator, I want historical displacement data so that I can understand the trends long term.
- As a Humanitarian Coordinator, I want to be able to export and download a country summary as a pdf so that I can quickly review conditions and understand where to direct resources to.

### Persona 3 — Mohammed Sako, Climate-Displaced Student

**Description:**
Mohammed is a 21 year old student at KU Leuven who is from a flood affected area in Southern Europe. He ended up relocating after many climate related disasters impacted his area. Due to his experiences he wants accessible information about climate risks and displacement trends affecting areas similar to his. He wants a tool that's easy to interpret visually and helps him understand whats happening in regions like his home area.

**User Stories:**

- As Mohammed, I want to look at a map of Europe that shows climate displacement risks so that I can quickly identify regions facing challenges similar to mine.
- As Mohammed, I want to click on a country and see which climate events (floods, wildfires, droughts, etc.) are affecting them.
- As Mohammed, I want to view a timeline chart that shows how displacement numbers in certain countries have changed overtime. 
- As Mohammed, I want information displayed through visuals and charts that are simple without worrying about a lot of technical knowledge so I can conduct my own research in school.
 


## Candidate Data Sources

### Source 1 — Open-Meteo6

**Purpose:** Open-Meteo will be our primary climate data source as it provides historical and forecast weather information globally which includes temperature, precipitation, wind speed, and climate related indicators.

**Link:** https://open-meteo.com/ 

**API Access (Python):** Open Meteo is free, publicly accessible and doesn’t require an API key. 
```python
import requests

url = "https://archive-api.open-meteo.com/v1/archive"
params = {
    "latitude": 48.8566,
    "longitude": 2.3522,
    "daily": ["temperature_2m_max", "precipitation_sum"],
    "start_date": "2023-01-01",
    "end_date": "2023-01-01",
    "timezone": "Europe/Paris"
}

response = requests.get(url, params=params)
data = response.json()
print(f"Status Code: {response.status_code}")
print(f"Paris on 2023-01-01 | Max Temp: {data['daily']['temperature_2m_max'][0]}°C | Precipitation: {data['daily']['precipitation_sum'][0]}mm")
```


**ML Data Contents:** Open-Meteo has a lot of historical and forecast observations through hourly and daily climate records including features like temperature max/mins, precipitation totals, wind speed, etc.

**Utility to Personas:** Supports Mohammed since it would help him understand the climate conditions affecting areas similar to his, Gabriel would be able to accurately understand climate across countries, and Diana would monitor environmental conditions that may contribute to humanitarian pressures.



### Source 2 — Eurostat

**Purpose:** Eurostat is our displacement and migration data source as it has information related to asylum applications, refugee statistics, and population.

**Link:** https://ec.europa.eu/eurostat/en/ 

**API Access:** Eurostat is publicly accessible and has free access to APIs without paywalls.
```python
import requests

url = "https://ec.europa.eu/eurostat/api/dissemination/statistics/1.0/data/migr_asyappctzm?geo=BE&time=2024-01"
response = requests.get(url, params=params)

print(response.status_code)
data = response.json()
print(data.keys())
```

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

