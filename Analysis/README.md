# Data Analysis Dashboard Using Power BI
## Table of Contents
1. Project Objective
2. Dataset Description
3. Preparing Data for Analysis
4. Visualizations
5. Project Insights
6. Project Objective
To develop a comprehensive credit card weekly dashboard that provides real-time insights into key performance metrics and trends, enabling stakeholders to monitor and analyze credit card operations effectively.

## Dataset Description
credit_card: No of rows=10,109, No of columns=18. This dataset contains basic information about credit card like Client_Num, Card_Category, Annual_Fees, Activation_30_Days, Customer_Acq_Cost, Week_Start_Date, Week_Num, Qtr, current_year, Credit_Limit, Total_Revolving_Bal, Total_Trans_Amt, Total_Trans_Vol, Avg_Utilization_Ratio, Use Chip, Exp Type, Interest_Earned, Delinquent_Acc. This dataset contains weekly data from January 1, 2023 to 24 December, 2023 (52-weeks data).
## customer: No of rows=10,109, No of columns=15. This dataset contains credit card customers data with columns like Client_Num, Customer_Age, Gender, Dependent_Count, Education_Level, Marital_Status, state_cd, Zipcode, Car_Owner, House_Owner, Personal_loan, contact, Customer_Job, Income, Cust_Satisfaction_Score.
cc_add: No of rows=186, No of columns=18. This dataset contains same columns as that of 'credit_card' dataset. This dataset contains data after Week 52 i.e., after 24 December, 2023.
## cust_add: No of rows=186, No of columns=15. This dataset contains same columns as that of 'customer' dataset.
Preparing Data for Analysis
Copying Excel data to PostgreSQL database using SQL queries.
Note: ALL SQL queries are provided in SQL-Query.sql file provided above.
Connecting PostgreSQL database to Power BI Desktop and importing the dataset.
Creating a new column 'Revenue' in 'credit_card' dataset.
Revenue = 'public cc_detail'[annual_fees] + 'public cc_detail'[total_trans_amt] + 'public cc_detail'[interest_earned]
Created a new column 'week_num2' in 'credit_card' dataset.
week_num2 = WEEKNUM('public cc_detail'[week_start_date])
Created a new measure 'Current_week_revenue' in 'credit_card' dataset.
Current_week_revenue = CALCULATE(SUM('public cc_detail'[Revenue]), FILTER(ALL('public cc_detail'), 'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])))
Created a new measure 'Previous_week_revenue' in 'credit_card' dataset.
Previous_week_revenue = CALCULATE(SUM('public cc_detail'[Revenue]), FILTER(ALL('public cc_detail'), 'public cc_detail'[week_num2] = MAX('public cc_detail'[week_num2])-1))
Created a new measure 'WOW_revenue' in 'credit_card' dataset.
WOW_revenue = DIVIDE(([Current_week_revenue] - [Previous_week_revenue]), [Previous_week_revenue])
Created a new column 'AgeGroup' in 'customer' dataset.
AgeGroup = SWITCH(TRUE(), 'public cust_detail'[customer_age] < 30, "20-30", 'public cust_detail'[customer_age] >= 30 && 'public cust_detail'[customer_age] < 40, "30-40", 'public cust_detail'[customer_age] >= 40 && 'public cust_detail'[customer_age] < 50, "40-50", 'public cust_detail'[customer_age] >= 50 && 'public cust_detail'[customer_age] < 60, "50-60", 'public cust_detail'[customer_age] >= 60, "60+", "unknown")
Created a new column 'IncomeGroup' in 'customer' dataset.
IncomeGroup = SWITCH(TRUE(), 'public cust_detail'[income] < 35000, "Low", 'public cust_detail'[income] >= 35000 && 'public cust_detail'[income] < 70000, "Medium", 'public cust_detail'[income] >= 70000, "High", "unknown")
Visualizations
Two Power BI reports are made - Credit Card Transaction Report, Credit Card Customer Report.

Credit Card Transaction Report
Created a Table visualizing Sum of Revenue, Sum of total_trans_amt and Sum of interest_earned according to credit card category. image
Created a Line and Stacked Column Chart visualizing Quarterly Revenue and Total Transaction Count. image
Created a Stacked bar chart visualizing Revenue by Expenditure Type. image
Created a Stacked bar chart visualizing Revenue by Customer Job. 

Main
![image](https://github.com/user-attachments/assets/c46feb73-0b06-4516-8831-54022b2cd90f)

Cards
![image](https://github.com/user-attachments/assets/538e2faf-6393-4f89-a686-f32bddf8f1b5)


Target
![image](https://github.com/user-attachments/assets/1f5da38a-5298-4c47-8be9-5e9382b0d9a9)


Category wise
![image](https://github.com/user-attachments/assets/49101996-c9a2-4645-9c72-f7aefe928f15)


Closer Rate
![image](https://github.com/user-attachments/assets/250354c4-0537-4be9-8214-283b6df99958)


table
![image](https://github.com/user-attachments/assets/29354dae-a967-4bd7-ac8b-c186bf795852)


Project Insights
WOW (Week On Week) change:
Revenue increased by 28.8%
Total transaction amount and count increased by 35.03% and 3.3%
Customer count increased by 12.8%
Overview YTD (Year To Date):
Overall revenue is 57M
Total interest is 8M
Total transaction amount is 46M
Male customers are contributing more in revenue 31M, females 26M
Blue and silver credit cards are contributing to 93% of overall transactions
TX, NY and CA are contributing to 68%
Overall activation rate is 57.5%
Overall delinquent rate is 6.06%
