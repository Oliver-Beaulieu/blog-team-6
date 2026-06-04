---
title: "Hamza's third Week Reflection"
date: 2026-05-28
draft: false
description: "My third week and phase 2 contribution"
tags: ["reflection"]
authors:
  - "Hamza"
showAuthorsBadges: false
---

### Individual Contributions

In Phase 3, I was mainly making risk classification routes on the backend and turning Gabriel's wireframes into actually working on the streamlit app. On the backend, I wrote the flask that handles risk data, including a GET route that returns every countrys risk classification joined to its country info, and a PUT route that updates a country's risk score, level, and notes for a specific year. Getting the PUT right meant making sure the required fields before touching the database and structuring the SQL so it targets the correct country and year, small details that matter a lot when the frontend depends on the data coming back cleanly.
On the frontend, I took Gabriel's Phase 2 wireframes and built them into Streamlit pages, one being a Compare Countries where the analyst can select any of the 27 EU member states and be able to view asylumapplication trends over time, and more. I also added a Saved Views page that stores and reloads specific comparisons. 

### Program Reflection
 
I enjoyed this week because of a talk on digital and industrial policy in Europe, put together by FEPS. People voiced their opinions and so on so discussion was overlapped, questioned, shifted direction now and then. Ideas bumped into one another. It made the overall talk very enjoyable.
