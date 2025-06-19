# 🧩 Customer Segmentation Using RFM Analysis and Clustering

This project segments retail customers based on their purchase behavior using RFM (Recency, Frequency, Monetary) analysis and clustering. The goal is to identify actionable customer groups for tailored marketing strategies.

## 🎯 Objectives

- Classify customers based on their purchase patterns using RFM scores
- Apply KMeans clustering to segment customers into meaningful groups
- Develop marketing strategies based on customer behavior and engagement level

## 🛠 Tools & Technologies

- Python (Google Colab)
- pandas, scikit-learn, seaborn, matplotlib
- Dataset: [Dacon - Customer Segmentation Challenge](https://dacon.io/competitions/official/236222/data)

## 🔍 Key Steps

1. Merged and preprocessed transaction, customer, and discount datasets
2. Calculated RFM scores using quantiles and assigned scores (1–4)
3. Applied KMeans clustering (k=4) without scaling due to uniform scoring
4. Profiled each cluster based on RFM metrics, demographics, and behavior
5. Suggested targeted marketing actions for each segment

## 💡 Key Insights

- **Cluster 1 (Loyal)**: High spenders who purchase frequently and recently — should be retained with loyalty programs
- **Cluster 0 (Lost)**: Inactive customers with low purchase history — need reactivation campaigns
- **Cluster 2 (Potential)**: Recently active but low spend — opportunities for upselling
- **Cluster 3 (Enthusiast)**: High spend but infrequent — ideal for targeted seasonal promotions

## 📈 Marketing Recommendations

- Display personalized product suggestions based on past behavior
- Offer coupon pop-ups at checkout for better usage
- Provide loyalty rewards for frequent or high-spending customers
- Run seasonal campaigns to re-engage inactive customers

## 🔗 Files & Resources

- 📒 [Notebook on GitHub](https://github.com/videpurple/DA_portfolio/blob/main/Customer%20Segmentation/Notebook/customer_segmentation_rfm_analysis.ipynb)
- 📊 All charts and visualizations are included in the notebook
