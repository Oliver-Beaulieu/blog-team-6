---
  title: "Oliver Last Week"
  date: 2026-06-11
  draft: false
  description: "Phase 4 discussion and contributions"
  tags: ["reflection", "ml", "validation", "europe"]
  authors:
    - "oliver_beaulieu"
  showAuthorsBadges: false
---

## Individual Contributions

### Phase 4 - Wrapping up and plugging Model 1

For the final phase, I focused on validating the model I had made and getting it to be production-ready. I added a residuals-vs-fitted plot and a QQ plot to see whether my linear regression assumptions help up. I learned that variance was not constant across the range. Next, I did a multicollinearity test with VIF and trimmed down a few features that would lead to a boost in performace. Some of these features include `population` (VIF ~= 7986), `urban_pct` (VIF ~= 338), `precip_total` and `evapotrans_total` which were both redundant with country and climate aggregates. After cleaning up the model, I updated the API routes and UI to match the new set of feature inputs the user will fill in.

Lastly, I cleaned up some of the projects documentation. I rewrote our README using the template into a bespoke one for our TERRA web-app. In addition, I added a sub-README to help keep track of everyone's contributions. I added my photo and bio to the About page, as well. 

### Last Week

The other day, I enjoyed our visit to Genk, C-Mine and Energyville. I learned a lot during the C-Mine visit as it was the first time I'd visiting a coal mine. The VR experience gave me the best insight into what it might be like to be a coal miner. To be honest, the conditions of the mine look a lot better than I thought, but still looks very dangerous. 