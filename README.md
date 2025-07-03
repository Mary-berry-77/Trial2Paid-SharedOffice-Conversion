# 📘 Shared Office Trial Conversion Analysis

This project analyses behavioural log data from a shared office provider to uncover key drivers of trial-to-paid conversion and proposes actionable strategies based on user segmentation.

---

## 🧭 TL;DR (Quick Summary)

### 1. Conversion Issue  
From 2021 to 2023, the trial-to-paid conversion rate fell from **53% to 23%**, despite stable trial sign-ups.  
The key bottleneck occurs **after users visit the space**, indicating friction in the **product experience**, not acquisition.

### 2. Behavioural Shift  
Conversion signals evolved over time:  
- **Pandemic era**: Usage volume (longer stays, more visits) was key  
- **Post-pandemic**: Intent-driven behaviours became more predictive, such as:  
  - **Visit timing (weekday/afternoon)**  
  - **Delayed first visit (1–2 days)**

### 3. Key Segment Insights

| Segment         | Characteristics                          | Conversion | Strategic Implication                              |
|-----------------|-------------------------------------------|------------|-----------------------------------------------------|
| **Core Hours**  | Weekday Morning/Afternoon (staffed hours) | 42.5%      | High-intent users → Refine onboarding & space value |
| **Out-of-Hours**| Night, Early Morning, or Weekend          | 33.9%      | Large underperforming group → Fix ops frictions     |

---

## 1. Key Research Questions

1. **At which stages** do most trial users drop off, and what behaviours precede those drop-offs?  
2. **Which behavioural patterns** are most strongly associated with successful trial-to-paid conversion?  
3. **How have trial user behaviours changed** between the pandemic and post-pandemic periods?  
4. **Which behavioural features** are most predictive of conversion, and how can they be used to design an effective targeting strategy?  
5. **Which user segments** offer the best balance of conversion potential and strategic feasibility?  

---

## 2. Project Scope & Data

- **Period:** May 2021 – Dec 2023  
- **Target Users:** Free trial users (3-day access)  
- **Business Model:** Trial-to-paid funnel based on first-hand experience  
- **Data Sources:**  
  - Sign-up logs  
  - Visit history  
  - In/out access events  
  - Payment status  
  - Site metadata  

> 🎯 **Goal:**  
> Discover behavioural patterns that differentiate converting users and use them to guide segment-level strategies.

---

## 3. Data Schema & Feature Engineering

### 3.1 Raw Data Schema  
Tracks the link between trial registration, behaviour, and payment outcome.  

<img src="https://github.com/user-attachments/assets/c4805fe0-a724-4660-a112-f4926d94f926" width="500"/>

### 3.2 Feature Engineering  
Created a user-level dataset optimised for behavioural segmentation and prediction.  

<img src="https://github.com/user-attachments/assets/661636af-942c-4ebc-ab15-659a23ab077e" width="500"/>

**Key Features:**
- `first_visit_delay`: Days between signup and first visit  
- `main_time_block`: Primary time-of-day usage  
- `visit_day_type`: Weekday vs weekend usage  
- `avg_density_scaled`: Congestion exposure (normalised)  
- `stay_hours`: Total stay time across 3-day trial  

---

## 4. Funnel Overview & Business Context

### 4.1 Funnel Drop-off Summary (2021–2023)

| Year | Sign-ups (avg/mo) | Visitors (avg/mo) | Trial-to-Paid Conversion Rate | Change   |
|------|-------------------|-------------------|-------------------------------|----------|
| 2021 | 351               | 218.6             | 42.8%                         | Baseline |
| 2022 | 290.3             | 196.8             | 39.1%                         | ▼3.7%p   |
| 2023 | 277.7             | 201.8             | 33.6%                         | ▼5.5%p   |

> 🔍 **Interpretation:**  
> Despite relatively stable acquisition and visitation, conversion dropped significantly — especially in 2023.  
> The issue lies in **post-visit experience**, not marketing.  
> → Focus must shift from “more users” to **“better conversion of the right users”**.

---

### 4.2 Funnel Churn & Conversion Trend Dashboard

<img src="https://github.com/user-attachments/assets/4169a524-d19a-4328-9d66-79f18b9677d0" width="700"/>

### 4.3 Key Churn Rates

| Metric              | Value   | Interpretation                                     |
|---------------------|---------|----------------------------------------------------|
| **Visit Churn**     | 32.1%   | 1 in 3 trial sign-ups never visit the space        |
| **Conversion Churn**| 62.1%   | 6 in 10 trial visitors do not convert to payment   |

> 💡 **Insight:**  
> The majority of churn occurs **after the visit**.  
> Strategy must address onboarding clarity, space experience, and follow-up engagement.

---

### 4.4 Monthly Conversion Rate Trend (2021.06–2023.12)

- **2021-06:** 52.96%  
- **2023-11:** 23.17%  
→ A consistent 30 percentage point decline, especially steep in late 2023.

> 📌 **Note:**  
> Indicates **structural behavioural changes**, not temporary fluctuations.  
> → Time to re-evaluate the **entire conversion experience pipeline**.

---

### 4.5 Trial Sign-ups vs. Conversion Trend

- Sign-ups remain steady  
- Conversions drop → widening gap

> 🔎 **Interpretation:**  
> The issue is not user interest — it’s the **failure to convert interested users into paying ones**.

---

## 5. Behavioural Shift: Pandemic vs Post-Pandemic

### 5.1 Period Split: May 2022  
Chosen based on Korea’s official lifting of COVID restrictions.

| Period         | Sign-ups (avg/mo) | Conversion Rate | Change |
|----------------|-------------------|------------------|--------|
| Pandemic       | 335.8             | 41.1%            | —      |
| Post-Pandemic  | 279.8             | 36.1%            | ▼5.0%p |

<img src="https://github.com/user-attachments/assets/0336d8e5-ea6f-47ef-add9-d89db3d151da" width="600"/>

> 📌 **Insight:**  
> Post-pandemic users behave with more intent — visiting on weekdays and delaying their first visit.  
> Pandemic-era data is biased by lockdown disruptions.  
> → All modelling and strategy henceforth focuses **only on post-pandemic data**.

---

## 6. Conversion Driver Deep Dive (Post-Pandemic Only)

### 6.1 Methods Used
- XGBoost for feature importance  
- Z-score × volume for contribution analysis  
- Logistic regression for individual behavioural effect

### Top Features (XGBoost Output)

<img src="https://github.com/user-attachments/assets/8bb068ea-e0d2-4f9c-aa29-bddcd238d344" width="500"/>

> 🧠 **Interpretation:**  
> **When** users visit matters more than **how much** they use.  
> Intent-based behaviours (timing, planning) outperform habitual ones.

---

### 6.2 Feature Contribution (Z-score × Volume Share)

#### a. `main_time_block`

<img src="https://github.com/user-attachments/assets/91ed6065-4b61-4730-a02b-8de0068df98f" width="500"/>

| Time Block     | Conv. Rate | Z-Score | Volume Share | Contribution     |
|----------------|------------|---------|---------------|------------------|
| Afternoon      | 45.7%      | +2.11   | 13.1%         | +0.277 (↑ high)  |
| Early Morning  | 34.3%      | –0.67   | 42.6%         | –0.287 (↓ low)   |
| Morning        | 37.3%      | +0.06   | 32.2%         | +0.020           |

> ⚠️ **Issue:** Early Morning has the largest volume but drags down conversion.

---

#### b. `visit_day_type`

<img src="https://github.com/user-attachments/assets/d58c0741-ab56-4b78-ac03-e82f6134b961" width="500"/>

| Day Type | Conv. Rate | Z-Score | Volume Share | Contribution     |
|----------|------------|---------|---------------|------------------|
| Weekday  | 38.4%      | +0.23   | 74.1%         | +0.174 (↑)       |
| Weekend  | 28.5%      | –1.47   | 17.2%         | –0.253 (↓)       |

> 🔎 **Insight:** Weekend usage has lower intent — consider nudging to weekday use.

---

#### c. `first_visit_delay`

<img src="https://github.com/user-attachments/assets/f115cef0-f558-44ff-b2e5-06081c340074" width="500"/>

| Delay    | Conv. Rate | Z-Score | Volume Share | Contribution     |
|----------|------------|---------|---------------|------------------|
| 0 days   | 31.9%      | –1.41   | 53.9%         | –0.386 (↓ risk)  |
| 1 day    | 38.9%      | +0.63   | 36.1%         | +0.363 (↑ boost) |

> 💡 **Tip:** Encourage 1–2 day planning delay via nudges for better conversion.

---

### 6.3 Combined Segment Analysis

<img src="https://github.com/user-attachments/assets/5f7670b6-4b8c-45b3-9f6b-fdca497d5de0" width="500"/>

> 🔍 Combining key features reveals **high-potential microsegments**  
> Ideal for targeted UX, operational, and marketing interventions.

---

## 7. Strategic Target Segments

### 7.1 High-ROI Group

| Segment Definition                      | Users | Conversion |
|-----------------------------------------|--------|------------|
| Weekday Morning/Afternoon + 1–2 day delay | 1,325  | 42.57%     |

> ✅ **Action:**  
> Optimise onboarding and reinforce value for these high-intent users.  
> Use automated nudges to encourage delayed visits.

---

### 7.2 At-Risk Group

| Segment Definition                   | Users  | Conversion |
|--------------------------------------|--------|------------|
| Early Morning / Midnight / Weekend   | 2,290  | 33.93%     |

> ⚠️ **Action:**  
> Enhance experience during low-staffed hours through:  
> - Digital onboarding & guidance  
> - Scheduled follow-ups  
> - Minimal but visible support presence

---

### 7.3 Impact Simulation

| Group          | Users  | Baseline CR | +1%p Gain | Conversion Uplift |
|----------------|--------|--------------|-----------|--------------------|
| Core Hours     | 1,327  | 42.5%        | +13.3     | +0.35%p            |
| Out-of-Hours   | 1,560  | 34.4%        | +15.6     | +0.41%p            |
| **Total**      | —      | —            | **+28.9** | **+0.76%p**        |

> 🧮 **Insight:**  
> Small, focused improvements to just two groups could lift overall conversion by +0.76%p  
> → **No new users required. Just better engagement.**

---

## 8. Final Takeaways

- 📉 The **main bottleneck lies post-visit**, not in acquisition  
- 🟦 Weekday + delayed visit users convert well → **focus effort here**  
- 🟥 Out-of-hours users are high-volume but low-yield → **improve support experience**  
- 💡 Sustainable growth comes from **serving the right users better**, not spending more

---

## 9. Strategic Implications for Teams

| Stakeholder     | Key Takeaway                                              |
|-----------------|-----------------------------------------------------------|
| **Product**     | Streamline onboarding; highlight value for key segments   |
| **Operations**  | Improve off-hour support (automation or minimal staffing) |
| **Marketing**   | Target weekday, delayed-intent users rather than all      |
| **Leadership**  | Focus investment on user experience, not just acquisition |

---
