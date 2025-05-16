# 🔄 Funnel Analysis for User Signup Optimization

This project analyzes the user signup flow of a service platform to identify drop-off points and improve conversion rates.

## 🎯 Objectives

- Understand user behavior through event logs
- Calculate conversion rates at each stage of the signup process
- Compare device and country-level conversion performance
- Propose data-driven improvements to increase signup completion

## 🛠 Tools & Technologies

- PostgreSQL
- Power BI
- Mode Analytics Dataset: [User Engagement Tutorial](https://mode.com/sql-tutorial/a-drop-in-user-engagement)

## 🧪 Key Steps

1. Queried raw event log data using PostgreSQL
2. Measured conversion between steps: create_user → enter_email → enter_info → complete_signup
3. Visualized funnel breakdown by device and country in Power BI
4. Identified segments with high dropout rates and suggested improvement points

## 💡 Key Insights

- Conversion rates were significantly lower on tablets and in certain countries (e.g., Turkey, Austria)
- Notebooks and PCs showed slightly higher completion rates
- Recommended UI testing and localization support for low-converting devices/regions

## 🔗 Files & Resources

- 📎 [Funnel Analysis Report (PDF)](https://github.com/videpurple/portfolio/blob/main/%ED%8D%BC%EB%84%90%EB%B6%84%EC%84%9D/project/%F0%9F%92%BBFunnel%20%EB%B6%84%EC%84%9D%EC%9D%84%20%ED%86%B5%ED%95%9C%20%EC%84%9C%EB%B9%84%EC%8A%A4%20%ED%9A%8C%EC%9B%90%20%EA%B0%80%EC%9E%85%20%EC%9C%A0%EB%8F%84%20%EB%B0%A9%EC%95%88.pdf)
- 🧮 Dataset & SQL Queries available in project folder
