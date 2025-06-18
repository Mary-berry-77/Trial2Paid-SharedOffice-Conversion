# 📊 Trial → Paid: Behavioral Analysis & Strategy for Shared Office Conversion

## 📌 TL;DR – Quick Summary

This project analyzes behavioral log data from a shared office trial service to identify key patterns that influence conversion from trial to paid users.

---

### 🎯 Goal

To improve the overall conversion rate by identifying high-impact behavioral segments and suggesting data-driven strategies for service and marketing optimization.

---

### ❓ What Business Questions Does This Project Answer?

- **Where is the biggest drop-off in the trial-to-payment funnel?**  
  → Only ~39% of visitors convert to paid users → post-visit experience is the core bottleneck.

- **Which behavioral traits most influence conversion?**  
  → Factors like time of first visit, main usage time block, and weekday/weekend usage.

- **How did user behavior change after the pandemic?**  
  → Post-pandemic users show clearer segmentation between paying and non-paying behavior.

- **Can we go beyond simple conversion rates to prioritize target segments?**  
  → Yes. We used Z-score × user share to quantify the actual business contribution of each segment.

- **Can machine learning help identify high-potential users?**  
  → Yes. An XGBoost model (ROC-AUC ~0.65) supports predictive targeting of high-conversion users.

---

### 📊 Project Summary

- **Period:** 2021.05 – 2023.12  
- **Data Sources:** Trial registration, visit logs, access logs, congestion levels, payment status  
- **Methods Used:** Funnel analysis, statistical tests, logistic regression, Random Forest, Z-score contribution analysis

---

### 🧠 Key Analyses

- AARRR funnel analysis to identify bottlenecks  
- Behavior shift analysis: Pandemic vs Post-pandemic  
- Z-score-based contribution scoring (conversion rate × user share)  
- Logistic regression & feature importance modeling  
- Strategic target segment identification  

---

### 💡 Key Findings

- Users who visited 1 day after signup during weekday mornings/afternoons had the highest conversion rate  
- Early morning (same-day) visitors had lowest contribution despite high volume  
- Post-pandemic behaviors became more intentional and structured, demanding era-specific strategies  

---

### 📈 Impact

- Identified high-ROI segments for targeted conversion improvement  
- Suggested operational enhancements (e.g. staffing, cleaning) during key time slots  
- Delivered a strategic roadmap integrating behavioral insights, modeling, and business actions
