# WWII Major Battles – Exploratory & Predictive Analysis

**`WW2_Analysis.ipynb`** – A fully executed Jupyter notebook analyzing **70 hand-curated major battles** of World War II (1939–1945) using data analysis, machine learning, interpretability, and causal reasoning.

---

### Dataset (100% hand-built by the author)

- **No external dataset used** – every single battle was researched, compiled, and inserted into a local PostgreSQL database (`ww2.battles`) by me.
- 70 battles with the following columns:

| Column                | Description                                      |
|-----------------------|--------------------------------------------------|
| battle_id             | Unique ID                                        |
| battle_name           | Name of the battle                               |
| location              | Geographic location                              |
| start_date / end_date | Battle period                                    |
| terrain_type          | Urban, Land, Sea, Mountainous, Beach, etc.       |
| countries_involved    | Nations involved                                 |
| ground_involved       | True/False                                       |
| air_involved          | True/False                                       |
| naval_involved        | True/False                                       |
| allied_soldiers       | Estimated Allied troop strength                  |
| axis_soldiers         | Estimated Axis troop strength                    |
| allied_deaths         | Estimated Allied casualties                      |
| axis_deaths           | Estimated Axis casualties                        |
| winner                | Allied or Axis                                   |
| battle_duration      | Duration in days                                 |

**Strong disclaimer**  
Casualty numbers, troop strengths, and even exact dates differ wildly across historical sources. These figures are **best-effort approximations** compiled from multiple references and should be treated as **illustrative only**, not definitive historical fact.

---

### What the Notebook Actually Does

1. **Basic EDA**
   - `df.info()` and `df.describe()`

2. **Visualizations (all with outputs shown)**
   - Pie charts: total soldiers & total deaths (Allied vs Axis)
   - Yearly combined casualties (line graph)
   - Casualties by terrain type (bar chart)
   - Number of battles per year (line graph)
   - Yearly victories by each side (stacked bar chart)
   - Battle duration vs terrain type (bar chart)
   - Military branch involvement breakdown (pie chart)
   - Interactive Folium heatmap of battle locations (coordinates found for 44 out of 70 battles)

3. **Machine Learning – Predicting the Winner**
   - Target: `winner` → 0 = Allied, 1 = Axis
   - Features: `allied_soldiers`, `axis_soldiers`, `terrain_encoded`, `ground/air/naval_involved`, `battle_duration`
   - Models tested: **Random Forest** vs **XGBoost**

   **Results (random split)**
   - Random Forest: **~79% accuracy**
     - Precision (Allied): 0.75 | Recall (Allied): 1.00
     - Precision (Axis): 1.00 | Recall (Axis): 0.40
   - XGBoost: **~57% accuracy** – clearly worse

   **Temporal holdout (train ≤ 1942, test 1943–1945)**
   - Random Forest accuracy: **76.67%**
   - Still beats random guessing by a huge margin even when predicting “future” battles

4. **SHAP Explanations**
   - On the Random Forest model
   - Top drivers of Axis victories: larger Axis troop numbers, ground involvement, certain terrain types

5. **Probabilistic Calibration**
   - Brier Score (Axis class): **0.1779** → reasonably well-calibrated probabilities
   - Calibration curve included

6. **Causal-Style Question: Does terrain affect Allied casualties?**
   - OLS regression: `allied_deaths ~ allied_soldiers + axis_soldiers + terrain_dummies`
   - Result:
     - Both troop size variables **highly significant** (p < 0.05 & p < 0.01)
     - No terrain category significant after controlling for army size
     - R² = 0.56

---

### Final Takeaways

- Troop strength dominates everything: casualties and who wins.
- Random Forest reliably predicts battle outcomes—even later in the war.
- Terrain matters historically, but its effect is mostly through how many troops can fight, not some independent “terrain magic.”
- With only 70 battles and heavy class imbalance (way more Allied wins), predicting rare Axis victories remains the hard part.

---

**All cells are pre-run with outputs displayed** – just open the notebook and scroll. No setup, no execution needed.

Enjoy the deep dive into WWII’s most important battles.
