# 📘 Shared Office Trial Conversion Analysis

This project analyzes behavioral log data from a shared office provider to uncover key drivers of trial-to-paid user conversion and proposes actionable strategies based on user segmentation.



---


## 🧭 TL;DR (Quick Summary)

### ❗ Conversion Issue
From 2021 to 2023, the trial-to-paid conversion rate fell from **53% to 23%**, despite stable trial sign-ups.  
The key bottleneck lies **after the visit**, not before.

### 🔍 Behavioural Shift
Feature analysis reveals a shift in conversion drivers:
- **Pandemic era**: Usage quantity (longer stays, more visits)
- **Post-pandemic**: Intent signals like **visit timing (weekday/afternoon)** and **delayed first visit (1–2 days)**

### ✅ Key Segment Insights
| Segment | Characteristics | Conversion | Strategic Implication |
|--------|------------------|------------|------------------------|
| **Core Hours** | Weekday Morning/Afternoon (staffed hours) | **42.5%** | High intent → Optimise onboarding & space quality |
| **Unmanned Hours** | Night, Early Morning, or Weekend (unstaffed hours) | **33.9%** | Underperforming but large group → Improve off-hour experience |




---

## 📂 Project Scope & Data

- **Data Period**: May 2021 – Dec 2023  
- **Target Users**: Free trial users (3-day access, first-time only)  
- **Business Context**: The trial aims to convert new users into paying customers through real workspace experience.  
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
To identify behavioural signals that distinguish paying users—and use them to define strategic segments and improve conversion performance.




---

## 📊 Key Questions

1. Where in the trial funnel do users most frequently drop off?
2. What behavioural traits most strongly differentiate paying users from non-paying users?
3. How did user behaviour shift between the pandemic and post-pandemic periods?
4. Which user segments offer the best balance of conversion potential and feasibility?
5. What low-cost, high-impact actions can improve trial-to-paid conversion?


---

## 🧱 Data Schema & Feature Engineering

- **Raw Data Schema**  
   ![Shared Office_raw](https://github.com/user-attachments/assets/c4805fe0-a724-4660-a112-f4926d94f926)
  
  _Relational schema outlining how trial events, access logs, and payment outcomes are linked_


- **Feature Engineering & Data Mart**   
   ![Shared Office_modified](https://github.com/user-attachments/assets/661636af-942c-4ebc-ab15-659a23ab077e)
  
  _Final integrated dataset used for segmentation and modelling (user-level granularity)_

Key engineered features include:
- `first_visit_delay`: Days from sign-up to first visit
- `main_time_block`: Most frequent time block used (e.g., afternoon)
- `visit_day_type`: Weekday vs weekend usage
- `avg_density_scaled`: Trial congestion exposure, adjusted by site size
- `stay_hours`: Total stay time during trial

  
---

## 📉 Funnel Breakdown & Business Context


| Year | Signups (avg/mo) | Visitors (avg/mo) | Conversion Rate | Change |
|------|------------------|-------------------|-----------------|--------|
| 2021 | 351              | 218.6             | 42.8%           | Baseline |
| 2022 | 290.3            | 196.8             | 39.1%           | ▼3.7%p |
| 2023 | 277.7            | 201.8             | 33.6%           | ▼5.5%p |

> 📌 **Key Observations**
- In 2022, **sign-up volume declined** significantly (–17%), indicating weaker top-of-funnel demand.
- In 2023, **visit volume stabilised**, but the **conversion rate dropped sharply**—revealing a drop-off **after users experienced the service**.
- The primary issue is not attracting users or getting them to visit—it's converting them afterwards.

---

## 🦠 Pandemic vs Post-Pandemic Behaviour Shift

To ensure strategy reflects **current user intent**, May 2022 was used as the split point (when most COVID restrictions lifted).

| Period         | Signups (avg/mo) | Conversion Rate | Key Change |
|----------------|------------------|-----------------|------------|
| Pandemic       | 335.8            | 41.1%           | Reference |
| Post-Pandemic  | 279.8            | 36.1%           | ▼5.0%p    |

> **What Changed After the Pandemic?**
- **Trial usage dropped**: Users stayed fewer hours, visited fewer days.
- **Intent shifted**: From passive, exploratory usage → goal-driven visits.
- **Top predictors changed**: 
  - Pre-pandemic: Usage quantity (e.g. stay hours, visit days)
  - Post-pandemic: Timing-related features (e.g. weekday visits, 1–2 day delay)

 ![feature importance difference (pandemic vs post-pandemic)](https://github.com/user-attachments/assets/0336d8e5-ea6f-47ef-add9-d89db3d151da)
_Change in behavioural feature importance (2021–2023)_

> 🧭 **Strategic Implication**  
Behavioural models trained on pandemic-era data are no longer reliable.  
From this point forward, all segmentation and conversion analysis focuses **exclusively on post-pandemic users**, where intent is more autonomous and patterns are more stable.

---

## 🧪 Conversion Analysis: What Drives Payment in the Post-Pandemic Era?

To identify which behaviours most influence conversion, we trained an XGBoost classifier on post-pandemic user data.  
This model was optimised for **recall (0.72)** to uncover behavioural patterns, not for deployment accuracy.

#### 1. Top Behavioural Drivers (Feature Importance)

![feature importance (post-pandemic)](https://github.com/user-attachments/assets/8bb068ea-e0d2-4f9c-aa29-bddcd238d344)  
_Extracted from user-level features_

> 📌 **Top 3 Predictive Features**
1. `main_time_block`  
   → **When users visit** (e.g., morning, afternoon, night)  
   → Indicates structured vs unstructured usage patterns  
2. `visit_day_type`  
   → **Weekday vs weekend usage**  
   → Reflects purpose-driven vs casual trial behaviour  
3. `first_visit_delay`  
   → **Days between signup and first visit**  
   → Measures level of planning or spontaneity

These three features collectively account for over **55% of total importance** and form the foundation for contribution analysis in the next sections.

---

### 2. Behavioral Contribution Analysis

To identify which behaviors most strongly contribute to overall conversion performance, we used two complementary views:

1. **Simple Visualization**: Shows segment **volume and raw conversion rate** for easy interpretation  
2. **Contribution Score Table**: Uses **Z-score of conversion rate × segment share** to measure **impact on total conversion**

##### a. `main_time_block`

- Afternoon users show the **highest conversion rate (45.7%)** with moderate user volume.
- Early Morning users represent the **largest segment (42.6%)** but have **low conversion** → negative contribution.

**Visualization** (Volume + Raw Conversion Rate):  

![main time block - user count and conversion rate](https://github.com/user-attachments/assets/91ed6065-4b61-4730-a02b-8de0068df98f)

**Contribution Score Table**:

| Time Block       | Conversion Rate | Z (Conv.) | Volume Share | Contribution Score |
|------------------|------------------|-------------|----------------|----------------------|
| Afternoon        | 45.7%           | +2.11      | 13.1%         | **+0.277** ✅        |
| Morning          | 37.3%           | +0.06      | 32.2%         | +0.020               |
| Night            | 43.8%           | +1.63      | 0.4%          | +0.007               |
| Evening          | 37.8%           | +0.17      | 1.2%          | +0.002               |
| Midnight         | 36.3%           | –0.18      | 10.4%         | –0.019 ⚠️           |
| Early Morning    | 34.3%           | –0.67      | 42.6%         | **–0.287** ⚠️       |


**Interpretation**  
-  **Afternoon users**: Despite being only 13% of users, they show the **highest conversion rate (45.7%)** and largest positive contribution (+0.277).  
-  **Morning users**: Large group (32%), but conversion is average → small contribution (+0.020).  
-  **Early Morning users**: The **largest group (42.6%)**, but poor performance drags down overall average (–0.287).  
-  **Midnight users** also show below-average performance.  
-  **Night and evening users** have high conversion rates but very small volume → limited impact.
  
---

##### b. `visit_day_type`

- Weekday users are the **largest group (74.1%)** with solid performance → high contribution.
- Weekend-only users show **low conversion (28.5%)** and drag down average.

**Visualization**:  
![visit day type - user count and conversion rate](https://github.com/user-attachments/assets/d58c0741-ab56-4b78-ac03-e82f6134b961)


**Contribution Score Table**:

| Visit Day Type | Conversion Rate | Z (Conv.) | Volume Share | Contribution Score |
|----------------|------------------|-------------|----------------|----------------------|
| Weekday        | 38.4%           | +0.23      | 74.1%         | **+0.174** ✅        |
| Both           | 42.3%           | +0.90      | 8.8%          | +0.079               |
| Weekend        | 28.5%           | –1.47      | 17.2%         | **–0.253** ⚠️       |

**Interpretation**  
- 🔹 **Weekday users**: Largest group (74.1%) with stable conversion → strong contributor (+0.174)  
- 🔹 **Both weekday/weekend** visitors convert well, but low volume  
- 🔸 **Weekend-only users** show lowest conversion (28.5%) → negative contribution (–0.253)


---

##### c. `first_visit_delay`

- Delayed visits (1–2 days after signup) show **higher intent and conversion**.
- Same-day visits have high volume but **lowest conversion**, lowering overall performance.

**Visualization**:  
![first visit delay - user count and conversion ate](https://github.com/user-attachments/assets/f115cef0-f558-44ff-b2e5-06081c340074)


**Contribution Score Table**:

| First Visit Delay | Conversion Rate | Z (Conv.) | Volume Share | Contribution Score |
|--------------------|------------------|-------------|----------------|----------------------|
| 0 days             | 31.9%           | –1.41      | 53.9%         | **–0.3865** ⚠️     |
| 1 day              | 38.9%           | +0.63      | 36.1%         | **+0.3629** ✅      |
| 2 days             | 39.4%           | +0.78      | 9.9%          | +0.1194             |


**Interpretation**  
-  **Users who visited 1–2 days after signup** show **higher conversion rates (~39%)** with strong positive contribution.  
-  **Same-day visitors (0 days delay)**: Large group (54%) but lowest conversion (31.9%) → most negative contributor (–0.386)

---

### 3. Combined Segment Analysis

We then examined strategic combinations of the top 2–3 behavioral features:

- `main_time_block` × `visit_day_type` (12 segments)
- Later extended to include `first_visit_delay` for deeper insight

**Visualization**:  
Shows each group’s **volume** and **conversion rate** (not contribution score)
![contribution by behavioral combination](https://github.com/user-attachments/assets/5f7670b6-4b8c-45b3-9f6b-fdca497d5de0)


Contribution score for each group is calculated separately in Section 7.3.

## 🎯 Target Segment Strategy
Based on contribution analysis, two operationally distinct user groups were identified as strategic targets:

#### ✅ High-ROI Segment
**Definition**:
- `main_time_block ∈ {morning, afternoon}`  
- `visit_day_type ∈ {weekday, both}`

**Performance**:
- Trial Users: **1,325**
- Paid Users: **564**
- Conversion Rate: **42.57%**

**Interpretation**:
- These users visit during staffed business hours and are likely evaluating the space for productive use.
- Their behaviour is purposeful and structured—**indicating high conversion intent**.

**Recommended Actions**:
- Improve onboarding flow: signage, greeting, workspace orientation
- Ensure workspace readiness: lighting, Wi-Fi, cleanliness
- Follow up with satisfaction nudges or retention offers


#### ⚠️ At-Risk Segment
**Definition**:
- `main_time_block ∈ {early_morning, evening, night, midnight}`  
- OR `visit_day_type = weekend`

**Performance**:
- Trial Users: **2,290**
- Paid Users: **777**
- Conversion Rate: **33.93%**

**Interpretation**:
- These users visit outside staffed hours, often facing unclear onboarding, poor lighting, or service limitations.
- They represent a **high-volume group with conversion friction**, likely due to experience gaps.

**Recommended Actions**:
- Secure off-hour quality: cleanliness, lighting, wayfinding
- Provide QR-based orientation guides for unstaffed hours
- Pilot minimal staffing or monitoring during off-hour traffic spikes


---

## 📈 Strategy Impact Simulation

| Group | Users | Baseline CR | +1%p Gain | Total Impact |
|-------|-------|-------------|-----------|---------------|
| Core Hours | 1,327 | 42.5% | +13.3 | +0.35%p |
| Unmanned Hours | 1,560 | 34.4% | +15.6 | +0.41%p |
| **Total** | — | — | +28.9 | **+0.76%p** |

> Even a modest **+1%p conversion uplift** in each segment would result in:  
> • **+13.25 additional payers** from Core Hours  
> • **+22.90 additional payers** from Unmanned Hours  
> → **+0.95%p total conversion increase** without adding new users.


---
## ✅ Final Takeaways

- 📉 The drop in conversion wasn't due to lack of traffic—**but to low post-visit effectiveness**.
- 🟦 **Core Hours users** (weekday mornings/afternoons with delayed first visits) are the **most strategic segment**—with strong intent and high conversion potential.
- 🟥 **Unmanned Hours users** face service experience barriers despite high volume—indicating room for operational improvement.
- 💡 No need to expand user acquisition.  
  Instead, focus on **optimising intent and experience**—  
  and unlock a **+0.95%p conversion uplift with minimal cost**.

