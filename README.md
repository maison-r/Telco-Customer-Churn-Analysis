# Customer Churn & CLTV Analysis PRoject 

In this project i wanted to explore the way customer churn rates impact a telecommunicatons business. Since customer churn is a key metirc in subscription based business it is a significant insight to understand since it directly impacts revenue. For this project i analysed customer churn for a telecomunciatiosn business ysing TELCO customer churn Dataset from Kaggle. the objecyive of this project was to understand which segemnts are at high risk of churning, calcualate reveinue at risk and estimate the lifetime value of customers. 

To acheive this i used data cleaning technique, feature engeneering, metric calcualtions and interactive viusalisations using python and power BI. the final dashboard prvides a comprehensive overview of the companies health enabling teh business to target retenetion strategies effectivly. 

# Project Objectives
the main objectives for thsi project were:
1. Measure customer churna dn retention: Identify the percentage of customers that were leaving the company and identifying trends based on contract type, tenure and service usage.
2. analysing financial impact: calcualte average value per customer, rentntion at risk and identofy high value churners.
3. estimate customer lifetime value : calculate the overall revenue based on a customers lifetime with teh comapny and highlight key segments based on high revenue potential.
4. segment customers : segment customers by tenure group, contract type and service usage to identify patterns in churn behaviour.
5. visualuase insights in power BI: design a multi-page dashbaord in PowerBI to provide a clear, actional overview of churn risk, revenue impact and CLTV. 


# Dataset
the dataset  i used for thios prject includesd around 7,000 customer records with the main attributes being:
- CustyomerOd : a unique identifier for each customer
- Churn Value : a boolean flag indiating if the customer churned
- Monthly Charges: the monthly ervenue from each customer
- Tenure Months: the number of monthly a customer has been active witht eh company
- Contract:: the type of contract the customer has either month- to-month, year one or year two
- Services: this incldes the different types of services the customer receives
- Payment Method: the customers prefered payment method

# Data Cleaning 
before analysing teh dataset it needed to be cleaned and transformed. during the cleaning process io checked for any missing or null values and hadled thema pprpiatly, converted the yes/no columns into boolean values and satandardised column names to avoid error in power Bi.

## Feature engineering
foer thr feature engineering i createda  few new groups such as tenure groupsing, contract grouping and high-alued customer identification. these groups helped me identify trends in customer churn rates. 

### Tenure Grouping
I grouped customeers into tenure groups based on their active months. 
```bash
Bins = [0, 12, 24, 48, 100]
labels = [ '0-12 Months', '13-24 Months', '25-48 Months', '49+ Months']
customer['Tenure_Group'] = pd.cut(customer['Tenure_Group'], bins=Bins, labels=lables, right=False)
```
### Contract grouping 
the opriginal dataset had boolean columsn fro each contract type so i created a singel contract ttype columns to categorise customers month-to-month, year one or year two. this allowed for easier aggregationa nd visualisation. 

### High-Value customers 
i created a column that identoifed the top 25% of monthly chrges. this featuresd enabled analysis if high value churnerd andf the corresponding revenue risk. 

### Service Count
i created a new feature that counted the number of services each customer was using to explore correlation betwwen service usage and churn rate. The customers that hadx nhigher nuimber of services with teh company were less likely tyop churn compared to customers with lower services. 

### Encoding categorical variables
for sopme of the columsn they were categorical data types so i used one-hot encoding to make iot easier to use during analysis without looking important categories such as Contract or Payment methods. 

# Metrics
In this project i foucsed on several key metrics that are cruicial for sunderstaing churn and its financial impact. 
- Churn rate: the percentage of customs who have left the company
- renetion rate: the percentage of customers that have stayed with te company
- average rever per user(ARPU): the average monthly revene per customer, highlighting the fincianll contribution of the customer base.
- Revenue at risk: the potentiosal; trevenue that is lost if all churned customers had insetad stayed with the company
- customer lifetime value : the totoal expected revenue per customer ove their tenure calcuiaterd by their monhtly charges and tenure
- high value churners: churned customers witht he highest revenue contribuution

#E PowerBi dahsbaord 
i designed the dahbaord as a thre page inertactuive report to give the best insights into the customer dataste. 

##Page 1 - the overview 
this page provides a basic overview highlights the customer health within the company
- KPI cards for Total customers, Churn Rate, Retention RAte, ARPU RAte and REvenue at risk
- combo charts for revenue at risk by tenure group, crevenue at risk by contract type and revenue at risk by payment method.
- Table that includes the churn rate, ARPU, rention rate, revenue at risk, CLTV average and monthlyc harges by total customers and contract type

## Page 2Customer churna nd revneu drivers 
this page focuses on identofying the drivers or churn and how that impacts the customers 
- churn rate by contract type
- ARPU and Churn rate by service count
- churn rate byu internet servioce
- churn rate by service count
- churn rate by payment method

##Page 3 Revenue and CLTV analysis 
this page provides analysis for the fincancial impactand lifetimevalue of the customers
- CLTV by tenure group and contract type
- revenue at risk by contract
- revenue at risk by tenure group
- KPI cards for reveneu at risk vy hgigh value chirners, reveneu at risk by contract type and revenue at risk by tenure group
- table shwing payemnt method, tenure grpup, churn rate, revenue at risk and total customers

#DAX Measures used
Churn rate 
```bash
Churn Rate =
VAR TotalCustomers =
COUNT(customer_cleaned_data[CustomerID]) VAR ChurnedCustomers =
SUM(customer_cleaned_data[Churn Value]) RETURN IF(TotalCustomers = 0, BLANK(), DIVIDE(ChurnedCustomers, TotalCustomers))
```
reveneu at risk
```bash
Revenue risk = CALCULATE(SUM(customer_cleaned_data[Monthly Charges]), customer_cleaned_data[Churn Value] = 1)
```

CLTV 
```bash
CLTV avg = AVERAGEX(
    'customer_cleaned_data',
    'customer_cleaned_data'[Monthly Charges] * 'customer_cleaned_data'[Tenure Months] / 12
)
```

high value churners
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

#Key Inisghts 
based on my analysis we can see that the Month-to-Month contract ahve the highest chrtun rate which highl.ights the imporantance of loongterm reention stratagies. the high value churners also fall into the month-to-motn contract which syuggestsd theya re critical taregts for renetion campaigns. customers with certain payment methods and less services also ccorrelate with high churners wehioch guive sus ungsights into target ,arkeringa dn service improvements. encouraging customers to use more than two seerviceas can decrease the churn rate by at least 6.41%. from this analysis we see that custonmers with longer tenuire and multip year contracts are more likely to stay which inducates that contract type and tenure storngly influycen the churn rate. 


#Tools anbd libraries
for this project i mostly used python for the cleaning and analysis, this uncluded libraies such as numpy seabord. 
i then used PowerBi to create the dahsboard and used a variatey opf differnt visualisations andf DaX measures to prvoide an insighful overview. 

## How to Run
```bash

git clone maison-r/Telco-Customer-Churn-Analysis
```
