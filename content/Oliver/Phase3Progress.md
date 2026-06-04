---
title: "Oliver 2nd Week"
date: 2026-05-26
draft: false
description: "Phase 2 discussion and contributions"
tags: ["reflection", "sql", "europe"]
authors:
  - "oliver_beaulieu"
showAuthorsBadges: false
---

## Individual Contribution

### Phase 2 revisions

After reading our feedback regarding Phase 2, I've decided to go back and make meaningful changes to ML model one. Firstly, I added a lag term, `asylum_lag1`, that points to a countries previous years assylum number. I believe this will lead to a more accurate predictions becasue a previous years asylum data is the best way to find the predicted years. Weighing data from 10 years ago the same as last years would be a silly mistake and this lag term fixed that. After I implemented it and added it to the dataframe, testing showed R^2 from 43% to 49%. 

Secondly, I added a standard scaler fitting to training data only, then applied to both training and test. Now, it trains on X_train_scaled and predicts on X_test_scaled instead. In addition, I also added scaled row prediction before being passed to the model, so it will match trained data. 

Lastly, I converted our first models Jupyter notebook to a runnable python script, as well as loaded data (model weights, training output) into an SQL database. 

### Life Update

Today, I enjoyed going to Brussels to hear about the left-leaning side of oppinons on digital policy. A key part of our discussion was the relationship between the EU and the US from their perspective, which I found very interesting. It seemed that they had a very pesimistic view of our relationship, and overall promoted complete isolation of tech ownership. Later today, I loved learning about the chocolate making process, as I am a huge fan of dark chocolate. What a great way to end the day!