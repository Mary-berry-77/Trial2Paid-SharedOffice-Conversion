## 📘 Shared Office Trial Conversion Analysis

This project analyses behavioural log data from a shared office provider to uncover key drivers of trial-to-paid user conversion and proposes actionable strategies based on user segmentation.

---

### 🧭 TL;DR (Quick Summary)

#### ❗ Conversion Issue
From 2021 to 2023, the trial-to-paid conversion rate fell from **53% to 23%**, despite stable trial sign-ups.  
The key bottleneck lies **after their visit**, not before.

#### 🔍 Behavioural Shift
Feature analysis reveals a shift in conversion drivers:
- **Pandemic era**: Usage quantity (longer stays, more visits)
- **Post-pandemic**: Intent signals like **visit timing (weekday/afternoon)** and **delayed first visit (1–2 days)**

#### ✅ Key Segment Insights

| Segment | Characteristics | Conversion | Strategic Implication |
|--------|------------------|------------|------------------------|
| **Core Hours** | Weekday Morning/Afternoon (staffed hours) | **42.5%** | High intent → Optimise onboarding & space quality |
| **Out-of-Hours** | Night, Early Morning, or Weekend (unstaffed hours) | **33.9%** | Underperforming but large group → Improve out-of-hours experience |

---

### 📂 Project Scope & Data

- **Data Period**: May 2021 – Dec 2023  
- **Target Users**: Free trial users (3-day access, first-time only)  
- **Business Context**: The trial aims to convert new users into paying customers through first-hand workspace experience.  
- **Data Sources**:  
  - Trial sign-up logs  
  - Visit history (check-in/out sessions)  
  - In/out access events  
  - Payment status  
  - Site metadata (location size, capacity)  
- **Final Dataset**:  
  - Integrated into a **user-level behavioural data mart**  
  - Includes engineered features like `first_visit_delay`, `main_time_block`, `visit_day_type`, and `avg_density_scaled`

> 🎯 **Goal**:  
To identify behavioural signals that distinguish paying users — and use them to define strategic segments and improve conversion performance.

---

### 📊 Key Questions

1. Where in the trial funnel do users most frequently drop off?  
2. What behavioural traits most strongly differentiate paying users from non-paying users?  
3. How did user behaviour shift between the pandemic and post-pandemic periods?  
4. Which user segments offer the best balance of conversion potential and feasibility?  
5. What low-cost, high-impact actions can improve trial-to-paid conversion?

---

### Data Schema & Feature Engineering

#### Raw Data Schema  
_Trial events, access logs, and payment outcomes relationship_

<img src="https://github.com/user-attachments/assets/c4805fe0-a724-4660-a112-f4926d94f926" width="600"/>


#### Feature Engineering & Data Mart  
_Final user-level dataset for segmentation and modelling_

<img src="https://github.com/user-attachments/assets/661636af-942c-4ebc-ab15-659a23ab077e" width="600"/>


Key engineered features:
- `first_visit_delay`: Days from sign-up to first visit  
- `main_time_block`: Most frequent time block used (e.g., afternoon)  
- `visit_day_type`: Weekday vs weekend usage  
- `avg_density_scaled`: Trial congestion exposure, adjusted by site size  
- `stay_hours`: Total stay time during trial

---
### Buniness Dashboard
![대시보드 1 (1)](https://github.com/user-attachments/assets/459fcc23-ca9d-4117-901b-019cff641b0e)


### 📉 Funnel Breakdown & Business Context

| Year | Sign-ups (avg/mo) | Visitors (avg/mo) | Conversion Rate | Change |
|------|-------------------|-------------------|-----------------|--------|
| 2021 | 351               | 218.6             | 42.8%           | Baseline |
| 2022 | 290.3             | 196.8             | 39.1%           | ▼3.7%p |
| 2023 | 277.7             | 201.8             | 33.6%           | ▼5.5%p |

> 📌 **Key Observations**  
- 2022 saw a **decline in sign-ups (–17%)**, reducing top-funnel volume.  
- 2023 had **stable visits but sharp drop in conversion** → post-visit friction is the issue.

---

### 🦠 Pandemic vs Post-Pandemic Behaviour Shift

Split point: **May 2022**, when most COVID restrictions were lifted

| Period         | Sign-ups (avg/mo) | Conversion Rate | Change |
|----------------|-------------------|-----------------|--------|
| Pandemic       | 335.8             | 41.1%           | —      |
| Post-Pandemic  | 279.8             | 36.1%           | ▼5.0%p |

<img src="https://github.com/user-attachments/assets/0336d8e5-ea6f-47ef-add9-d89db3d151da" width="600"/>

> 🔄 **Behavioural Shift**  
> • **Pandemic**: Longer stays, more frequent visits  
> • **Post-pandemic**: Time-driven and goal-oriented usage (e.g. weekday/afternoon visits, delayed start)

> 🧭 **Strategic Implication**  
Post-pandemic user intent is more autonomous and purposeful.  
→ All modelling and segmentation henceforth focus **solely on post-pandemic data**,  
where behavioural patterns are clearer and more stable.



---

### 🧪 Conversion Analysis: What Drives Payment in the Post-Pandemic Era?

We trained an XGBoost classifier on post-pandemic data (optimised for recall, not deployment).

#### 🔍 Top Predictive Features

<img src="https://github.com/user-attachments/assets/8bb068ea-e0d2-4f9c-aa29-bddcd238d344" width="600"/>

1. `main_time_block`: Time of visit (structured vs unstructured use)  
2. `visit_day_type`: Weekday vs weekend usage  
3. `first_visit_delay`: Planning delay → high intent

---

#### 2. Behavioural Contribution Analysis

##### a. `main_time_block`

<img src="https://github.com/user-attachments/assets/91ed6065-4b61-4730-a02b-8de0068df98f" width="600"/>

| Time Block     | Conv. Rate | Z-Score | Volume Share | Contribution |
|----------------|------------|---------|---------------|--------------|
| Afternoon      | 45.7%      | +2.11   | 13.1%         | +0.277 ✅     |
| Early Morning  | 34.3%      | –0.67   | 42.6%         | –0.287 ⚠️    |
| Morning        | 37.3%      | +0.06   | 32.2%         | +0.020        |

**Insight**: Largest drag comes from Early Morning group despite volume.

---

##### b. `visit_day_type`

<img src="https://github.com/user-attachments/assets/d58c0741-ab56-4b78-ac03-e82f6134b961" width="600"/>

| Type     | Conv. Rate | Z-Score | Volume Share | Contribution |
|----------|------------|---------|---------------|--------------|
| Weekday  | 38.4%      | +0.23   | 74.1%         | +0.174 ✅     |
| Weekend  | 28.5%      | –1.47   | 17.2%         | –0.253 ⚠️    |

**Insight**: Weekend-only users show clear conversion friction.

---

#### c. `first_visit_delay`

<img src="https://github.com/user-attachments/assets/f115cef0-f558-44ff-b2e5-06081c340074" width="600"/>

| Delay    | Conv. Rate | Z-Score | Volume Share | Contribution |
|----------|------------|---------|---------------|--------------|
| 0 days   | 31.9%      | –1.41   | 53.9%         | –0.386 ⚠️    |
| 1 day    | 38.9%      | +0.63   | 36.1%         | +0.363 ✅     |

**Insight**: Immediate visits are impulsive → less likely to convert.

---

### 3. Combined Segment Analysis

<img src="https://github.com/user-attachments/assets/5f7670b6-4b8c-45b3-9f6b-fdca497d5de0" width="600"/>

Combination of:
- `main_time_block`
- `visit_day_type`
- `first_visit_delay`

Reveals strategic segments for targeting.

---

### 🎯 Target Segment Strategy

#### ✅ High-ROI Segment

**Definition**:  
- Weekday Morning/Afternoon  
- 1–2 Day Delay  

**Conversion Rate**: 42.57%  
**Users**: 1,325 → **564** converted

**Actions**:
- Improve onboarding, workspace quality  
- Send follow-up nudges

---

### ⚠️ At-Risk Segment

**Definition**:  
- Early Morning, Midnight, Weekend use  

**Conversion Rate**: 33.93%  
**Users**: 2,290 → **777** converted

**Actions**:
- Fix out-of-hours onboarding issues  
- Provide digital orientation  
- Explore minimal staffing on peak nights/weekends

---

### 📈 Strategy Impact Simulation

| Group          | Users | Baseline CR | +1%p Gain | Conversion Uplift |
|----------------|-------|-------------|-----------|--------------------|
| Core Hours     | 1,327 | 42.5%       | +13.3     | +0.35%p            |
| Out-of-Hours   | 1,560 | 34.4%       | +15.6     | +0.41%p            |
| **Total**      | —     | —           | +28.9     | **+0.76%p**        |

> Even modest improvements deliver measurable uplift without new user acquisition.

---

### ✅ Final Takeaways

- 📉 Conversion drop is **post-visit**, not pre-visit.  
- 🟦 Core Hour users show **high intent and ROI** — invest here.  
- 🟥 Out-of-Hours users show **high friction** — fix operations.  
- 🧠 Focus on **experience, not expansion** → efficient growth.
