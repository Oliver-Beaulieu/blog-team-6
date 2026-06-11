---
title: "James Last Week"
date: 2026-06-11
draft: false
description: "Phase 4 discussion and contributions"
tags: ["reflection", "ml", "europe"]
authors:
  - "James_Chan"
showAuthorsBadges: false
---

## Individual Contributions

### Phase 4 - Wrapping up and plugging Model 2

For the final phase, I focused on making a new model as the original second model was too similar to the first model Oliver made. The original model was a logistic regression model that predicted whether a country had a large amount of asylum applicants or not, but Oliver's model predicted the amount of asylum applicants, so that was way too similar. For the new model, I instead made a multiple linear regression model to predict heatwave days, high precipitation days, and dry days. To do this, the model takes in GDP per capita, unemployment rate, population, urban pct, asylum applications, and country code (country code is hot encoded). For the R^2, heatwave days was 0.84, heavy precipitation days was 0.66, and dry days was 0.64. For RMSE, heatwave days was 3.054, precipitation days was 2.29, and dry days was 18.27. For the MAE, heatwave days was 1.32, precipitation days was 1.80, and dry days was 15.52. I then added residuals plots for heatwave days, precipitation days, and dry days. Finally, I proceeded to write my model in python so that it could be implemented into the website

I also added my photo and bio to the about page on the website.

### Program Related

This week I went to Antwerp, Genk, and an escape room.
In Antwerp, I went to the Het Steen and the Plantin-Moretus Museum. Het Steen was a small castle, and the top had a nice view. The Plantin-Moretus Museum was about printing presses, and I enjoyed it a lot, even getting a poem printed with one of the presses.
In Genk, I went to Energyville and C-Mine. I really liked the Energyville tour as I got to see lots of cool things, like the battery exploding box. At C-Mine I had fun walking through the mines and climbing the giant tower was worth it for the view.
For the escape room, I chose the Saving Ryan one. Although I had fun, one of the locks was incorrectly set so we lost a bunch of time on that and only finished 80% the escape, and I think we would've been able to solve the whole thing if it wasn't for that lock.