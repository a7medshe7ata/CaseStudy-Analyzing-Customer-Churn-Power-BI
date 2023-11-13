# Introduction

For subscription-based businesses, reducing customer churn is a top priority. In this Power BI case study, I will investigate a dataset from an example telecom company called Databel and analyze their churn rates.
Analyzing churn doesn’t just mean knowing what the churn rate is: it’s also about figuring out why customers are churning at the rate they are, and how to reduce churn. You'll answer these questions by creating measures and calculated columns, while simultaneously creating eye-catching report pages.

**Defining churn**

A good definition is the one from [Investopedia](https://www.investopedia.com/terms/c/churnrate.asp):
“<strong><em>The churn rate, also known as the rate of attrition or customer churn, is the rate at which customers stop doing business with an entity</em></strong>

# Methodology

1. Exploratory Analysis
2. Investigating Churn Patterns
3. Visualizing your Analysis

## Data Analysis Flow

![Alt text](/Images/1.png "Optional title")




# Implemntaion Steps

### Discovring Dataset

[Dataset Meta Data](https://assets.datacamp.com/production/repositories/5993/datasets/c55ad82061b13bc07f6516e51cba9883a90bfa27/Metadata%20-%20Case%20Study_Analyzing%20Customer%20Churn%20in%20Power%20BI.pdf)
The **Databel** dataset consists of 29 different columns and has one row per customer.
The dataset contains numerous dimensions.

* The **Customer\_id** is a unique ID that identifies an individual customer.
* The second column is called <strong>Churn Label</strong>, and it indicates if a customer churned with **“Yes”** and **“No”** labels. 
* The dataset contains various other dimensions, such as demographic fields and information about premium plans.

### Data check time

creating two measures to check if the count of customer ids is equal to the count of unique customer ids. 
This check is particularly important, to avoide duplicates which makes results wrong.

```
Number of Customers = count('Databel - Data'[Customer ID])
Number of Unique Customers = DISTINCTCOUNT('Databel - Data'[Customer ID])
```

`[`<strong>`Results`</strong>` :Both values are the same which equal (6687)]`

### **Calculating churn** 

**[The simplified formula for churn is to divide customers lost by the total number of customers]**

```
Churned = if('Databel - Data'[Churn Label]="Yes",1,0)Number of Churned Customers = sum('Databel - Data'[Churned])
Churn Rate = [Number of Churned Customers]/[Number of Customers]
```

`[`<strong>`Results`</strong>` :The total churn rate for “Databel” is 26.86%]`

### **Investigating Churn reasons [The top three reasons] :**

Competitor made better offer
Competitor had better devices
Attitude of support person

### Deep Dive into churn Categories:

Churn Reasons are grouped together in the Churn Category column. 
The “Extra data charges”, “Price too high” and other price related reasons are grouped together in the “Price” category
`[`<strong>`Results`</strong>` :Almost half of all the customers churning are related to the competitor category]`

### **Using Map Visualization:**

createing a map to look at the churn rate by state
`[`<strong>`Results`</strong>` :California has a Most churn rate of 63.24 %, which should be invistgated]`

**Insights discovered** 

* The average churn rate is around 27%, and that the main reason why customers churn is related to competitors. This could result in asking questions such as “Is **Databel** competitive enough?”.
* I also discovered that the churn rate in California is abnormally high at 63.24%. I don’t have a clear explanation yet for the relatively high churn rate of 27%, and there are still so many columns need to be analyzed.


### Analyzing Demographics [Age]:


looking at the classified demographic characteristics associated to age , IF() method will be used to create a column with three categories

```
Demographics = if('Databel - Data'[Senior]="Yes","Senior",If('Databel - Data'[Under 30]="Yes","Under 30","Other"))
```

`[`<strong>`Results`</strong>` :The churn rate for senior citizens is around 10% higher than average]`

### **Analyzing Demographics , Deep dive to Analyze Age Groups :**

create age bins and make a combo chart visualizing the number of customers per bracket and their respective churn rates.

`[`<strong>`Results`</strong>` :In general, the churn rate has an increasing trend through the age brackets.]`
`[`<strong>`Results`</strong>` :The churn rate for customers more than 80 years old is exactly 52%.]`
`[`<strong>`Results`</strong>` :As the age increases AVG churn rate for age brackets also increases.]`

Since Dtabel offers group contracts to customers from the same household , The advantage for the customer is a discounted rate, while it’s a great way for **Databel** to grow its customer base.

### Inspecting Contract Groups :


Determineing whether consumers who belong to a group have a lower phone bill and whether this has an effect on churn

`[`<strong>`Results`</strong>` :It appears the Monthly Charge is significantly lower for people who are in a group of 2 or more. Adding churn rate]`

contract types : calculate if customers have only yearly contracts or monthly contracts , through gather the values of yearly contracts into one

```
Contract Category = SWITCH('Databel - Data'[Contract Type], "One Year", "Yearly", "Two Year", "Yearly", "Monthly")
```

`[`<strong>`Results`</strong>` :There’s a significant difference: monthly contract customers churn more than yearly contract customers]`

### Analyzing Demographics [Gender]:


Investigate whether gender influences the churn rate, through creating a clustered column chart to see how customers differ in terms of churn rate by looking at their contract categories and gender .
`[`<strong>`Results`</strong>` : The female churn rate on a monthly basis is 47.31%]`

<br>
**Databel** has a hypothesis that people who are not on an unlimited data plan are more likely to churn. My task is to investigate how the Unlimited Data Plan influences the churn rate.

### Analyzing Unlimited plan :


`[`<strong>`Results`</strong>` : It appears that customers who are on an unlimited plan are more likely to churn]`
creating a new column Grouped Consumption that categorizes the average monthly GB download into the following groups to determine whether it’s related to the amount of mobile data (GB) used:

```
Grouped Consumption = IF('Databel - Data'[Avg Monthly GB Download]<5,"Less than 5 GB", IF('Databel - Data'[Avg Monthly GB Download]<10,"Between 5 and 10 GB","10 or more GB"))
```

`[`<strong>`Results`</strong>` : There’s a difference in churn rate by categories which evident by the chart]`

since The analysis requirement given by **Databel** includes a request to analyze the international activity of customers and its relationship to churn. 
They are curious about the behavior of customers who call internationally, and if paying for an international plan influences their loyalty.

### Analyzing International Calls :


**Databel** wants to focus on the budget on a state-by-state basis for the promotion of the international plan
Using matrix chart , showing the churn rate by Intl Plan and Intl Active

`[`<strong>`Results`</strong>` :seems to be a region experiencing an unusually high rate of customer turnover]`
`[`<strong>`Results`</strong>` :I estimate that 72% of customers engage in international calls without having an international plan]`

<br>
***

> Results I discovered that customers who pay for an international plan but do not make international calls had a very high churn rate. I recommend that **Databel** contact customers who have an international plan but have not made any international calls and suggest that they downgrade their plan.


***

Since **Databel** also aims to enhance customer service after certain complaints, i have to look at three key aspects of customers: payment methods, contract types, and the length of time a client has been a customer.

### Analyzing Contract Type :

![Alt text](/Images/2.png "Optional title")
![Alt text](/Images/3.png "Optional title")




`[`<strong>`Results`</strong>` :Churn rate does decrease over time]`
`[`<strong>`Results`</strong>` :Exploring the different payment methods, I can see that there is one type that is much smaller than the others]`
`[`<strong>`Results`</strong>` :The number of customers who pay with paper checks accounting for only 5.5% of all customers]`
`[`<strong>`Results`</strong>` :The pattern appears to be ambiguous solely due to the small number of cases]`

# End Results : Dashboard


## Overview Report

![Overview Report](/Images/4.png "Overview Report")

## Insights Report
![Insights Report](/Images/5.png "Insights Report")


