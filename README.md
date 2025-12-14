# Customer Acquisition and Duration Prediction Analysis

A comprehensive machine learning analysis examining the drivers of B2B customer acquisition and relationship longevity.

## Executive Summary

This analysis develops a two-stage predictive framework to understand what drives successful customer acquisition and long-term relationship duration in a B2B context. Using machine learning techniques, the study identifies key drivers, quantifies their effects, and provides actionable insights for customer targeting and resource allocation.

**Key Findings:**
- Company size is the dominant acquisition driver (21% to 90% probability range)
- Acquisition expenditure significantly impacts success (35% to 81% probability range)
- Retention investment and purchase frequency determine relationship longevity
- Combined models achieve 80% acquisition prediction accuracy and 97.8% duration prediction accuracy

## Business Problem

Organizations face important decisions about:
1. Which prospects to target for acquisition efforts
2. How much to invest in acquisition marketing
3. Which acquired customers will generate long-term value
4. How to estimate customer lifetime value before acquisition

This analysis addresses these questions through predictive modeling and effect quantification.

## Analysis Overview

### Two-Stage Modeling Framework

**Stage 1: Acquisition Prediction**
- Predicts which prospects are most likely to become customers
- Uses pre-acquisition firmographic data only (employees, revenue, industry, acquisition spend)
- Prevents data leakage by excluding post-acquisition behavioral metrics

**Stage 2: Duration Prediction**
- Forecasts relationship longevity for acquired customers
- Incorporates both firmographic and behavioral engagement metrics
- Enables customer lifetime value estimation

## Data Description

Analysis based on 338 B2B customer records with:

**Firmographic Variables:**
- Company size (18 to 1,461 employees)
- Annual revenue ($16M to $965M)
- Industry classification
- Acquisition marketing expenditure ($1.22 to $970.31)

**Behavioral Variables (post-acquisition):**
- Relationship duration (654 to 1,673 days)
- Purchase frequency
- Profit margin
- Cross-buying behavior
- Share of wallet
- Retention expenditure

**Sample Split:**
- Training set: 245 customers
- Test set: 93 customers

## Methodology

### Model Comparison

Three approaches were evaluated for acquisition prediction:

| Model | Accuracy | AUC | Interpretation |
|-------|----------|-----|----------------|
| **Logistic Regression** | 80.0% | 0.849 | High - clear coefficients |
| Random Forest | 76.7% | 0.821 | Medium - variable importance |
| Decision Tree | 73.3% | 0.743 | High - visual rules |

**Winner:** Logistic Regression balances strong performance with interpretability for business decision-making.

### Critical Methodological Insight: Data Leakage

Initial models including all variables achieved unrealistic 100% accuracy. Investigation revealed that post-acquisition behavioral metrics (frequency, cross-buying) were inadvertently included, allowing the model to "cheat" by using information unavailable at prediction time.

**Solution:** Strict temporal feature separation ensures models use only information available at the time predictions would be made in production.

## Key Results

### Acquisition Drivers

**Partial Dependence Analysis** quantifies the effect of each variable:

| Variable | Effect Range | Interpretation |
|----------|--------------|----------------|
| **Employees** | 21% → 90% | Strongest driver: company size increases acquisition 4.3x |
| **Acquisition Expenditure** | 35% → 81% | Marketing investment increases probability 2.3x |
| **Revenue** | 51% → 85% | Financial scale increases probability 1.7x |
| **Industry** | 64% → 75% | Sector has minimal effect (1.2x) |

**Logistic Regression Coefficients:**
- All variables except acquisition expenditure showed statistical significance (p < 0.001)
- Employees: β = 0.0067 (each additional employee increases log-odds by 0.0067)
- Industry category 1: β = 1.27 (substantial positive effect)
- Acquisition expenditure: Not significant in linear model (p = 0.338), but PDP shows strong non-linear effect

**Interpretation:** The non-significance of acquisition expenditure in logistic regression despite strong PDP effects suggests threshold or non-linear relationships that ensemble methods capture better than linear models.

### Duration Drivers

**Random Forest Regression Results:**
- R² = 0.978 (explains 97.8% of variance)
- RMSE = 32.9 days (approximately 1 month average error)
- Predictions are highly accurate for relationships lasting 2-4.5 years

**Variable Importance Rankings:**

| Variable | Importance (% IncMSE) | Interpretation |
|----------|----------------------|----------------|
| **Retention Expenditure** | 21.6% | Dominant predictor: ongoing investment matters most |
| Retention Expenditure² | 21.3% | Non-linear relationship (squared term) |
| Purchase Frequency² | 13.4% | Engagement frequency drives longevity |
| Purchase Frequency | 11.3% | Linear and non-linear effects both present |
| Profit Margin | 11.1% | Profitability predicts sustainability |
| Employees | 3.7% | Firmographics become less relevant post-acquisition |
| Cross-buying | -0.85% | Negative importance: adds noise, not signal |

**Key Insight:** Once a customer is acquired, their *engagement behavior and profitability* determine relationship longevity, not their firmographic profile. Company characteristics that drive acquisition become largely irrelevant for predicting duration.

## Strategic Implications

### 1. Precision Targeting
Focus acquisition efforts on:
- **Large companies** (>500 employees): 70%+ acquisition probability
- **High-revenue firms** (>$200M): Moderate probability boost
- **Adequate marketing investment**: Threshold effects suggest minimum spend requirements

**Avoid over-investing in:**
- Small firms (<100 employees): <30% acquisition probability regardless of spend
- Industry-based targeting: Minimal differentiation (11 percentage points)

### 2. Marketing Budget Optimization
- Acquisition expenditure shows **threshold effects** rather than linear returns
- Under-funded campaigns (<$50) show dramatically lower success rates
- Focus on *strategic allocation* to high-potential prospects rather than uniform spending increases

### 3. Post-Acquisition Retention Strategy
- **Prioritize retention investment**: 21.6% importance for duration prediction
- **Encourage frequent transactions**: 13.4% importance for engagement
- **Monitor profitability**: 11.1% importance signals relationship health
- **Don't over-emphasize cross-selling**: Negative importance suggests it may distract from core relationship building

### 4. Customer Lifetime Value Estimation

The two-stage framework enables pre-acquisition CLV calculation:
```
Expected CLV = P(acquisition) × Predicted Duration × Average Daily Profit
```

**Example:**
- Prospect: 800 employees, $300M revenue, $200 acquisition spend
- Predicted acquisition probability: 85%
- Predicted duration (if acquired): 1,200 days
- Average daily profit: $500
- **Expected CLV = 0.85 × 1,200 × $500 = $510,000**

This enables sophisticated ROI analysis comparing acquisition costs to expected lifetime value.

## Model Performance Summary

### Acquisition Model (Logistic Regression)
- **Accuracy:** 80% (4 out of 5 acquisitions correctly identified)
- **AUC:** 0.849 (strong discrimination between acquired/not acquired)
- **Interpretability:** High (clear coefficient interpretation)
- **Use Case:** Real-time prospect scoring, sales prioritization

### Duration Model (Random Forest)
- **R²:** 0.978 (explains 97.8% of variance)
- **RMSE:** 32.9 days (±1 month prediction error)
- **Interpretability:** Medium (variable importance rankings)
- **Use Case:** CLV forecasting, retention strategy planning

## Limitations

1. **Static predictions**: Models don't capture temporal dynamics or changing customer behavior over time
2. **Linear assumptions**: Logistic regression may miss complex variable interactions
3. **Sample size**: 338 customers limits ability to detect subtle effects or rare patterns

## Future Research Directions

1. **Survival Analysis**: Incorporate time-varying covariates to model changing customer behavior throughout relationship lifecycle
2. **Multi-Industry Validation**: Test framework across different sectors to establish generalizability
3. **Field Experiments**: Conduct A/B tests using model-guided targeting to validate real-world impact
4. **Deep Learning**: Explore neural networks for capturing complex non-linear relationships
5. **Causal Inference**: Move beyond prediction to understand causal effects of interventions (e.g., retention spending increases)

## Technical Details
  
**Key Packages:** caret, randomForest, rpart, pROC, pdp, tidyverse  
**Analysis Date:** November 2025 
**Train/Test Split:** 72.5% / 27.5% random split


## Conclusions

This analysis demonstrates that customer acquisition and duration are highly predictable using appropriate features and modeling techniques. The two-stage framework provides actionable insights for:

- **Targeting**: Focus on large companies with adequate marketing investment
- **Budgeting**: Allocate spend strategically based on prospect potential
- **Retention**: Prioritize ongoing investment and engagement frequency
- **Valuation**: Estimate customer lifetime value before acquisition

Organizations implementing these insights can expect substantial improvements in marketing efficiency, customer lifetime value, and overall profitability through data-driven targeting and engagement strategies.
