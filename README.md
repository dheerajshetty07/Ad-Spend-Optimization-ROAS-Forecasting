# Forecasting Return on Ads Spend to Guide Daily Ad Decisions

---

##  Project Overview

This project develops a **predictive analytics framework** to forecast **next-day Return on Advertising Spend (ROAS)** for **Amazon Sponsored Products campaigns**.  

In highly dynamic, auction-based advertising environments, advertisers must make daily decisions about **budget pacing, bid adjustments, and campaign activation**. Traditional rule-based or reactive approaches fail to capture the **nonlinear, volatile behavior** of ROAS.

Our goal was to shift decision-making from *reactive* to *forward-looking* by identifying the **key drivers of next-day ROAS** and evaluating which modeling approaches generalize best over time

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide1.PNG)

---

## Business & Analytics Questions

**Primary Question**  
> Which campaign-level and product-level variables best predict next-day ROAS, and how can these insights inform budget pacing, bidding strategy, and campaign management?

**Supporting Questions**
- How stable are ROAS prediction patterns under time-based validation?
- Which features consistently emerge as the strongest predictors?
- Do nonlinear models outperform traditional linear approaches in this context? 


![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide2.PNG)

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide3.PNG)

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide3.PNG)

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide4.PNG)

---

## Data Description

The modeling dataset was built at the **ASIN × Day** level by integrating two Amazon Ads data sources:

### Data Sources
- **Sponsored Products – Product Ads Daily**
  - Impressions
  - Clicks
  - Cost
  - Attributed Conversions (14-day)
  - Attributed Sales (14-day)

- **Sponsored Products – Campaigns Daily**
  - Daily budget
  - Bidding strategy
  - Budget type
  - Campaign status

### Key Engineered Metrics
- **CTR** = Clicks ÷ Impressions  
- **CVR** = Conversions ÷ Clicks  
- **CPC** = Cost ÷ Clicks  
- **ROAS** = Attributed Sales ÷ Cost *(target variable)*
  
![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide5.PNG)

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide6.PNG)

---

## Data Cleaning & Preprocessing

- Standardized column names and date formats
- Safely handled division-by-zero cases (CTR, CVR, CPC, ROAS)
- Removed **682 zero-activity days** (no impressions, clicks, cost, or sales)
- Validated ASIN-date uniqueness and join integrity
- No global normalization applied; transformations handled within modeling pipelines

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide7.PNG)

---

## Exploratory Data Analysis (EDA)

Key EDA insights:

- **Strong long-tail distributions** in impressions, clicks, and cost
- **ROAS is highly volatile**, nonlinear, and weakly correlated with raw spend
- Engagement metrics (CTR, CVR) show tighter distributions
- Strong correlations among clicks, cost, and conversions
- Limited seasonality, with mild weekday effects

These patterns suggested that **tree-based ensemble models** would outperform linear approaches

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide8.PNG)

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide9.PNG)

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide9.PNG)

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide10.PNG)

---

## Modeling Methodology

### Train–Test Strategy
- **70 / 30 time-based split**
- Ensures realistic, forward-looking forecasting
- Prevents look-ahead bias

### Models Evaluated
- Linear Regression (OLS)
- Ridge & Lasso
- k-Nearest Neighbors
- Decision Trees
- **Random Forest**
- **XGBoost**
- Partial Least Squares (PLS – log-transformed ROAS)

### Evaluation Metrics
- RMSE (Root Mean Squared Error)
- MAE (Mean Absolute Error)
- R² (Variance Explained)

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide11.PNG)

---

## Model Performance Summary

| Model | RMSE | MAE | R² |
|------|------|-----|----|
| **XGBoost** | **18.21** | **1.91** | **0.85–0.88** |
| Random Forest | 23.02 | 1.55 | 0.86 |
| PLS (log ROAS) | 0.55* | 0.39* | 0.73* |

\* Metrics reported in log-space for PLS

**XGBoost** delivered the best overall accuracy, robustness, and stability across time.

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide12.PNG)

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide13.PNG)

---

## Feature Importance Insights

Across models, the most influential predictors were:

- **Total Cost**
- **Cost per Click (CPC)**
- **CTR**
- **Campaign Budget**
- **Spend & Impression Volume**

Key insight:
> ROAS exhibits **diminishing returns** — higher spend does not guarantee better performance unless engagement improves proportionally 

---

## Strategic Recommendations

### Budget Allocation
- Prioritize ASINs with strong **CTR and CVR**
- Reduce spend on high-CPC, low-engagement products
- Use forecasted ROAS to guide daily budget pacing

### Bidding Strategy
- Avoid aggressive bids when engagement quality is weak
- Increase bids selectively when predicted ROAS is high
- Control volatility by responding to predicted efficiency, not past performance

### Campaign Activation
- Pause campaigns during low-return windows
- Activate campaigns when forecasted ROAS exceeds efficiency thresholds

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide15.PNG)

---

## Limitations & Risks

- ROAS uses a **14-day attribution window**, which may blur short-term causality
- Daily aggregation masks within-day bid and budget changes
- Sparse ASIN activity can introduce noise
- Single time split may not capture seasonal regime shifts

Future improvements include rolling validation, lagged features, and uplift modeling.

![Image alt](https://github.com/dheerajshetty07/Ad-Spend-Optimization-ROAS-Forecasting/blob/main/Slides/Team%20Normalizers%20Powerpoint/Slide14.PNG)

---

## Next Steps

- Deploy XGBoost forecasts into daily campaign workflows
- Extend modeling with longer time horizons
- Incorporate causal and uplift-based optimization
- Move toward automated budget and bid recommendations

---

**Contributors:** Fahima Alizada, Marco Capoccia, Dheeraj Shetty, Sahaj Kaur  
**Date:** December 2025  

