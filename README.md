# Globox Analysis (A/B Testing)

## Table of content
 - [Project Overview](#Project-Overview)
 - [Data Source](#Data-Source)
 - [Tools](#Tools)
 - [Data Collection](Data-Collection)
 - [Data Wrangling](#Data-Wrangling)
 - [Data Analysis](#Data-Analysis)
  

## Project Overview
The main aim of this experiment was to determine whether launching the food and drinks banner on the
Globox website would lead to a significant increase in the company’s revenue.
This project utilized all the steps involved in a typical data analytics project from the development of
the problem to data storytelling and visualization.

## Data Source
The main data used in this project was contained in the Globox database which can be accessed through this link:  postgres://Test:bQNxVzJL4g6u@ep-noisy-flower-846766-pooler.us-east-2.aws.neon.tech/Globox

### Tools
- Excel - Data cleaning and Statistical analysis (A/B testing)
- SQL - Data analysis
- Tableau - Creating reports

## Data Collection
The final data used in the project was created by joining multiple tables within the database and selecting the relevant parameters based on the business questions. This was done using PostgreSQL. The final CSV file was then exported to Excel for cleaning and statisitical analysis. The code used to extract the CSV file was:
```sql
SELECT g.uid,
u.country,
u.gender,
g.device,
g.group,
SUM(COALESCE(spent, 0)) AS spent,
CASE
WHEN SUM(COALESCE(spent, 0)) > 0 THEN 'Yes'
ELSE 'No'
END AS Converted
FROM users u
LEFT JOIN activity a ON u.id = a.uid
LEFT JOIN groups g ON u.id = g.uid
GROUP BY g.uid,
u.country,
u.gender,
g.device,
g.group;
```

## Data Wrangling
Minimal data cleaning practices were done on the collected dataset because the data was fairly suitable
for analysis. The major pre-processing action done on the dataset was replacing the null values on the
'spent' column with zero values. This was important for further analysis which required the use of the
spent parameter.

## Data Analysis
### Exploratory Data Analysis
The following questions were answered:
1. what was the total number of users in the experiment?
```sql
SELECT count(DISTINCT a.uid) AS distinct_users,
count(a.uid) AS purchasing_users,
count(DISTINCT u.id) AS total_users
FROM users u
LEFT JOIN activity a ON u.id = a.uid;
```
2. How many users were in the control and treatment group?
```sql
SELECT "group",
count(*)
FROM groups
GROUP BY "group"
```
3. What was the total conversion rate by users?
```sql
SELECT count(DISTINCT CASE
WHEN COALESCE (spent, 0)>0
THEN UID
END)*100.0/ count(UID) AS
conversion_rate
FROM groups g
LEFT JOIN activity a USING (UID)
```
4. What was the conversion rate of users in each group?
```sql
SELECT g.group,
count(DISTINCT a.uid)*100.0/count(g.uid)
AS conversion_rate
FROM groups g
LEFT JOIN activity a USING (UID)
GROUP BY g.group)
```   
### Statistical  Analysis
Two main parameters were tested during the statisitcal analysis. they included:
- Conversion rate
- Average user purchase

#### Analysis of Conversion rate
- The 2-sample proportion Z-test was employed here to determine if the difference in the conversion rate between users in the control and treatment group was statistically significant.
- The goal of the test was to deternmine if the calculated p-value would support or reject the null hypothesis which stated that there was no difference in the conversion rate between the two groups.
- Several parameters were adopted in this analysis, including:
  - Standard error: SE = √ ( p̂c * (1 - p̂c) * (1/nA + 1/nB) )
  - Critical Value: NORM.S.INV
  - P-value: NORM.S.DIST
  - Margin of Error: [Zc*SE]
  - Confidence Interval = (p̂A - p̂B) ± Zc * √ ( p̂A*(1 - p̂A)/nA + p̂B*(1 - p̂B)/nB )
  - Lower Bound = (p̂A - p̂B) - Margin of Error
  - Upper Bound = (p̂A - p̂B) + Margin of Error
  
 #### Analysis of Average user purchase
- The 2-tailed T-test was adopted here to determine if there was statistically significant difference between the mean purchases by users in each group
- The two sample t-test with unequal variance is also called the Welchs test.
- Parameters used in this test included the group mean purchases, standard deviation, the degree of freedom.
- df = ( (sA^2 / nA + sB^2 / nB)^2 ) / ( ( (sA^2 / nA)^2 ) / (nA - 1) + ( (sB^2 / nB)^2 ) / (nB - 1) )
- t = (x̄A - x̄B) / sqrt( sA^2 / nA + sB^2 / nB )
- Critical t-value = T.INV(1 - α/2, df)
- p-value = T.DIST.2T(ABS(t), df

## Result
The outcome of the A/B Test showed that there was a statistically significant difference between the conversion rate of the control and treatment group. Yet, despite the recognisable difference in the mean purchase amongst users within each group, the statistical test showed that this difference held no statistical significance, and had a high probability of occuring by chance. Further analysis showed the presence of novelty and a low statistical power with regards to the sample size.

## Recommendation
Considering the statistical analysis, the power analysis, and the test for novelty, I recommended that the experiment needed more iteration.
This is based on the premise that despite the interesting result obtained when the difference in conversion rate was considered, we failed to identify a significant difference in the average amount spent
by users between the two groups.
Furthermore, The presence of the novelty effect only raises questions regarding the validity of the results.
Finally, the low statistical power buttresses the resolve of my recommendation, as it is clear that for the
intended 80% statistical power, the sample size was rather insufficient to meeting the standard.
Therefore, the recommended iteration should account for a larger sample size as required.
