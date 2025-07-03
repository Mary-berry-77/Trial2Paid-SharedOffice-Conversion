# 📘 Shared Office Trial Conversion Analysis

This project analyses behavioural log data from a shared office provider to uncover key drivers of trial-to-paid user conversion and proposes actionable strategies based on user segmentation.

---

### 🧭 TL;DR (Quick Summary)

#### 1. Conversion Issue  
From 2021 to 2023, the trial-to-paid conversion rate fell from **53% to 23%**, despite stable trial sign-ups.  
The key bottleneck lies **after the first visit**, not before — indicating a product or experience-related friction rather than a marketing issue.

#### 2.  Behavioural Shift  
Conversion signals shifted over time:  
- **Pandemic era**: Usage volume (longer stays, more visits) was key  
- **Post-pandemic**: Behavioural intent signals matter more, such as:  
  - **Visit timing (weekday/afternoon)**  
  - **Delayed first visit (1–2 days)**

#### 3.  Key Segment Insights

| Segment         | Characteristics                          | Conversion | Strategic Implication                        |
|-----------------|-------------------------------------------|------------|-----------------------------------------------|
| **Core Hours**  | Weekday Morning/Afternoon (staffed hours) | 42.5%      | High-intent users → Refine onboarding, boost workspace appeal |
| **Out-of-Hours**| Night, Early Morning, or Weekend          | 33.9%      | Underperforming but large group → Target with low-cost operational fixes |

---
### 1. Project Overview

#### 1.1 Context  
The study uses log-level data from a commercial shared office provider, capturing user registrations, visits, access records, and payment outcomes between May 2021 and December 2023.

#### 1.2 Objective  
To identify which user behaviours most strongly correlate with conversion and propose practical ways to increase paid user conversion.

#### 1.3 Scope  
Analysis includes:
- Funnel performance across stages
- Pandemic vs post-pandemic behavioural changes
- Feature importance and segment contribution
- Actionable strategies targeting high-impact user groups

### 2. Project Scope & Data

- **Period**: May 2021 – Dec 2023  
- **Target**: Free trial users (3-day access, first-time only)  
- **Business Context**: Trial aims to convert users via first-hand experience  
- **Data Sources**:  
  - Sign-up logs  
  - Visit history  
  - In/out access events  
  - Payment status  
  - Site metadata  

> 🎯 **Goal**:  
> Identify behavioural patterns that distinguish paying users and use them to build effective, segment-driven strategies.

---

### 3. Data Schema & Feature Engineering

#### 3.1 Raw Data Schema  
_Tracks the relationship between trial sign-up, behaviour, and payment outcomes._

<img src="https://github.com/user-attachments/assets/c4805fe0-a724-4660-a112-f4926d94f926" width="500"/>

#### 3.1 Feature Engineering  
_Created a user-level dataset optimised for behavioural segmentation and prediction._

<img src="https://github.com/user-attachments/assets/661636af-942c-4ebc-ab15-659a23ab077e" width="500"/>

**Key Features:**
- `first_visit_delay`: Time taken to start using the trial  
- `main_time_block`: Most used time window (e.g. afternoon)  
- `visit_day_type`: Weekday vs weekend usage  
- `avg_density_scaled`: Congestion exposure normalised by site  
- `stay_hours`: Total stay time over trial period  

---

### 4. Funnel Overview & Business Context

#### 4.1 Funnel Drop-off Summary (2021–2023)

| Year | Sign-ups (avg/mo) | Visitors (avg/mo) | Conversion Rate | Change   |
|------|-------------------|-------------------|------------------|----------|
| 2021 | 351               | 218.6             | 42.8%            | Baseline |
| 2022 | 290.3             | 196.8             | 39.1%            | ▼3.7%p   |
| 2023 | 277.7             | 201.8             | 33.6%            | ▼5.5%p   |

> **Interpretation:**  
> While acquisition and visitation remained relatively stable, the conversion rate continued to drop — especially in 2023.  
> This confirms that **experience post-visit, not acquisition volume, is the core issue**.  
> → Strategy must shift from “bring more users” to **“convert the right ones better”**.


#### 4.2 Dashboard: Funnel Churn & Conversion Trend

<img src="https://github.com/user-attachments/assets/459fcc23-ca9d-4117-901b-019cff641b0e" width="700"/>

##### 4.3 Key Churn Rates

| Metric              | Value   | Interpretation |
|---------------------|---------|----------------|
| **Visit Churn**     | 32.1%   | 3 out of 10 users who sign up for trial do not visit the space |
| **Conversion Churn**| 62.1%   | Over 6 out of 10 trial visitors do not convert into paying users |

> **Insight:**  
> Most of the churn happens **after the trial visit**.  
> This highlights post-visit friction, indicating the need for improvements in onboarding, space experience, or follow-up strategies.


#####  4.4 Monthly Conversion Rate Trend (2021.06–2023.12)

- **2021-06**: 52.96%  
- **2023-11**: 23.17%  
- Downward trend of ~30%p, particularly sharp in late 2023

> **Strategic Note:**  
> This persistent decline suggests **structural issues**, not just short-term noise.  
> Likely causes include user behaviour shift, operational challenges, and market saturation.  
> → Calls for a **fundamental review** of the trial experience pipeline.

---

##### 4.5 3-Day Trial Sign-ups vs. Conversion Trend

- Sign-ups remain volatile but generally steady  
- Conversions **continue to decline**, showing widening gap

> **Interpretation:**  
> While marketing still brings users in, the **product experience fails to seal the deal**.  
> → Focus must shift to post-sign-up experience: onboarding clarity, motivation reinforcement, and space quality assurance.

---

### 5. Behavioural Shift: Pandemic vs Post-Pandemic

#### 5.1 Split: May 2022 (when COVID restrictions were lifted)

| Period         | Sign-ups (avg/mo) | Conversion Rate | Change |
|----------------|-------------------|------------------|--------|
| Pandemic       | 335.8             | 41.1%            | —      |
| Post-Pandemic  | 279.8             | 36.1%            | ▼5.0%p |

<img src="https://github.com/user-attachments/assets/0336d8e5-ea6f-47ef-add9-d89db3d151da" width="600"/>

> **Interpretation:**  
> Post-pandemic users behave more autonomously and purposefully — they prefer weekday usage and delay their first visit by 1–2 days.  
> Pandemic-era data reflects irregular usage patterns due to lockdown effects.  
> → Henceforth, all modelling and strategy focus exclusively on post-pandemic behaviour.

---

### 6. Conversion Driver Deep Dive (Post-Pandemic Only)

#### 6.1 Behavioural signals were analysed using:
- XGBoost (for feature importance)
- Z-score × Volume contribution
- Logistic regression (for directionality)


#### Top Predictive Features (XGBoost)

<img src="https://github.com/user-attachments/assets/8bb068ea-e0d2-4f9c-aa29-bddcd238d344" width="500"/>

> **Interpretation:**  
> Timing matters more than volume.  
> Weekday visits, afternoon hours, and 1–2 day planning delays are strong predictors of payment.  
> These signals indicate **intent-driven usage** over habitual or impulsive use.

---

#### 6.2 Contribution Analysis by Feature (Z-score × Volume Share)

##### a. `main_time_block`

<img src="https://github.com/user-attachments/assets/91ed6065-4b61-4730-a02b-8de0068df98f" width="500"/>

| Time Block     | Conv. Rate | Z-Score | Volume Share | Contribution |
|----------------|------------|---------|---------------|--------------|
| Afternoon      | 45.7%      | +2.11   | 13.1%         | +0.277 ✅     |
| Early Morning  | 34.3%      | –0.67   | 42.6%         | –0.287 ⚠️    |
| Morning        | 37.3%      | +0.06   | 32.2%         | +0.020        |

> **Insight:**  
> The Early Morning group is the largest segment but underperforms significantly.  
> Optimising this time slot experience could unlock large conversion gains.



##### b. `visit_day_type`

<img src="https://github.com/user-attachments/assets/d58c0741-ab56-4b78-ac03-e82f6134b961" width="500"/>

| Type     | Conv. Rate | Z-Score | Volume Share | Contribution |
|----------|------------|---------|---------------|--------------|
| Weekday  | 38.4%      | +0.23   | 74.1%         | +0.174 ✅     |
| Weekend  | 28.5%      | –1.47   | 17.2%         | –0.253 ⚠️    |

> **Insight:**  
> Weekend users represent a hidden loss centre.  
> Improving weekend experience or rerouting them toward weekday slots could boost overall performance.


##### c. `first_visit_delay`

<img src="https://github.com/user-attachments/assets/f115cef0-f558-44ff-b2e5-06081c340074" width="500"/>

| Delay    | Conv. Rate | Z-Score | Volume Share | Contribution |
|----------|------------|---------|---------------|--------------|
| 0 days   | 31.9%      | –1.41   | 53.9%         | –0.386 ⚠️    |
| 1 day    | 38.9%      | +0.63   | 36.1%         | +0.363 ✅     |

> **Insight:**  
> Immediate visits underperform. Encouraging a 1–2 day delay (e.g., via nudges) is a **simple but high-leverage tactic**.

---

#### 6.3 Combined Segment Analysis

<img src="https://github.com/user-attachments/assets/5f7670b6-4b8c-45b3-9f6b-fdca497d5de0" width="500"/>

> **Interpretation:**  
> Combining `main_time_block`, `visit_day_type`, and `first_visit_delay` reveals **high-ROI micro-segments**.  
> These combinations outperform any single variable and form the foundation for strategic targeting.

---

### 7.Strategic Target Segments

#### 7.1 High-ROI Group

| Segment Definition                      | Users | Conversion |
|-----------------------------------------|--------|------------|
| Weekday Morning/Afternoon + 1–2 day delay | 1,325  | 42.57%     |

> **Action:**  
> These users plan and visit during ideal hours.  
> Optimise onboarding, reinforce workspace value, and automate delayed-visit nudges.


#### 7.2 At-Risk Group

| Segment Definition                   | Users | Conversion |
|--------------------------------------|--------|------------|
| Early Morning / Midnight / Weekend   | 2,290  | 33.93%     |

> **Action:**  
> Improve experience during low-staffed hours via:  
> - Digital onboarding  
> - Scheduled follow-ups  
> - Minimal night/weekend support



#### 7.3 Impact Simulation

| Group          | Users | Baseline CR | +1%p Gain | Conversion Uplift |
|----------------|--------|--------------|-----------|--------------------|
| Core Hours     | 1,327  | 42.5%        | +13.3     | +0.35%p            |
| Out-of-Hours   | 1,560  | 34.4%        | +15.6     | +0.41%p            |
| **Total**      | —      | —            | **+28.9** | **+0.76%p**        |

> **Interpretation:**  
> Small improvements in just two segments could lift total conversion rate by +0.76%p — without acquiring a single new user.  
> → Low-cost, **operationally focused growth** is achievable.

---

### 8. Final Takeaways

- 📉 The **main conversion bottleneck lies post-visit**, not in marketing or acquisition  
- 🟦 Weekday + delayed visit users convert well — **focus efforts here**  
- 🟥 Out-of-hours usage is **high volume but low return** — **fix operational friction**  
- 💡 Growth doesn’t always require more users — it comes from **serving the right ones better**

---


