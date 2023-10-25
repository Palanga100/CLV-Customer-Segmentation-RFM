# CLV, Customer Segmentation & RFM

https://docs.google.com/spreadsheets/d/1jvAE9tFCXc2JRQWr2YZt1KwnVQRKH87ITEVmUgb_6U0/edit?usp=sharing
https://public.tableau.com/app/profile/g.kasparaviciene/viz/RFMProject_16872617270640/Dashboard1

You got a follow up task from your product manager to give stats on how subscriptions churn looks like from a weekly retention standpoint. You should provide weekly subscriptions data that would show how many subscribers who started on a particular week remain active in the upcoming 6 weeks. Your end result should show weekly retention cohorts and their retention from week 1 to week 6. Assume that you are doing this analysis on 2021-02-07.

WITH										
cohorts AS (										
SELECT										
user_pseudo_id,										
DATE_TRUNC(DATE (TIMESTAMP_MICROS(MIN(event_timestamp))),week) AS first_event_week,										
FROM										
`tc-da-1.turing_data_analytics.raw_events`										
GROUP BY										
user_pseudo_id),										
registrations AS (										
SELECT										
user_pseudo_id,										
purchase_revenue_in_usd AS revenue,										
DATE_TRUNC(DATE (TIMESTAMP_MICROS(event_timestamp)),week) AS purchase										
FROM										
tc-da-1.turing_data_analytics.raw_events										
WHERE										
event_name='purchase'),										
cohort_summary AS (										
SELECT										
cohorts.user_pseudo_id,										
first_event_week,										
purchase,										
registrations.revenue										
FROM										
cohorts										
LEFT JOIN										
registrations										
ON										
cohorts.user_pseudo_id=registrations.user_pseudo_id)										
SELECT										
first_event_week AS week,										
COUNT(DISTINCT user_pseudo_id) AS customers,										
SUM(CASE WHEN purchase = first_event_week THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_0,										
SUM(CASE WHEN purchase = DATE_ADD(first_event_week,INTERVAL 1 week) THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_1,										
SUM(CASE WHEN purchase = DATE_ADD(first_event_week,INTERVAL 2 week) THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_2,										
SUM(CASE WHEN purchase = DATE_ADD(first_event_week,INTERVAL 3 week) THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_3,										
SUM(CASE WHEN purchase = DATE_ADD(first_event_week,INTERVAL 4 week) THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_4,										
SUM(CASE WHEN purchase = DATE_ADD(first_event_week,INTERVAL 5 week) THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_5,										
SUM(CASE WHEN purchase = DATE_ADD(first_event_week,INTERVAL 6 week) THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_6,										
SUM(CASE WHEN purchase = DATE_ADD(first_event_week,INTERVAL 7 week) THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_7,										
SUM(CASE WHEN purchase = DATE_ADD(first_event_week,INTERVAL 8 week) THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_8,										
SUM(CASE WHEN purchase = DATE_ADD(first_event_week,INTERVAL 9 week) THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_9,										
SUM(CASE WHEN purchase = DATE_ADD(first_event_week,INTERVAL 10 week) THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_10,										
SUM(CASE WHEN purchase = DATE_ADD(first_event_week,INTERVAL 11 week) THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_11,										
SUM(CASE WHEN purchase = DATE_ADD(first_event_week,INTERVAL 12 week) THEN revenue END)/COUNT(DISTINCT user_pseudo_id) AS week_12										
FROM										
cohort_summary										
GROUP BY										
first_event_week										
ORDER BY										
week										        
