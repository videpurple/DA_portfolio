# E-commerce Customer Retention Analysis (Full Report)

## 1. 🏢 Business Context (Simulated)
H Company is a UK-based online select shop that launched in December 2018. The platform sells a wide range of products, including gift items and practical home goods. This project simulates a real-world business scenario using open-source data from Kaggle, covering one year of transaction data to examine customer retention and uncover engagement trends.

## 2. 🎯 Analytical Goal
The objective of this analysis is to understand customer retention trends using cohort and range-based retention analysis. Based on observed patterns, this report also proposes data-driven strategies to improve customer re-engagement and long-term value.

## 3. 📊 Dataset Overview
- **Source**: Kaggle – [E-commerce Business Transactions](https://www.kaggle.com/datasets/gabrielramos87/an-online-shop-business)
- **Period**: December 1, 2018 – December 9, 2019

### Tables
#### `records`
| Column        | Description                    |
|---------------|--------------------------------|
| OrderId       | Unique order ID                |
| OrderDate     | Date of transaction            |
| ProductNo     | Product identifier             |
| ProductName   | Name of product                |
| Price         | Unit price in GBP              |
| Quantity      | Quantity purchased             |
| CustomerId    | Unique customer ID             |
| Country       | Customer's country             |

#### `customer_stats`
| Column           | Description                    |
|------------------|--------------------------------|
| CustomerId       | Unique customer ID             |
| first_order_date | Date of first purchase         |
| last_order_date  | Date of most recent purchase   |
| cnt_orders       | Number of orders               |
| sum_sales        | Total spending (GBP)           |

```sql
-- SQL for customer_stats generation
SELECT DISTINCT CustomerId,
       MIN(Date) AS first_order_date,
       MAX(Date) AS last_order_date,
       COUNT(DISTINCT OrderId) AS cnt_orders,
       ROUND(SUM(Price * Quantity), 2) AS sum_sales
FROM records
GROUP BY CustomerId;
```

## 4. ⚙️ Methodology
### Cohort Grouping
Customers were grouped by their **first purchase month** to evaluate retention trends over time.

### Range-Based Retention
To reflect the long purchase cycle of the service, retention was measured in **60-day intervals**, using the **median purchase cycle of 57 days** as a reference point.

## 5. 📈 Analysis Results
### Purchase Cycle
- Median purchase interval: **57 days**  
- Average purchase interval: **78.4 days**  
- Over **52.9%** of customers had a purchase cycle of 1–60 days

```sql
-- SQL: Purchase Cycle Grouping
WITH cycle AS (
  SELECT ROUND(DATEDIFF(last_order_date, first_order_date) / (cnt_orders - 1), 0) AS purchase_cycle,
         CustomerId
  FROM customer_stats
  WHERE cnt_orders >= 2
  GROUP BY purchase_cycle, CustomerId
  HAVING purchase_cycle > 0
)
SELECT
  COUNT(DISTINCT CASE WHEN purchase_cycle BETWEEN 1 AND 60 THEN CustomerId END) AS '1–60',
  COUNT(DISTINCT CASE WHEN purchase_cycle BETWEEN 61 AND 120 THEN CustomerId END) AS '61–120',
  ...
FROM cycle;
```

### Retention Trends
- Month 2 retention: **33.8%**, above industry benchmark (~20%)
- August cohort retention drops significantly at Month 8

![Figure 1 – Purchase Cycle Distribution](Retention Analysis/visuals/Number of Customers by Purchase Interval Group.png)
![Figure 2 – Retention by Cohort](Retention Analysis/visuals/retention_by_cohort.png)

## 6. 💡 Recommendations
### Challenge 1: Low new user acquisition
**→ Launch a referral program**  
Reward both the referrer and the invitee when the latter completes their first purchase.

### Challenge 2: Early churn within 2 months
**→ Trigger personalized push notifications**  
Send discount alerts or recommended items before Month 2 if no second purchase has occurred.

### Challenge 3: Low activity in August
**→ Run summer promotions**  
Capitalize on UK summer holidays with themed discounts and family-oriented products.

### Challenge 4: Flat long-term retention
**→ Use calendar-based reminders**  
Send alerts for birthdays and holidays to prompt gift purchases.

## 7. 📊 Seasonal Shopping Events (UK – 2019)
| Event             | Date          | Typical Items                  |
|------------------|---------------|--------------------------------|
| Valentine’s Day  | Feb 14        | Chocolates, jewelry, cards     |
| Mother’s Day     | Mar 31 (UK)   | Flowers, snacks, gifts         |
| Back to School   | September     | Books, stationery              |
| Halloween        | Oct 31        | Costumes, candy, decorations   |
| Black Friday     | Nov 29        | Electronics, high-ticket items |
| Christmas        | Dec 25        | Gifts, lights, cards           |

## 8. 📷 Visualizations
- **Figure 1**: Customer distribution by purchase interval
- **Figure 2**: Retention trends by cohort (60-day buckets)
- **Figure 3**: Drop in new customers after December 2018

> Include visual files under `/visuals` folder

## 9. 🔁 Reflection
- **Liked**: Gained clarity on how retention analysis works in practice
- **Lacked**: Missing product category limited segmentation
- **Learned**: Importance of defining the objective clearly
- **Longed for**: Deeper skills in applying classic vs. rolling retention models

> Project completed as part of **SQL Data Analysis Camp (Datarian)**  
> Reference: [Mixpanel – What’s a Good Retention Rate?](https://mixpanel.com/blog/whats-a-good-retention-rate/)
