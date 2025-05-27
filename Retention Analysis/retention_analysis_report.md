# 🏬 E-commerce Customer Retention Analysis

<br>

---
Author: Bora Lee
Skills: Google Sheets, Mysql
Period: 2023/07/15 → 2023/07/21
Contribution: 100%

---

<br>

## 1️⃣ Project Objective

### 1. Business Context (Assumed)

H Company is a UK-based online select shop that launched in December 2018. They offer a wide variety of products including gifts and household items. With one year of transaction data available, the company seeks to assess how well customers are retained over time and identify ways to improve long-term engagement.

### 2. Analytical Goal

This project aims to simulate a real-world analysis scenario using open-source data from Kaggle.

The goal is to analyze customer retention trends using cohort and range-based analysis, and to derive data-driven strategies for user re-engagement and lifecycle management.

### 3. Methodology

Retention was analyzed using two key methods:

- **Cohort Analysis**: Users were grouped into cohorts based on shared characteristics, primarily by signup or first purchase date, to analyze behavioral trends over time.
- **Retention Charting**: Retention rates for each cohort were visualized to identify patterns in user activity and drop-off across time periods.

<br>

## 2️⃣ Key Findings

### 1. Purchase Interval

|🔖 The **median purchase interval** for our users is 57 days|
|:-|


✔️ The analysis targeted users who made at least one repeat purchase after their first order, representing approximately **66%** of total users.
✔️ The purchase interval was calculated as:
`Last Purchase Date - First Purchase Date / (Number of Purchases - 1)`

<p>
    <img src = "C:\Users\dlqhf\Downloads\retention_boralee\Number_of_Customers_by_Purchase_Interval_Group.png", width = "45%"/>
    <img src = "C:\Users\dlqhf\Downloads\retention_boralee\Customer_Distribution_by_Purchase_Interval.png", width = "45%"/>
</p>


- More than half of repeat customers made their next purchase within **1 to 60 days**.
- While the **average interval** was 78.4 days, the distribution showed high variability due to the wide range of product types offered.
- Therefore, the **median** was used as a more stable retention benchmark, as it is less affected by extreme outliers.

<details>
<summary>SQL Query - Purchase Cycle Distribution</summary>
<div markdown="1">

```sql
WITH cycle AS (
    SELECT ROUND(DATEDIFF(last_order_date, first_order_date) / (cnt_orders - 1), 0) AS purchase_cycle 
    	 , CustomerId
    FROM customer_stats
    WHERE cnt_orders >= 2
    GROUP BY purchase_cycle, CustomerId HAVING purchase_cycle > 0
    ORDER BY purchase_cycle
)
SELECT COUNT(DISTINCT CASE WHEN purchase_cycle BETWEEN 1 AND 60 THEN CustomerId END) AS '1~60'
     , COUNT(DISTINCT CASE WHEN purchase_cycle BETWEEN 61 AND 120 THEN CustomerId END) AS '61~120'
     , COUNT(DISTINCT CASE WHEN purchase_cycle BETWEEN 121 AND 180 THEN CustomerId END) AS '121~180'
     , COUNT(DISTINCT CASE WHEN purchase_cycle BETWEEN 181 AND 240 THEN CustomerId END) AS '181~240'
     , COUNT(DISTINCT CASE WHEN purchase_cycle BETWEEN 241 AND 300 THEN CustomerId END) AS '241~300'
     , COUNT(DISTINCT CASE WHEN purchase_cycle BETWEEN 301 AND 360 THEN CustomerId END) AS '301~360'
     , COUNT(DISTINCT CASE WHEN purchase_cycle > 360 THEN CustomerId END) AS '360~'
FROM cycle;
    
-- Note: The average and median purchase intervals were calculated separately using Google Sheets for better visualization.
```
</div>
</details>
    

### 2. **Retention Analysis**

> To better understand customer engagement over time, we applied **range-based retention analysis** using 60-day (2-month) intervals, based on the median purchase interval of 57 days.
Each “Month 0” represents the number of users who made their **first purchase** in that particular month.
> 



🔖 Compared to December 2018, the number of new customers in January 2019 dropped sharply. By November 2019, this figure had declined by approximately **72%** from the baseline.
The **lowest new user count** occurred in August 2019, followed by a **63% increase** in September—likely due to the back-to-school season.






![cohort_chart](cohort_chart.png)

- From October onward, new user acquisition remained steady, supported by **seasonal shopping events** such as Halloween, Black Friday, and early Christmas preparation.
  
<details>
<summary>Top 10 Most Purchased Products by October First-Time Customers</summary>
<div markdown = 1>    
    
| Product Name | Total Purchases  |
| --- | --- |
| Popcorn Holder | 945 |
| Wooden Heart Christmas Scandinavian | 805 |
| World War 2 Gliders Asstd Designs | 738 |
| Assorted Colours Silk Fan | 704 |
| Paper Chain Kit 50'S Christmas | 576 |
| Assorted Colour Bird Ornament | 567 |
| Vintage Snap Cards | 530 |
| Christmas Hanging Star With Bell | 473 |
| Hanging Heart With Bell | 442 |
| 60 Cake Cases Vintage Christmas | 435 |

</div>
</details>

<details>
<summary> Top 10 Most Purchased Products by November First-Time Customers</summary>
<div markdown = 1>     
    
| Product Name | Total Purchases |
| --- | --- |
| Asstd Design 3d Paper Stickers | 12544 |
| Popcorn Holder | 3373 |
| Rabbit Night Light | 1645 |
| Paper Chain Kit 50'S Christmas | 1538 |
| Jumbo Bag Red Retrospot | 1170 |
| Jumbo Bag 50'S Christmas | 1103 |
| Traditional Pick Up Sticks Game | 978 |
| Wooden Star Christmas Scandinavian | 934 |
| Doughnut Lip Gloss | 886 |
| Traditional Naughts & Crosses | 860 |

</div>
</details>

<details>
<summary>SQL: Top Products Purchased by First-Time Customers in October & November</summary>
<div markdown = 1>    

```sql
-- Purchased Products by October First-Time Customers
WITH records_join AS (
SELECT r.CustomerId
     , r.OrderDate
     , DATE_FORMAT(c.first_order_date, '%Y-%m-01') first_order_month
     , r.ProductName
     , r.Price
     , r.Quantity
     , r.Country
FROM records r
 INNER JOIN customer_stats c ON r.CustomerId = c.CustomerId
)
SELECT DISTINCT ProductName
     , SUM(Quantity) cnt_product
FROM records_join
WHERE first_order_month = '2019-10-01'
AND OrderDate BETWEEN '2019-10-01' AND '2019-10-31'
GROUP BY ProductName
ORDER BY cnt_product DESC;
    
-- Purchased Products by November First-Time Customers
WITH records_join AS (
SELECT r.CustomerId
     , r.OrderDate
     , DATE_FORMAT(c.first_order_date, '%Y-%m-01') first_order_month
     , r.ProductName
     , r.Price
     , r.Quantity
     , r.Country
FROM records r
     INNER JOIN customer_stats c ON r.CustomerId = c.CustomerId
)
SELECT DISTINCT ProductName
     , SUM(Quantity) cnt_product
FROM records_join
WHERE first_order_month = '2019-11-01'
AND OrderDate BETWEEN '2019-11-01' AND '2019-11-30'
GROUP BY ProductName
ORDER BY cnt_product DESC;
```
</div>
</details>
    
<details>
<summary> UK Seasonal Shopping Events – 2019 Calendar</summary>
<div markdown = 1>
    
    
| Holiday/Event | Date (2019) | Description & Popular Items |
| --- | --- | --- |
| **New Year** | January 1 | New Year's greetings, wellness gifts, good-luck items |
| **Valentine’s Day** | February 14 | Gifts for loved ones – chocolates, cards, jewelry |
| **St. Patrick’s Day** | March 17 | Irish-themed goods, green clothing, party supplies |
| **Mother’s Day** | March 31 (UK date) | Flowers, snacks, and jewelry for mothers |
| **Easter Holiday** | April 19–22 | Easter eggs, decorations, religious items |
| **Father’s Day** | June 16 | Ties, books, sports goods, grooming sets |
| **Back to School** | September | School supplies – books, stationery, backpacks |
| **Halloween** | October 31 | Costumes, makeup, candy, decorations |
| **Black Friday** | November 29 | Big sales on electronics and high-ticket items |
| **Cyber Monday** | December 2 | Online-exclusive promotions on various products |
| **Christmas** | December 25 | Gift items, greeting cards, candles, lights |
| **Boxing Day** | December 26 | Post-Christmas sales – clothing, electronics, perfume |

</div>
</details>

<br>

🔖 The **average retention rate at Month 2** was **33.8%**, which is relatively high compared to industry benchmarks (typically below 20% after 8 weeks[¹](https://www.notion.so/E-commerce-Customer-Retention-Analysis-1f5aa5dfec478105b447ec2506ba0d24?pvs=21)[**⁾**](https://www.notion.so/E-commerce-Customer-Retention-Analysis-1f5aa5dfec478105b447ec2506ba0d24?pvs=21)).
Although Month 2 exhibited the **lowest average retention rate**, a **gradual upward trend** was observed in later months, with some temporary drops and stabilizations in between.
→ It is essential to investigate strategies to improve Month 2 retention, as it represents a key drop-off point despite relatively strong early engagement.

</aside>

![cohort_rate_chart](cohort_rate_chart.png)

![Retention Trends by Cohort over 12 Months.png](Retention_Trends_by_Cohort_over_12_Months.png)


🔖 A notable drop in retention was found around **Month 8** across most cohorts, coinciding with **August**, a month without major promotional events or campaigns.

![cohort_rate_chart_highlight](cohort_rate_chart_highlight.png)

<details>
<summary>SQL Query – Generating 2-Month Retention Cohorts by First Purchase</summary>
    
```sql
WITH records_preprocessed AS (
 SELECT r.CustomerId
      , DATE_FORMAT(r.OrderDate, '%Y-%m-01') order_month
      , DATE_FORMAT(c.first_order_date, '%Y-%m-01') first_order_month
FROM records r
 INNER JOIN customer_stats c ON r.CustomerId = c.CustomerId
)
    
SELECT first_order_month
     , COUNT(DISTINCT CustomerId) 'Month 0'
     , COUNT(DISTINCT CASE WHEN order_month = DATE_ADD(first_order_month, INTERVAL 1 MONTH) OR order_month = DATE_ADD(first_order_month, INTERVAL 2 MONTH)
    		 THEN CustomerId END) 'Month 2'
     , COUNT(DISTINCT CASE WHEN order_month = DATE_ADD(first_order_month, INTERVAL 3 MONTH) OR order_month = DATE_ADD(first_order_month, INTERVAL 4 MONTH)
    		 THEN CustomerId END) 'Month 4'
     , COUNT(DISTINCT CASE WHEN order_month = DATE_ADD(first_order_month, INTERVAL 5 MONTH) OR order_month = DATE_ADD(first_order_month, INTERVAL 6 MONTH)
    		 THEN CustomerId END) 'Month 6'
     , COUNT(DISTINCT CASE WHEN order_month = DATE_ADD(first_order_month, INTERVAL 7 MONTH) OR order_month = DATE_ADD(first_order_month, INTERVAL 8 MONTH)
    		 THEN CustomerId END) 'Month 8'
     , COUNT(DISTINCT CASE WHEN order_month = DATE_ADD(first_order_month, INTERVAL 9 MONTH) OR order_month = DATE_ADD(first_order_month, INTERVAL 10 MONTH)
    		 THEN CustomerId END) 'Month 10'
     , COUNT(DISTINCT CASE WHEN order_month = DATE_ADD(first_order_month, INTERVAL 11 MONTH) OR order_month = DATE_ADD(first_order_month, INTERVAL 12 MONTH)
    		 THEN CustomerId END) 'Month 12'
FROM records_preprocessed
GROUP BY first_order_month
ORDER BY first_order_month;
```

</div>
</details>
    

<br>


## 3️⃣ Recommendations

Based on the retention analysis and customer behavior patterns, we propose the following action plans to address key challenges:

---

#### Challenge 1: Decreasing number of new customers

![Monthly First-Time Customer Count.png](Monthly_First-Time_Customer_Count.png)

**✅ Action Plan 1 – Launch a Referral Program**

Introduce a referral system where existing users can invite friends. If a referred friend makes a purchase, both users can choose between a reward (e.g., free shipping or bonus points).

This approach not only encourages new signups but also drives first purchases, increasing the pool of potential repeat customers.

**✅ Action Plan 2 – First Purchase Incentives**

Offer exclusive perks such as gift wrapping or a discount for first-time buyers. Promote these benefits through marketing campaigns to attract more first-purchase users.

---

#### **Challenge 2: Low retention within the first two months after initial purchase**

**✅ Action Plan – Personalized Push Notifications**

Send push notifications before the 2-month mark to users who haven't made a second purchase. Include personalized product suggestions based on previous browsing behavior and offer surprise discount coupons to re-engage them.

---

#### **Challenge 3: Extremely low purchasing activity in August**

**✅ Action Plan – Back-to-School and Summer Campaign**

Since August coincides with the UK summer holiday period, run seasonal promotions on vacation items, summer goods, and family activity kits (e.g., baking or crafts) to stimulate purchases.

---

#### **Challenge 4: Long-term retention slope flattens too early**

**✅ Action Plan – Calendar-Based Gifting Reminders**

Allow users to sync their calendars. For those who opt in, send gift reminders and recommendations ahead of birthdays and anniversaries of friends and family.

Even without calendar sync, suggest gifts for widely celebrated events (e.g., New Year’s Day, Mother’s Day, Father’s Day) to maintain engagement and repeat visits.

<br>

## 4️⃣ Dataset Overview

Source: Kaggle [**E-commerce Business Transaction**](https://www.kaggle.com/datasets/gabrielramos87/an-online-shop-business)

Collection Period: December 1, 2018 – December 9, 2019

<details>
<summary>1. records</summary>
<div markdown = 1>
  - Raw transaction data containing individual purchase events.
    
    
| Column | Description |
| --- | --- |
| `OrderId` | Unique order identifier |
| `OrderDate`  | Date of transaction |
| `ProductNo` | Product identifier |
| `ProductName` | Name of the product |
| `Price` | Unit price in GBP |
| `Quantity` | Quantity purchased |
| `CustomerId` | Unique customer ID |
| `Country` | Customer’s country of residence |

</div>
</details>


<details>
<summary>2. customer_stats</summary>
<div markdown = 1>
→ A summary table created for retention analysis by aggregating the `records` table.
    
| Column | Description |
| --- | --- |
| `CustomerID` | Unique customer ID |
| `first_order_date` | Date of first purchase |
| `last_order_date` | Date of most recent purchase |
| `cnt_orders` | Total number of orders |
| `sum_sales` | Total amount spent (in GBP) |

</div>
</details>

<details>
<summary>SQL Query Uesed for Aggregaion</summary>
<div markdown = 1>
        
 ```sql
SELECT DISTINCT CustomerId
     , MIN(Date) first_order_date
     , MAX(Date) last_order_date
     , COUNT(DISTINCT OrderId) cnt_orders
     , ROUND(SUM(Price * Quantity), 2) sum_sales
FROM records
GROUP BY CustomerId;
 ```
</div>
</details>
        
<br>

## 5️⃣ Reflection

**✅ Liked**

When I first learned retention analysis, I found the cohort charts somewhat confusing. However, by working through this project step by step, I gained a much clearer understanding and was able to deepen my knowledge of the methodology.

**⚠️ Lacked**

The dataset did not include product category information, which made it impossible to calculate purchase intervals by category. This limited the scope of behavioral segmentation.

**📚 Learned**

Beyond the technical understanding of retention metrics, this project reinforced the importance of clearly defining the objective before beginning any analysis.

**🎯 Longed for**

I would like to build the ability to compare different retention types (e.g., bounded, classic, rolling) and determine which approach is most appropriate based on the business context.

---

¹ ⁾Reference : **Mixpanel** [User retention rate for mobile apps and websites](https://mixpanel.com/blog/whats-a-good-retention-rate/)

> 📌 This project was completed as part of the "SQL Data Analysis Camp" by Datarian.
