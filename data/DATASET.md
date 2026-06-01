# RavenStack: Synthetic SaaS Dataset (Multi-Table)



**Author:** River @ Rivalytics  

**Credit Requirement:** You may use or remix this dataset for educational or portfolio purposes, but please credit the original author.  

**Blog:** [Building a Dataset Generator App Journey](https://rivalytics.medium.com)  

**License:** MIT-like (fully synthetic, no PII)  

**Refresh Interval:** Monthly  

**Complexity:** Capstone-level (multi-table, event-driven, time-sensitive)  

**Data Format:** CSV  

**Row Volume:**

- accounts - 500

- subscriptions - 5,000

- feature_usage - 25,000

- support_tickets - 2,000

- churn_events - 600



---



## Scenario



You're investigating RavenStack, a stealth-mode SaaS startup delivering AI-driven team tools. The product was secretly piloted with coding bootcamp graduates, and every sign-up, feature use, support ticket, and churn was captured. Now, you're tasked with discovering what drove conversions, support load, and churn patterns before their public launch.



---



## How This Dataset Was Generated



- Scripted in Python using pandas, numpy, and uuid

- Temporal logic: Validated date ranges (e.g., signup ≤ subscription ≤ churn)

- Statistical realism: Exponential and Poisson distributions for seats, usage, and durations

- Primary & foreign keys: All tables link properly; no orphans

- Edge cases: Mid-cycle plan changes, null fields, reactivations, duplicate referrals, beta feature spikes

- Nulls included: Satisfaction scores, feature usage, churn feedback

- Fully synthetic: All names, domains, feedback, and data are generated



---



## Table Relationships



```
accounts (PK: account_id)
│
├── subscriptions (FK → accounts.account_id)
│   └── feature_usage (FK → subscriptions.subscription_id)
│
├── support_tickets (FK → accounts.account_id)
└── churn_events (FK → accounts.account_id)
```



All account_id and subscription_id links are referentially complete.



---



## Table Schemas



### accounts.csv

| Column         | Type       | Description                                |
|----------------|------------|--------------------------------------------|
| account_id     | ID         | Unique customer (primary key)              |
| account_name   | string     | Fictional company name                     |
| industry       | categorical| SaaS vertical (e.g., DevTools, EdTech)     |
| country        | string     | ISO-2 country code                         |
| signup_date    | date       | Account creation date                      |
| referral_source| categorical| organic, ads, event, partner, other        |
| plan_tier      | categorical| Initial plan (Basic, Pro, Enterprise)      |
| seats          | integer    | Licensed user count                        |
| is_trial       | boolean    | Currently trialing                         |
| churn_flag     | boolean    | Churned at any point                       |



### subscriptions.csv

| Column           | Type       | Description                            |
|------------------|------------|----------------------------------------|
| subscription_id  | ID         | Unique subscription (primary key)      |
| account_id       | ID (FK)    | Links to accounts.account_id           |
| start_date       | date       | Subscription start                     |
| end_date         | date       | Nullable for active plans              |
| plan_tier        | categorical| Plan at time of billing                |
| seats            | integer    | Licensed seats                         |
| mrr_amount       | currency   | Monthly revenue                        |
| arr_amount       | currency   | Annual revenue                         |
| is_trial         | boolean    | Trial status                           |
| upgrade_flag     | boolean    | Plan upgraded mid-cycle                |
| downgrade_flag   | boolean    | Plan downgraded mid-cycle              |
| churn_flag       | boolean    | True if ended                          |
| billing_frequency| categorical| monthly or annual                      |
| auto_renew_flag  | boolean    | 80% true                               |



### feature_usage.csv

| Column           | Type       | Description                            |
|------------------|------------|----------------------------------------|
| usage_id         | ID         | Unique usage event                     |
| subscription_id  | ID (FK)    | Links to subscriptions.subscription_id |
| usage_date       | date       | Date of usage                          |
| feature_name     | categorical| From pool of 40 SaaS features          |
| usage_count      | integer    | Event frequency                        |
| usage_duration_secs | integer | Time spent                             |
| error_count      | integer    | Logged errors                          |
| is_beta_feature  | boolean    | 10% flagged as beta                    |



### support_tickets.csv

| Column                  | Type       | Description                          |
|-------------------------|------------|--------------------------------------|
| ticket_id               | ID         | Unique ticket                        |
| account_id              | ID (FK)    | Links to accounts.account_id         |
| submitted_at            | datetime   | Time opened                          |
| closed_at               | datetime   | Time resolved                        |
| resolution_time_hours   | float      | Duration                             |
| priority                | categorical| low, medium, high, urgent            |
| first_response_time_minutes | integer| Minutes to first response            |
| satisfaction_score      | integer    | 1-5 (null = no response)             |
| escalation_flag         | boolean    | True if escalated                    |



### churn_events.csv

| Column              | Type       | Description                           |
|---------------------|------------|---------------------------------------|
| churn_event_id      | ID         | Unique churn instance                 |
| account_id          | ID (FK)    | Links to accounts.account_id          |
| churn_date          | date       | When account left                     |
| reason_code         | categorical| pricing, support, features, etc.      |
| refund_amount_usd   | currency   | $0 default, 25% have credit/refund    |
| preceding_upgrade_flag| boolean | Had upgrade within 90 days             |
| preceding_downgrade_flag| boolean| Had downgrade within 90 days          |
| is_reactivation     | boolean    | 10% were previously churned           |
| feedback_text       | string     | Optional customer comment             |



---



## Suggested Projects



- Churn prediction using subscriptions + support data

- Feature adoption tracking during beta phases

- Support workload forecasting

- Revenue cohort analysis by referral channel

- Plan tier upgrade funnel by industry

- Latency analysis by seat count and plan tier



---



## Licensing



This dataset is fully synthetic and distributed under a permissive MIT-like license.  

You may use or remix it for learning, research, or portfolio purposes, but **you must credit the dataset author: River @ Rivalytics.**





