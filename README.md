# Customer Churn & CLTV Analysis Project 

In this project I wanted to explore the way customer churn rates impact a telecommunications business. Since customer churn is a key metric in subscription based business it is a significant insight to understand since it directly impacts revenue. For this project I analysed customer churn for a telecommunications business using TELCO customer churn Dataset from Kaggle. The objective of this project was to understand which segments are at high risk of churning, calculate revenue at risk and estimate the lifetime value of customers. 

I chose this dataset because churn directly correlated with revenue loss in subscription based business. This dataset was particularly well suited to churn analysis as it contains a wide range of customer-level and business-level features, including service subscriptions, contract types, tenure, and monthly charges. These variables allow for a holistic analysis of customer behaviour and the factors that contribute to churn.

Working with this dataset allowed me to analyse churn trends, identify high-risk customer segments, and estimate revenue at risk. Through this project, I was able to apply my data analysis skills end-to-end from data cleaning and feature engineering to insight generation and dashboard visualisation while delivering clear, business-focused insights to support data-driven decision-making.

To achieve this I used data cleaning technique, feature engineering, metric calculations and interactive visualisations using python and power BI. The final dashboard provides a comprehensive overview of the companies health enabling the business to target retention strategies effectively. 

# Repo Navigation
* /DataSet - raw dataset and cleaned dataset
* /EDA & Analysis - Jupyter notebook for cleaning and analysis
* /PowerBI & Insights = .pbix dashboard file

# Project Objectives
The main objectives for this project was:
1. Measure customer churn and retention: Identify the percentage of customers that were leaving the company and identifying trends based on contract type, tenure and service usage.
2. Analysing financial impact: calculate average value per customer, retention at risk and identify high value churners.
3. Estimate customer lifetime value : calculate the overall revenue based on a customers lifetime with the company and highlight key segments based on high revenue potential.
4. Segment customers : segment customers by tenure group, contract type and service usage to identify patterns in churn behaviour.
5. Visualise insights in power BI: design a multi-page dashboard in PowerBI to provide a clear, actional overview of churn risk, revenue impact and CLTV. 


# Dataset
The dataset  I used for this project includes around 7,000 customer records with the main attributes being:
- CustomerID : a unique identifier for each customer
- Churn Value : a Boolean flag indicating if the customer churned
- Monthly Charges: the monthly revenue from each customer
- Tenure Months: the number of monthly a customer has been active with the company
- Contract:: the type of contract the customer has either month- to-month, year one or year two
- Services: this includes the different types of services the customer receives
- Payment Method: the customers preferred payment method

# Data Cleaning 
Before analysing the dataset it needed to be cleaned and transformed. During the cleaning process I checked for any missing or duplicate values and handled them appropriately, converted the yes/no columns into Boolean values and standardised column names to avoid error in power Bi.

## Feature engineering
For the feature engineering I created  few new groups such as tenure grouping, contract grouping and high-valued customer identification, these groups helped me identify trends in customer churn rates. 

### Tenure Grouping
I grouped customers into tenure groups based on their active months. 
```bash
Bins = [0, 12, 24, 48, 100]
labels = [ '0-12 Months', '13-24 Months', '25-48 Months', '49+ Months']
customer['Tenure_Group'] = pd.cut(customer['Tenure_Group'], bins=Bins, labels=lables, right=False)
```
### Contract grouping 
The original dataset had Boolean columns for each contract type so I created a single contract type columns to categorise customers month-to-month, year one or year two, this allowed for easier aggregational and visualisation. 

### High-Value customers 
I created a column that identified the top 25% of monthly charges, this features enabled analysis of high value churned and the corresponding revenue risk. 

### Service Count
I created a new feature that counted the number of services each customer was using to explore correlation between service usage and churn rate. The customers that had higher number of services with the company were less likely to churn compared to customers with lower services. 

### Encoding categorical variables
For some of the columns they were categorical data types so I used one-hot encoding to make it easier to use during analysis without losing important categories such as Contract or Payment methods. 

# Metrics
In this project I focused on several key metrics that are crucial for understating churn and its financial impact. 
- Churn rate: the percentage of customs who have left the company
- Retention rate: the percentage of customers that have stayed with the company
- Average revenue per user(ARPU): the average monthly revenue per customer, highlighting the finically contribution of the customer base.
- Revenue at risk: the potential; revenue that is lost if all churned customers had instead stayed with the company
- Customer lifetime value : the total expected revenue per customer over their tenure calculated by their monthly charges and tenure
- High value churners: churned customers with the highest revenue contribution

# PowerBi dashboard 
I designed the dashboard as a three page interactive report to give the best insights into the customer dataset. 

## Page 1 - Overview 
This page provides a basic overview highlights the customer health within the company
- KPI cards for Total customers, Churn Rate, Retention Rate, ARPU Rate and Revenue at risk
- Combo charts for revenue at risk by tenure group, revenue at risk by contract type and revenue at risk by payment method.
- Table that includes the churn rate, ARPU, retention rate, revenue at risk, CLTV average and monthly charges by total customers and contract type

## Page 2 - Customer churn and revenue drivers 
This page focuses on identifying the drivers or churn and how that impacts the customers. 
- Churn rate by contract type
- ARPU and Churn rate by service count
- Churn rate by internet service
- Churn rate by service count
- Churn rate by payment method

## Page 3 - Revenue and CLTV analysis 
This page provides analysis for the financial impact and lifetime value of the customers.
- CLTV by tenure group and contract type
- Revenue at risk by contract
- Revenue at risk by tenure group
- KPI cards for revenue at risk by high value churners, revenue at risk by contract type and revenue at risk by tenure group
- Table showing payment method, tenure group, churn rate, revenue at risk and total customers

# DAX Measures used
## Churn rate 
```bash
Churn Rate =
VAR TotalCustomers =
COUNT(customer_cleaned_data[CustomerID]) VAR ChurnedCustomers =
SUM(customer_cleaned_data[Churn Value]) RETURN IF(TotalCustomers = 0, BLANK(), DIVIDE(ChurnedCustomers, TotalCustomers))
```
## Revenue at risk
```bash
Revenue risk = CALCULATE(SUM(customer_cleaned_data[Monthly Charges]), customer_cleaned_data[Churn Value] = 1)
```

## CLTV 
```bash
CLTV avg = AVERAGEX(
    'customer_cleaned_data',
    'customer_cleaned_data'[Monthly Charges] * 'customer_cleaned_data'[Tenure Months] / 12
)
```

## High Value Churners
```bash
High Value Churner = 
IF(
    customer_cleaned_data[Churn Value] = 1
    && customer_cleaned_data[Monthly Charges] >= 
       PERCENTILEX.INC(
           customer_cleaned_data,
           customer_cleaned_data[Monthly Charges],
           0.75
       ),
    TRUE(),
    FALSE()
)
```

# Key Insights 
Based on my analysis we can see that the Month-to-Month contract have the highest carton rate which highlights the importance of long-term retention strategies. The high value churners also falls into the month-to-month contract which suggests they are critical targets for retention campaigns. Customers with certain payment methods and less services also correlate with high churners which give us insights into target marketing service improvements. Encouraging customers to use more than two services can decrease the churn rate by at least 6.41%. From this analysis we see that customers with longer tenure and multiple year contracts are more likely to stay which indicates that contract type and tenure strongly influence the churn rate. 


# Tools and libraries
For this project I mostly used python for the cleaning and analysis, this unclouded libraries such as numpy and seaborn. 
I then used PowerBI to create the dashboard and used a variety of different visualisations and Dax measures to provide an insightful overview. 

## How to Run
```bash
git clone maison-r/Telco-Customer-Churn-Analysis
```
