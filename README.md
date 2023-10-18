# Globox Analysis (A/B Testing)

## Table of content
 - [Project Overview](#Project-Overview)
 - [Data Source](#Data-Source)
 - [Tools](#Tools)

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
The final data used in the project was created by joining multiple tables within the database and selecting the relevant parameters based on the business questions. This was done using PostgreSQL. The final CSV file was then exported to Excel for cleaning and statisitical analysis

## Data Wrangling
Minimal data cleaning practices were done on the collected dataset because the data was fairly suitable
for analysis. The major pre-processing action done on the dataset was replacing the null values on the
'spent' column with zero values. This was important for further analysis which required the use of the
spent parameter.

