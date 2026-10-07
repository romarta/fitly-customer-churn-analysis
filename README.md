# Fit.ly Customer Churn Analysis & Retention Monitoring

End-to-end customer churn analysis combining account, customer support, and user activity data to identify patterns associated with cancellations and translate them into actionable retention recommendations. The project also includes a Power BI prototype for monitoring monthly churn and related operational KPIs.

## Project Overview

Fit.ly is a subscription-based fitness app experiencing increasing customer churn. The goal of this project was to understand which customer characteristics and behaviors are associated with churn, identify areas for retention action, and define a business KPI that can be monitored over time.

The analysis combines three source datasets:

- customer account and subscription information,
- customer support interactions,
- user activity events.

The final customer-level analytical dataset contains 400 customers and integrates 918 support tickets and 445 recorded activity events.

## Business Questions

The analysis focused on four questions:

1. Does churn differ across subscription plans?
2. Is customer support contact associated with churn, and does resolution time matter?
3. How is the number of recorded user activities associated with churn?
4. Are specific types of activity associated with different churn levels?

## What I Did

### 1. Data validation and cleaning

- validated identifiers, missing values, categories, numeric ranges, and date fields,
- checked duplicate records and data consistency,
- converted date columns to datetime format,
- standardized customer identifiers across source tables,
- replaced non-standard support channel values with a consistent category,
- documented assumptions made when handling missing churn-status values.

### 2. Data preparation and integration

- created customer-level summaries for support and activity data,
- aggregated one-to-many tables before joining them to the account table,
- used left joins to preserve the full customer population and avoid row multiplication,
- created features such as ticket counts, average/max resolution time, activity counts, and activity-type indicators.

### 3. Exploratory data analysis

- analyzed distributions of subscription plans, prices, churn status, support tickets, support channels, activity counts, and activity types,
- compared churn across customer segments,
- created univariate and multivariate visualizations,
- explored relationships between churn, engagement, plan type, and support experience.

### 4. Statistical analysis

The project used inferential statistics to test whether observed differences were statistically meaningful:

- **Chi-square tests of independence** for churn vs. subscription plan, support-ticket history, and activity-count groups,
- **Cramér's V** to assess effect size for categorical associations,
- **Welch's t-test** to compare average support resolution time between churned and non-churned customers,
- **Fisher's exact tests** for activity-type comparisons,
- **Holm correction** for multiple hypothesis testing,
- **correlation analysis** after binary / one-hot encoding of relevant variables.

### 5. Business KPI design

The recommended KPI is **Monthly Customer Churn Rate**:

```text
Monthly Churn Rate =
Customers who cancelled during the month
------------------------------------------------ x 100
Customers active at the beginning of the month
```

The original source data did not contain cancellation dates or historical monthly active-customer counts, so the true monthly churn rate could not be calculated from the original dataset.

### 6. Power BI monitoring prototype

A Power BI dashboard was created to demonstrate how churn could be monitored operationally. It includes:

- monthly churn rate and trend,
- target comparison,
- new subscriptions,
- active subscriptions,
- monthly cancellations,
- subscription-plan breakdowns,
- average activities per customer,
- average support resolution time,
- churn by activity count,
- resolution time by churn status.

> **Important:** the original data did not include the signup / cancellation history required for monthly churn monitoring. Synthetic signup and cancellation dates were therefore created for the dashboard prototype. Monthly values in the Power BI monitoring dataset are illustrative and should not be interpreted as historical Fit.ly performance.

## Tools and Technologies

- **Python**
- **pandas** — data cleaning, transformation, aggregation, and joins
- **NumPy** — numerical operations
- **Matplotlib / Seaborn** — exploratory visualizations
- **SciPy** — statistical hypothesis testing
- **statsmodels** — multiple-testing correction
- **Power BI** — KPI monitoring and interactive dashboard
- **DAX** — churn-rate and dashboard calculations

## Key Findings

- **28.5%** of customers in the analyzed dataset were classified as churned (114 of 400). This is a dataset-level churn share, not a monthly churn rate.
- The **Free plan** had the highest churn share at **41.0%**; the association between plan and churn was statistically significant, although the effect size was small.
- Simply having a support ticket was not a statistically significant churn signal, but **support resolution time was substantially longer for churned customers**: approximately **18.69 hours vs. 6.66 hours** for non-churned customers.
- Customers with fewer recorded activities were much more frequently classified as churned. The churn share was **53.9% for customers with no recorded activity** and **4.2% for customers with 3+ activities**.
- Among customers with recorded activity, **workout tracking and workout sharing were associated with lower churn levels** after multiple-testing correction.

These results describe associations, not causal effects.

## Business Recommendations

- reduce customer-support resolution times and monitor them alongside churn,
- test initiatives that encourage regular app usage, especially workout tracking and sharing,
- monitor churn separately by subscription plan, with additional attention to Free-plan customers,
- improve event tracking and collect reliable cancellation dates and monthly active-customer counts.

## Repository Contents

```text
fitly-customer-churn-analysis/
|
|-- README.md
|
|-- analysis/
|   `-- fitly_churn_analysis_notebook.ipynb
|
|-- dashboard/
|   `-- Churn-Rate-Monitoring.pbix
|
|-- data/
|   |-- customer_month_churn_updated.csv
|   |-- da_fitly_account_info.csv
|   |-- da_fitly_customer_support.csv
|   `-- da_fitly_user_activity.csv
|
|-- presentation/
|   `-- fitly_churn_analysis_presentation.pptx
|
`-- report/
    `-- fitly_churn_analysis_report.pdf
```

### File descriptions

- **`analysis/fitly_churn_analysis_notebook.ipynb`** — working analysis notebook containing data validation, cleaning, integration, exploratory analysis, statistical testing, KPI definition, and recommendation development.
- **`data/da_fitly_account_info.csv`** — customer account and subscription data used as the base customer table.
- **`data/da_fitly_customer_support.csv`** — customer support ticket data, including contact channel, topic, and resolution time.
- **`data/da_fitly_user_activity.csv`** — recorded user activity events used to analyze engagement patterns.
- **`data/customer_month_churn_updated.csv`** — derived customer-month dataset with synthetic registration and cancellation dates used for the Power BI monthly churn monitoring prototype.
- **`dashboard/Churn-Rate-Monitoring.pbix`** — interactive Power BI dashboard for monthly churn and retention monitoring.
- **`report/fitly_churn_analysis_report.pdf`** — final written report covering data validation, methodology, findings, limitations, KPI definition, and business recommendations.
- **`presentation/fitly_churn_analysis_presentation.pptx`** — executive presentation summarizing the business problem, analytical approach, key findings, and recommended actions.

## Limitations

- The original analysis is cross-sectional rather than longitudinal.
- Cancellation dates were not available in the source dataset.
- User activity logs may contain gaps caused by upstream-system downtime.
- Some cleaning decisions required explicit assumptions, including the treatment of missing churn-status values.
- Statistical associations should not be interpreted as proof of causation.
- Monthly churn data used in the Power BI dashboard are simulated for demonstration purposes.

## Summary

This project demonstrates an end-to-end analytics workflow: data validation and integration, exploratory and statistical analysis, KPI design, business recommendations, stakeholder communication, and Power BI reporting. The analysis identified support resolution time, low recorded engagement, and subscription plan as useful areas for further retention testing and monitoring.
