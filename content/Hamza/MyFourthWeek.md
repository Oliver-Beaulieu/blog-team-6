---
title: "Hamza's Second Week Reflection"
date: 2026-6-12
draft: false
description: "My 4th week and phase 4 contribution"
tags: ["reflection"]
authors:
  - "Hamza"
showAuthorsBadges: false
---

### Individual Contributions

I took control of a majority of Mohammad Sako's persona pages such as his his dashboard, country lookup, and asylum pathway views. This involved thinking through what a user in Mohammad's situation actually needs from the app and making sure the pages reflected that, not just technically but in terms of what information surfaced and how. This is really important because a student needs to easily be able to see the information they are looking for. 

I also contributed heavily to Gabriel's persona, particularly the Compare Countries and Saved Views pages. Compare Countries lets you select any of the 27 EU member states, toggle between regional groupings, and get a chart of risk and displacement data over time. Saved Views lets the user save a chart if they wanted to and can change it at any time. Getting the cross page state to connect correctly with st.session_state took a good amount of debugging since switch_page paths had to match exact filenames after the repo was reorganized.

Behind the scenes, I put in some help on some REST API paths with Yadiel, shaping some routes on how they go from Streamlit to mysql. For handling risk categories, a separate file called risk_routes.py came together under Flask's blueprint system, managing retrieval and updates while checking inputs before saving changes.

At first, we sat down together to map out how the database would work, focusing on tables such as risk_assessment, displacement, and watchlist so they’d line up with what each user type really does. Later on came Phase II, where sketches of the interface took shape across all three roles, setting a clear direction for everyone long before development started.

### Program Reflection

Belgium was a great experience overall. Brussels and Leuven are both really cool cities and being able to explore them while working on a project like this made the whole thing feel worthwhile. Outside of class, getting to meet new people, try new food, and just be somewhere completely different from home was something I didn't take for granted. The professors and Seamus did a good job of balancing the academic side with actually making the trip enjoyable. I’m really glad I did it
