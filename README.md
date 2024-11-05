# **Project One**

## **Project Title: Sales Performance of a Retail Store**


### **Project Overview**
---
In this project, the sales performance of a retail store is analysed. However, before the analysis, the Capstone dataset was subjected to data cleaning and preparation to ensure no missing or incorrect values in critical columns like product names, order dates, and sales figures.


### **Objectives**
---
The goal of this analysis is to explore sales data to uncover key insights such as top-selling products, regional performance, and monthly sales trends.


### **Data Sources**
---
The main data source for this analysis is salesdata.xls, titled LITA Capstone Sales Data, provided by the Incubator as part of the assessment for the LITA training program I completed.


### **Tools Used**
---
The customer segmentation data was analysed using three major analysis tools which are:
- Microsoft Excel [Download Here] (https://www.microsoft.com)
   1. For Data Cleaning
   2. For Analysis
- SQL (Structured Query Language) was used to write queries that further extract insights from the sales data such as top-selling products, regional 
performance, and monthly sales trends etc.; and
- Power BI
   1. Is used to create an interesting story-telling dashboard that presents the outcomes of the analysis. 


### **Data Cleaning and Preparation**
---
In the initial phase of cleaning and preparation of the data, the following steps are taken after the loading of the data:
- The data type is checked to ensure they are correctly formatted. 
- Missing values or variables are checked to detect their presence in the data.
- Duplicate values were checked and removed from the data while in the Excel environment to obtain unique entries in the dataset to attain the right information from the provided data.


### **Exploratory Data Analysis**
---
Exploratory Data Analysis (EDA) is a method for examining datasets to highlight their key characteristics, often using visual techniques. This process typically involves applying summary statistics—like mean, median, and standard deviation—and visualizations, such as bar charts and pie charts, to gain a deep understanding of the data. In this project, pivot table summaries help identify data types, detect missing values, and pinpoint outliers or errors. Additionally, EDA offers insights into total sales by product, region, and month etc., revealing the performance of the products and sales generated. Through this approach, the following questions about the data are addressed:
- What are the total sales by product, region, and month? 
- What is the average sales per product and total revenue by region?
- Which of the products is the highest-selling product by total sales value?
- What are the monthly sales totals for the current year? 
- What are the top 5 customers by total purchase amount? 
- What is the percentage of total sales contributed by each region? 
- Which of the products has no sales in the last quarter?


### **Data Analysis**
---
In this section, queries written to extract key insights into about the data are presented:
```SQL
Create database Project1_LITA

----------LITA Capstone Dataset Project 1--------
select * from [dbo].[LITASalesDataProject]



----Retrieve the total sales for each product category----
select sum(Total_Sales) as [Sales Per Product], 
	Product from [dbo].[LITASalesDataProject]
Group by Product


------Number of sales transactions in each region-------
select sum(Total_Sales) as [Sales Per Region], 
		Region from [dbo].[LITASalesDataProject]
group by Region 
	Order by [Sales Per Region] Desc;


-----Highest selling product by total sales value---------
Select Top 1
    Product,
    sum(Total_Sales) as [Total Sales Value]
From 
    [dbo].[LITASalesDataProject]
Group by 
    product
Order by 
    [Total Sales Value] Desc;



----Total revenue per product------------
Select
    product, 
    sum(Total_Sales) as [Total Revenue Per Product]
From 
    [dbo].[LITASalesDataProject]
Group by 
    product
order by 
    [Total Revenue Per Product] desc;


--------Monthly sales totals for the current year.
Select 
    Format(OrderDate, 'MMM') as [Month Name],
    sum(Total_Sales) as [Current Year Total Sales]
From 
    [dbo].[LITASalesDataProject]
Where 
    Year(OrderDate) = 2024
Group by 
    Format(OrderDate, 'MMM'),
		Month(OrderDate)
Order by 
    month(OrderDate);



-----Top 5 customers by total purchase amount--------
Select 
    Top 5 CustomerID, 
     Sum(Total_Sales) as [Total Purchase Amount]
From 
    [dbo].[LITASalesDataProject]
Group by 
    CustomerID 
Order by 
    [Total Purchase Amount] Desc;



----Percentage of total sales contributed by each region----
Select 
    Region, 
		(Sum(Total_Sales) * 100.0 / (Select Sum(Total_Sales) From [dbo].[LITASalesDataProject])) as [Sales Percentage]
From 
    [dbo].[LITASalesDataProject]
Group by 
    Region
order by 
    [Sales Percentage] desc;


----Products with no sales in the last quarter------
with LastQuarterDates as (
    Select 
        DateAdd(Quarter, DateDiff(Quarter, 0, GetDate()) - 1, 0) as start_date,
        DateAdd(Quarter, DateDiff(Quarter, 0, GetDate()), 0) - 1 as end_date
)
Select 
    Product
From
	[dbo].[LITASalesDataProject]
Where 
    Product NOT IN (
        Select Product 
        From [dbo].[LITASalesDataProject]
        where OrderDate Between 
            (Select start_date From LastQuarterDates) AND (Select end_date from LastQuarterDates)
    )
Group by 
    Product;
```

```Excel
Total Revenue= SUM(H2:H9922)
Average Sales by Order=AVERAGE(H2:H50001)
Revenue by East Region=SUMIF(D2:D9922,"East",H2:H9922)
Revenue by West Region=SUMIF(D2:D9922,D6,H2:H9922)
Revenue by North Region=SUMIF(D2:D50001,"North",H2:H50001)
Revenue by South Region=SUMIF(D2:D9922,"South",H2:H9922)
```



### **Findings of the Analysis**
---
Here the results of the analysis of the sales performance of the retail store are presented. The following results are obtained:

#### *A. Total Sales Overview*
---
The store's total sales revenue across all product categories is 2,101,090. The breakdown of this revenue by products is as follows:
Insights by Product Category
1.	Shoes:
  - Total Sales: 613,380
  - Percentage of Total Sales: 29.2%
Observation: Shoes are the highest revenue-generating product, contributing nearly one-third of the store's total sales. This product category appears to be very popular, indicating strong customer demand.
2.	Shirt:
  - Total Sales: 485,600
  - Percentage of Total Sales: 23.1%
Observation: Shirts are the second highest revenue contributor with over 23% of total sales. This indicates consistent demand and positions shirts as another essential product category for the store.
3.	Hat:
  - Total Sales: 316,195
  - Percentage of Total Sales: 15.1%
Observation: Hats contribute 15.1% to total sales, making them a significant mid-range performer in terms of revenue.
4.	Gloves:
  - Total Sales: 296,900
  - Percentage of Total Sales: 14.1%
Observation: Gloves generate 14.1% of total revenue. Their sales are close to those of hats, suggesting they are also popular but less so than shirts or shoes.
5.	Jacket:
  - Total Sales: 208,230
  - Percentage of Total Sales: 9.9%
Observation: Jackets account for just under 10% of total revenue. While not as popular as shoes or shirts, they are still a significant product category, potentially influenced by seasonal demand.
6.	Socks:
  - Total Sales: 180,785
  - Percentage of Total Sales: 8.6%
Observation: Socks have the lowest sales among all product categories, contributing 8.6% to total revenue. This could indicate lower demand or the potential for increased sales through promotional strategies.

#### *Key Insights from Total Sales Overview*
1. Top Performers (Shoes and Shirts): Shoes and shirts are driving a substantial portion of the store's revenue. It would be beneficial to maintain high inventory levels for these items, as they are in strong demand.
2. Mid-Tier Products (Hats and Gloves): Hats and gloves, with 15.1% and 14.1% of total sales respectively, are solid performers. Offering seasonal promotions or bundling these items with high-performing products (e.g., shirts or shoes) could improve their revenue contribution.
3. Opportunities for Jackets and Socks: Jackets and socks generate the lowest revenue among all categories. It may be helpful to run discount promotions or flash sales on these products to stimulate interest and increase sales. Additionally, analysing customer feedback on these products could reveal insights into potential barriers to sales, such as pricing or quality perceptions, and help adjust strategies accordingly.


#### **B. Sales Performance Analysis by Product and Year**
---
This analysis provides a breakdown of revenue and quantity sold by product category over the years 2023 and 2024, along with quarterly and monthly performance insights. The total revenue generated across all products is 2,101,090, with a total of 68,461 units sold.

#### *Yearly and Quarterly Breakdown*
1. Shoes
- 2023 Revenue: 277,380 (6,942 units Sold)
- 2024 Revenue: 336,000 (7,460 units Sold)
#### *Key Insights:*
- In both years, sales peak in Q1. In 2024, February alone accounts for 4,980 units, significantly boosting quarterly revenue.
- Seasonal Impact: Sales drop in Q3, suggesting a potential seasonal decline in late summer.

2. Shirts
- 2023 Revenue: 287,200 (8,420 units sold)
- 2024 Revenue: 198,400 (3,968 units sold)
#### *Key Insights:*
- Q1 consistently drives sales, with January being strong in both years.
- Decline in Sales: Revenue and quantity drop in 2024 compared to 2023, indicating possible shifts in demand.

3. Hats
- 2023 Revenue: 87,115 (6,965 units sold)
- 2024 Revenue: 229,080 (8,964 units sold)
#### *Key Insights:*
- Substantial Growth in 2024: Sales nearly triple in 2024, with Q1 and Q3 showing particularly high demand.
- Monthly Insights: March and August contribute significantly, indicating potential popularity during early spring and late summer.

4. Gloves
- 2023 Revenue: 148,700 (6,441 units sold)
- 2024 Revenue: 148,200 (5,928 units sold)
#### *Key Insights:*
- Steady Demand: Sales remain consistent across the years.
- Seasonal Trends: High sales in Q2 and Q4 suggest that gloves might be seasonal products, potentially related to colder months.

5. Jackets
- 2023 Revenue: 163,590 (3,964 units sold)
- 2024 Revenue: 44,640 (1,488 units sold)
#### *Key Insights:*
- Decline in Sales for 2024: There’s a noticeable reduction in both revenue and units sold in 2024, which could indicate a decrease in demand or possible inventory limitations.
- Peak Months: Q2 and Q4 drive sales in 2023, potentially related to seasonal weather changes.

6. Socks
- 2023 Revenue: 141,345 (5,949 units sold)
- 2024 Revenue: 39,440 (1,972 units sold)
#### *Key Insights:*
- Strong Q4 Performance in 2023: October shows the highest revenue and units sold.
- Sales Drop in 2024: Both revenue and units sold decrease in 2024, suggesting shifts in demand.

#### *Total Quarterly Performance Summary*
Across all products:
- Q1 is the strongest quarter overall, with consistently high revenue and quantity sold, particularly for shoes and hats.
- Q4 shows strong performance for items like gloves, jackets, and socks, likely indicating seasonal purchasing trends.
- While hats and gloves are on the rise, products like jackets and socks are seeing declines in 2024, suggesting a need for further market research or strategy adjustments.


#### **C. Regional Product Sales Breakdown (Percentage of Total Revenue)**
---
This analysis shows the percentage contribution of each product to total revenue by region, highlighting the most and least popular products in each area. Understanding these regional preferences can guide marketing and inventory strategies.

#### *Regional Sales Contribution*
This breakdown highlights the revenue contribution of each product category by region, allowing us to identify which products perform best in specific areas.

#### *Key Observations from the Regions*
1. South Region
- Total Revenue Contribution: 44.16%
- Top Products:
  - Shoes: 26.00%
  - Gloves: 11.78%
  - Socks: 6.37%
Insights: The South region is the top-performing market, with a dominant preference for shoes, making up over a quarter of the region's revenue. Gloves and socks also contribute significantly, suggesting a strong demand for footwear and winter accessories in this region.

2. East Region
- Total Revenue Contribution: 23.13%
- Top Products:
  - Shirt: 11.31%
  - Hat: 5.10%
  - Jacket: 4.95%
Insights: The East region shows a balanced preference across different product categories, with shirts being the most popular item. The demand for hats and jackets indicates an interest in outdoor and seasonal wear.

3. North Region
- Total Revenue Contribution: 18.42%
- Top Products:
  - Shirt: 11.80%
  - Jacket: 4.96%
  - Hat: 1.65%
Insights: Shirts are the primary driver of revenue in the North, followed by jackets. This region shows potential for further growth in clothing items, especially with a focus on outerwear.

4. West Region
- Total Revenue Contribution: 14.29%
- Top Products:
  - Hat: 8.30%
  - Gloves: 2.35%
  - Socks: 2.23%
Insights: The West region has a unique preference for hats, contributing significantly to its overall revenue. Gloves and socks are also popular, suggesting a market trend toward accessories.


#### **D. Customer Acquisition and Retention by Year and Quarter**
---
This analysis provides insight into the customer count trends across different quarters in 2023 and 2024, revealing quarterly performance and growth patterns.

#### *Yearly Customer Count Overview*
##### *Quarterly Breakdown*
1. 2023:
- Q1: 1,490
- Q2: 1,489
- Q3: 1,489
- Q4: 1,484
•	Total Number of Customers for 2023: 5,952
2. 2024:
- Q1: 1,492
- Q2: 1,483
- Q3: 994 (incomplete quarter)
•	Total Number of Customers for 2024: 3,969

#### *Key Observations*
- Consistent Performance in 2023: The customer count for each quarter in 2023 remained stable, averaging around 1,490 customers per quarter. This steady trend suggests a consistent customer acquisition or retention rate throughout the year.
- Early 2024 Growth:
  - The first quarter of 2024 shows a slight increase in customer count (1,492) compared to each quarter in 2023, indicating potential growth momentum at the beginning of the year.
  - Q2 has a minor dip to 1,483, close to 2023 averages, suggesting a steady continuation.
- Q3 2024:
  - The customer count in Q3 2024 is currently recorded at 994, which appears incomplete or reflects a decrease. If this is due to incomplete data, further tracking is needed to assess the trend.
By understanding customer acquisition and retention patterns, the business can make data-driven decisions to sustain growth and improve customer loyalty across quarters.

#### **Monthly Revenue Analysis**
---
This report examines the monthly revenue distribution for the retail store, identifying peak months and potential seasonal trends.

##### *Monthly Revenue Overview*

##### *Key Observation*
1. Peak Revenue Months:
    - February generated the highest revenue at 546,300, which is significantly above the monthly average.
    - July also performed well with 274,800, indicating another potential high-sales period.
2. Lowest Revenue Months:
    - April and September had the lowest revenues, with 46,865 and 34,720, respectively. This may indicate lower seasonal demand or an opportunity to introduce sales or promotional activities during these months to boost revenue.
3. Steady Performance Months:
    - January, June, and August maintained moderately high revenues, suggesting stable customer demand during these periods.
4. End-of-Year Performance:
    - Revenue tends to decline towards the end of the year, with October to December showing lower sales figures. Although October has some sales activity (133,920), November and December revenues are relatively low.
