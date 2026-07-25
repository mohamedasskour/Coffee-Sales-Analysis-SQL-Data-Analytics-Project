
☕️ Coffee Sales Analysis | SQL Data Analytics Project

My Tableau Data Visualization: Coffee Sales Dashboard: 
https://public.tableau.com/views/CoffeeSalesDashboard_17800697210440/SalesDashboard?:language=fr-FR&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link

📌 Project Overview
This project analyzes a coffee sales dataset using SQL to uncover business insights, sales performance, customer behavior, and growth trends.
The goal is to simulate a real-world data analyst workflow, transforming raw transactional data into meaningful insights that support decision-making.

🎯 Business Questions
This analysis answers key business questions:
* Which coffee products generate the most revenue and profit?
* How does the business perform over time (MoM / YoY growth)?
* Which countries and cities drive the most sales?
* Do loyalty customers contribute more to revenue?
* What are the main drivers of business performance?

🗂️ Dataset Description
The project is based on three main tables:
* Orders → Sales transactions (Order_ID, Order_Date, Customer_ID, Product_ID, Quantity, Customer_Name, Email, Country, Coffee_Type_Name, Roast_Type_Name, Size, Unit_Price, Sales, Card_Loyalty)
* Customers_coffee → Customer data (Customer_ID, Customer_Name, Email, City, Country, Postcode)
* Products_C → Product details (Product_ID, Price_per_100_g, Profit)

🧱 Project Structure
The analysis is structured into five main sections:
💰 1. Business Overview
* Total Sales
* Total Profit
* Total Orders
* Profit Margin
* AOV (Average Orders Value)
👉 Provides a high-level understanding of overall business performance.

🌍 2. Geography Analysis
* Sales by Country
* Sales by City
* Country segmentation (High / Medium / Low)
👉 Identifies key markets and potential expansion opportunities.

📈 3. Time Analysis
* Sales by Year & Month
* Month-over-Month (MoM) Growth
* Year-over-Year (YoY) Growth
👉 Reveals trends, seasonality, and business growth patterns.

🧍 4. Customer Insights
* Top 5 Customers
* Cumulative Sales Analysis
* Loyalty vs Non-Loyal Customers
👉 Evaluates customer value and the impact of loyalty programs.

☕ 5. Product Performance
* Coffee Type & Roast Type analysis
* Sales and Profit contribution (Part-to-Whole)
* Product segmentation (High / Medium / Low)
* Top-performing months per coffee type
* Best-selling coffee size
👉 Identifies top-performing products and optimization opportunities.

📊 Key Insights
* ☕ Certain coffee types (e.g., Liberica and Excelsa) act as primary revenue drivers
* 🌍 A small number of countries (United States) generate the majority of total sales
* 📈 The business shows clear growth trends over time, with seasonal peaks
* 🧍 No-Loyalty customers tend to generate higher value per order
* ⚖️ Some products Like Arabica and Robusta have high sales but low profitability, indicating optimization potential

🚀 Recommendations
* Focus on high-profit and high-performing coffee types (Liberica and Excelsa)
* Expand in top-performing countries (United States) and cities (Washington, Houston, Toledo und New York City)
* Improve or re-evaluate low-margin products
* Strengthen the customer loyalty program
* Leverage seasonal trends for marketing and promotions for each Coffee Type

🛠️ Tools & Skills Used
* Excel (Cleaning and preparing Data)
* SQL (CTEs, Window Functions, Aggregations)
* Data Analysis
* Business Intelligence Thinking
* KPI Design
* Data Segmentation

📌 Key SQL Techniques
* SUM(), COUNT(), AVG()
* GROUP BY
* CTE (WITH)
* LAG() for growth analysis
* NTILE() for segmentation
* ROW_NUMBER() for ranking
* SUM() OVER() for cumulative analysis

📈 Future Improvements
* Build an interactive dashboard using Power BI / Tableau
* Add forecasting models for sales prediction
* Include customer segmentation (RFM Analysis)
* Automate reporting using SQL views

🏁 Conclusion
This project demonstrates how SQL can be used not only for querying data, but for delivering actionable business insights.
It reflects a real-world analytics workflow:
Data → Analysis → Insights → Recommendations

Made with ♡ by Asskour Analyst
