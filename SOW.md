# Statement of Work

**Project Title:** RavenStack: SaaS Retention & Growth Analysis   
**Data Analyst:** Elene Kikalishvili  
**Date:** May 2026

---

# Part 1. Project Definition

## 1. Company Context

RavenStack is a stealth-mode B2B SaaS startup delivering AI-powered team collaboration and developer tools. The product was piloted with coding bootcamp graduates across five industries - DevTools, FinTech, Cybersecurity, HealthTech, and EdTech - before public launch. With 500 accounts across three subscription tiers (Basic, Pro, Enterprise) and five acquisition channels, the business is preparing for public launch and needs to understand what drives long-term customer retention and revenue expansion.

Despite steady account growth, the business faces elevated churn and uneven revenue expansion across segments. The data team has been asked to deliver a full analytical report answering six business questions before the next board review.

---

## 2. Business Questions

| # | Question | Theme |
|---|---|---|
| Q1 | Which customer segments generate the highest long-term revenue, and which expand their contracts over time? | Revenue & Expansion |
| Q2 | Which referral sources and industries produce the highest-value customers, measured by LTV, MRR, and churn rate? | Acquisition Quality |
| Q3 | What does the journey from trial → upgrade → churn look like, and where does revenue expand or collapse? | Lifecycle |
| Q4 | Which features drive product value, and do engaged users retain better? | Product |
| Q5 | Which support experience patterns are associated with increased churn risk? | Support |
| Q6 | How do different signup cohorts retain and expand over time? | Retention |

---

## 3. Metric Definitions

| Metric | Definition |
|---|---|
| Active subscription | `end_date` is null in subscriptions |
| Active MRR | Sum of `mrr_amount` for active subscriptions only |
| Churned account | Account whose most recent subscription record has a non-null `end_date` |
| Trial-to-paid conversion | Trial account with at least one non-trial subscription record |
| Churn rate | Churned accounts / total accounts |
| Customer LTV | `mrr_amount × tenure_months` where tenure = months from signup to churn or current date |
| Engagement score | Normalized composite of usage frequency, session duration, and error rate - all components min-max normalized before weighting to ensure scale independence |
| Support burden | Average ticket count per account segmented by plan tier |

---

## 4. Dataset & Table Overview

**Dataset:** RavenStack Synthetic SaaS Dataset  
**Source:** Kaggle - River @ Rivalytics  
**Format:** 5 CSV files, fully relational with FK/PK integrity

| Table | Rows | Description |
|---|---|---|
| accounts | 500 | Master customer records - the primary table linked by all others |
| subscriptions | 5,000 | Subscription lifecycle records including MRR, ARR, and plan changes |
| feature_usage | 25,000 | Product interaction logs across 40 features |
| support_tickets | 2,000 | Support activity with satisfaction scores and resolution metrics |
| churn_events | 600 | Historical churn log with reason codes and reactivation flags |

**Table relationships:**
- `subscriptions.account_id` → `accounts.account_id`
- `feature_usage.subscription_id` → `subscriptions.subscription_id`
- `support_tickets.account_id` → `accounts.account_id`
- `churn_events.account_id` → `accounts.account_id`

---

## 5. Data Quality & Assumptions

| Issue | Decision |
|---|---|
| `end_date` nulls in subscriptions (4,514 rows) | Null = active subscription - authoritative definition of active MRR |
| `satisfaction_score` nulls in support_tickets (825 rows) | Null = no survey response - excluded from averages automatically by pandas `.mean()` |
| `feedback_text` nulls in churn_events (148 rows) | Null = no written feedback - expected for optional free text field |
| Accounts with multiple subscriptions | Most recent subscription used for account-level analysis, identified by `start_date` descending |
| `churn_flag` in both accounts and subscriptions | `accounts.churn_flag` is unreliable - all 110 accounts marked as churned have active subscriptions (flag never reset after reactivation), and 33 genuinely churned accounts show `churn_flag = False`. Neither flag is used as the authoritative churn indicator. Current churn is defined via subscription-based logic |
| `is_reactivation == True` in churn_events (61 rows) | Reported separately - not merged into primary churn rate to avoid double-counting |
| Beta features `is_beta_feature == True` (2,544 rows) | Reported separately - not excluded from overall usage totals |
| Duplicate `usage_id` values (21 pairs, 42 rows) | ID generation collisions confirmed - all rows retained; `usage_id` not used as a merge key |
| Discrepancy between churn_events (352 unique accounts) and accounts.churn_flag (110 churned) | Explained by accounts that churned multiple times or reactivated. `churn_events` is the historical log; `accounts.churn_flag` reflects current status only |  
| Trial-to-paid conversion rate | 100% conversion confirmed across all channels - synthetic data artifact. Q2 revised to focus on customer value by acquisition channel instead. |

---

# Part 2. Execution Plan

## 6. Engagement Roadmap

*All analysis conducted in Python (pandas, matplotlib, seaborn) within a Jupyter Notebook environment.*

| Phase | Activities | Deliverable |
|---|---|---|
| I - Setup & Validation | Data loading, exploration of all five tables, cleaning, metric definitions, executive KPI snapshot | Validated dataset, documented assumptions, five headline KPIs |
| II - Core Analysis | Revenue & expansion segmentation (Q1), trial conversion by channel and industry (Q2), customer lifecycle mapping from trial to churn (Q3) | Visualizations and insight drafts for Q1–Q3 |
| III - Advanced Metrics | Feature adoption and engagement score analysis (Q4), support quality vs churn correlation (Q5), cohort retention and expansion heatmap (Q6) | Final visualizations including cohort heatmap for Q4–Q6 |
| IV - Final Synthesis | Cross-question statistical observations, prioritized stakeholder recommendations, notebook polish, README and GitHub publication | Complete analytical report ready for portfolio |

**Estimated timeline:** 7 working days

---

## 7. Deliverables

- **Annotated Jupyter Notebook** (`ravenstack_analysis.ipynb`) - full analysis structured as a stakeholder-ready report, with markdown commentary explaining findings in plain English throughout
- **Technical Documentation** (`README.md`) - project context, six business questions, key findings, tools used, and dataset attribution (River @ Rivalytics, required by license)
- **Public Repository** - version-controlled GitHub project (`saas-retention-growth-analysis`) linked from LinkedIn and resume

**Success criteria:** The analysis is considered complete when all six business questions are answered with supporting visualizations, a findings summary, and actionable recommendations addressed to named stakeholders.
