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
---
# TERRA: Tracking European Climate Risk & Refugee Asylum

# Project Description

Climate change is a current, real threat: from rising sea levels to temperatures higher than ever, and even increased extreme weather events like wildfires and flooding. It is affecting people all over the world, destroying peoples’ houses and displacing them from their homes, including those in the countries in the European Union. However, no website or app is currently available to track both disasters related to climate change and displacement data caused by it.

This is why we created/This is why we want to create TERRA. TERRA (Tracking European Climate Risk & Refugee Asylum) is an app that’s driven by data and designed in a way that helps visualize/analyze displacement trends related to climate change across Europe. Our app will aim to combine climate indicators, disaster records, economic data, and displacement statistics from international datasets which are public and with a focus on all 27 EU member states. 
Our users would be able to explore interactive maps, compare countries, and view climate stressors like floods and wildfires to help monitor displacement trends over time. It will also use machine learning models to help classify climate displacement risk levels and understand countries with similar situations.
Overall, our goal with TERRA is to provide a better understanding of how climate events may influence displacement and humanitarian pressures across Europe.

# User Personas

## Persona 1 — Gabriel Diallo, Policy Analyst

## Persona 2 — Diana Willis, Humanitarian Coordinator

## Persona 3 — Mohammed Sako, Climate-Displaced Student


# Candidate Data Sources

## Source 1 — Open-Meteo

Purpose: Open-Meteo will be our primary climate data source as it provides historical and forecast weather information globally which includes temperature, precipitation, wind speed, and climate related indicators.

Link: https://open-meteo.com/ 

API Access: Open Meteo is free, publicly accessible and doesn’t require an API key.

ML Data Contents: Open-Meteo has a lot of historical and forecast observations through hourly and daily climate records including features like temperature max/mins, precipitation totals, wind speed, etc.

Utility to Personas: Supports Mohammed since it would help him understand the climate conditions affecting areas similar to his, Gabriel would be able to accurately understand climate across countries, and Diana would monitor environmental conditions that may contribute to humanitarian pressures.


## Source 2 — Eurostat

Purpose: Eurostat is our displacement and migration data source as it has information related to asylum applications, refugee statistics, and population.

Link: https://ec.europa.eu/eurostat/en/ 

API Access: Eurostat is publicly accessible and has free access to APIs without paywalls.

ML Data Contents: Eurostat contains observations across all EU member states and includes asylum applications, migration statistics, refugee data, etc which would provide data that’s good for machine learning as a displacement classification model.

Utility to Personas: This supports Mohammed by helping him explore displacement across Europe, Gabriel through policy analysis and migration comparisons, and Diana by identifying areas experiencing humanitarian pressure.

## Source 3 — World Bank API

## Source 4 — GDELT Project