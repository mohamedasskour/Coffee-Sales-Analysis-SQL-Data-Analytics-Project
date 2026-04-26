
-- FINAL REPORT STRUKTUR Coffee_Orders --

/*******
Overview:
    1) Total Sales
    2) Total Profit
    3) Total Orders
    4) Profit Margin
    5) AOV (Avg. Orders Value)
*/
Select SUM(od.Sales) AS Total_Sales, 
ROUND(SUM(pd.Profit), 2) AS Total_Profit, 
Count(DISTINCT Order_ID) AS Total_Orders, 
SUM(pd.Profit) * 100 / Sum(od.Sales) AS Profit_Margin,
SUM(od.Sales) * 1.0 / Count(DISTINCT Order_ID) AS AOV
From Orders od 
Left Join Products_C pd 
ON od.Product_ID = pd.Product_ID;
/* 
Insights:
* Das Business generiert seit 4 Jahre (von 2019 bis 2022) insgesamt 45135,35 € Umsatz und 1301,38 € Profit
* Die Anzahl der Bestellungen (ca. 957 Orders) zeigt eine stabile Nachfrage
* Das Verhältnis von Sales zu Profit deutet auf eine solide Marge (ungefähr 2,88%) hin
*/

/********
Geography:
    1) Sales by Country
    2) Sales by Cities
    3) Which Country has Most Sales, Orders, and Profits
*/
-- Sales by Country:
Select Country, ROUND(Sum(Sales), 2) AS Total_Sales
From Orders
Group By Country
Order by Total_Sales DESC;

-- Sales by Cities:
Select CC.City, ROUND(SUM(Od.Sales), 2) AS Total_Sales
From Orders Od
Left Join Customers_coffee CC
ON Od.Customer_ID = CC.Customer_ID
Group By cc.City
Order By Total_Sales DESC;

-- Which Country has Most Sales, Orders, and Profits:
With Country_CTE AS (

     Select od.Country, SUM(od.Sales) AS Total_Sales, Count(Distinct od.Order_ID) AS Total_Orders, 
     SUM(od.Quantity) AS Total_Quantity, SUM(Profit) AS Total_Profits
     From Orders od 
     Left Join Products_C pd 
     ON od.Product_ID = pd.Product_ID
     Group By od.Country
), Percent_CTE AS (

     Select Country, ROUND(Total_Sales, 2) AS Total_Sales, Total_Sales/SUM(Total_Sales) Over() *100 AS Percent_Of_Sales, 
     Total_Orders, Total_Quantity, ROUND(Total_Profits, 2) AS Total_Profits,
     CASE WHEN NTILE(3) OVER(ORDER BY Total_Sales DESC) = 1 THEN 'High Sales'
          WHEN NTILE(3) OVER(ORDER BY Total_Sales DESC) = 2 THEN 'Medium Sales'
          ELSE 'Low Sales'
     END AS Change_Sales
     From Country_CTE
)
Select Country, Total_Sales, ROUND(Percent_Of_Sales, 2) AS Percent_Of_Sales, Total_Orders, 
Total_Quantity, Total_Profits, Change_Sales
From Percent_CTE
Order By Total_Sales DESC;
/*
Insights:
* Der Großteil des Umsatzes und auch Profit kommt aus United States (mit Umsatz 35.639,73€ und 1012,74€ Profit)
* Einige Länder tragen nur minimal zum Umsatz bei → Wachstumspotenzial
* Städte wie Washington, Houston, Toledo und New York City sind zentrale Verkaufsstandorte
*/

/************
Time Analysis:
    1) Sales by Years
    2) Sales by Months
    3) YoY-Analysis
    4) MoM-Analysis
*/
-- Sales by Years:
Select year(order_date) AS Yearly_S, ROUND(SUM(Sales), 2) AS Total_Sales
From Orders
Group By year(order_date)
Order By Yearly_S;

-- Sales by Months:
Select Month(order_date) AS Monthly_S, Year(order_date) AS Yearly_S, ROUND(Sum(Sales), 2) AS Total_Sales
From Orders
Group By Year(Order_Date), Month(order_date)
Order By Total_Sales DESC;

-- YoY-Analysis:
With Current_Sales AS (
    Select Year(order_date) AS Yealry_Date, ROUND(Sum(Sales), 2) AS Current_Sales, ROUND(Avg(Sales), 2) AS Avg_Sales
    From Orders
    Group By Year(order_date)
)
Select Yealry_Date, Current_Sales, Avg_Sales,
Current_Sales - Avg(Current_Sales) OVER(Order By Yealry_Date) AS Compare_Sales,
CASE WHEN Current_Sales - Avg(Current_Sales) OVER(Order By Yealry_Date) < 0 THEN 'Below AVG'
     WHEN Current_Sales - Avg(Current_Sales) OVER(Order By Yealry_Date) > 0 THEN 'Over AVG' 
     ELSE 'On AVG'
END AS AVG_Change,
(Current_Sales - LAG(Current_Sales) Over(Order By Yealry_Date)) *100/ LAG(Current_Sales) OVER(Order By Yealry_Date) AS Compare_Previous_Y_S,
CASE WHEN (Current_Sales - LAG(Current_Sales) Over(Order By Yealry_Date)) *100/ LAG(Current_Sales) OVER(Order By Yealry_Date) < 0 THEN 'Decrease'
     WHEN (Current_Sales - LAG(Current_Sales) Over(Order By Yealry_Date)) *100/ LAG(Current_Sales) OVER(Order By Yealry_Date) > 0 THEN 'Increase'
     ELSE 'No Change'
END AS Change_Previous
From Current_Sales;

-- MoM-Analysis:
With Current_Sales AS (
    Select Month(order_date) AS Monthly_Date, ROUND(Sum(Sales), 2) AS Current_Sales, ROUND(Avg(Sales), 2) AS Avg_Sales
    From Orders
    Group By Month(order_date)
)
Select Monthly_Date, Current_Sales, Avg_Sales,
Current_Sales - Avg(Current_Sales) OVER(Order By Monthly_Date) AS Compare_Sales,
CASE WHEN Current_Sales - Avg(Current_Sales) OVER(Order By Monthly_Date) < 0 THEN 'Below AVG'
     WHEN Current_Sales - Avg(Current_Sales) OVER(Order By Monthly_Date) > 0 THEN 'Over AVG' 
     ELSE 'On AVG'
END AS AVG_Change,
Current_Sales - LAG(Current_Sales) Over(Order By Monthly_Date) AS Compare_Previous_Y_S,
CASE WHEN Current_Sales - LAG(Current_Sales) Over(Order By Monthly_Date) < 0 THEN 'Decrease'
     WHEN Current_Sales - LAG(Current_Sales) Over(Order By Monthly_Date) > 0 THEN 'Increase'
     ELSE 'No Change'
END AS Change_Previous
From Current_Sales;
/*
Insights:
* Das Business zeigt ein positives Wachstum über die Zeit (am 2019 ca. 12.187,44€ und am 2021 mehr als 13.766,41€)
* Bestimmte Monaten wie 'Februar, März, Juni, September und Oktober' sind deutlich stärker.
* MoM/YoY Analyse zeigt Phasen von Wachstum und Rückgang
*/

/****************
Customer Insights:
    1) Top 5 Customers
    2) Cumulative Analysis
    3) Has the Card Loyalty any Impact By Sale and Profit
*/
-- Top 5 Customers:
Select Top 5 
Customer_Name, ROUND(SUM(Sales), 2) AS Total_Sales
From Orders
Group By Customer_Name
Order By Total_Sales DESC;

-- Cumulative Analysis:
With Cumulative_Cte As (
    Select SUBSTRING(CAST(order_date AS Varchar(50)), 1, 7) AS Y_M_Date,
    ROUND(Sum(Sales), 2) AS Total_Sales, ROUND(Avg(Sales), 2) AS Avg_Sales
    FROM Orders
    Group By SUBSTRING(CAST(order_date AS Varchar(50)), 1, 7)
), Cumulative_AVG_CTE AS (

     Select Y_M_Date, Total_Sales, SUM(Total_Sales) Over(Order By Y_M_Date) AS Cumulative_SALES,
     Avg_Sales, Avg(Avg_Sales) Over(Order By Y_M_Date) AS Cumulative_AVG
     From Cumulative_Cte
)
Select Y_M_Date, Total_Sales, ROUND(Cumulative_SALES, 2) AS Cumulative_Sales, Avg_Sales, ROUND(Cumulative_AVG, 2) AS Cumulative_Avg 
From Cumulative_AVG_CTE;

-- Has the Card Loyalty any Impact By Sale and Profit:
ALTER TABLE Orders ALTER COLUMN Card_loyalty Varchar(50);

WITH card_CTE AS (

     Select od.Card_loyalty, SUM(od.Sales) AS Total_Sales, COUNT(Distinct od.Order_ID) AS Total_Orders, SUM(od.Quantity) AS Total_Quantity,
     SUM(pd.Profit) AS Total_Profits
     From Orders od
     Left Join Products_C pd
     ON od.Product_ID = pd.Product_ID
     Group By Card_loyalty
)
Select Card_loyalty, ROUND(Total_Sales, 2) AS Total_Sales, Total_Sales/SUM(Total_Sales) OVER() *100 AS Percent_Of_Sales, 
Total_Orders, Total_Quantity, ROUND(Total_Profits, 2) AS Total_Profits
From card_CTE
Order By Total_Sales DESC;
/*
Insights:
* Die Top 5 Kunden generieren einen großen Teil des Umsatzes (wie Allis Wilmore, Brenn Dundredge, Terri Farra, Nealson Cuttler und Don Flintiff)
* Kunden mit Loyalty Card haben eigentlich nicht höheren Average Order Value (46,34% bei Gesamtumsatz). 
  Aber können wir sagen, dass  wir im Zukunft mehr Loyalität Kunden sehen würden.
* Wiederkehrende Kunden sind entscheidend für den Umsatz.
* Beim Cumulative Analysis können wir sagen, dass am Juni-2019, Juni- und November-2020 steigt unsere Umsatz so Höhe ein, 
*/

/******************
Product Performance:
    1) 'Light' Roast Type:
    2) 'Medium' Roast Type:
    3) 'Dark' Roast Type:
    4) Which Coffee_Type makes Higher or Lower Sales/Profits:
    5) Which Roast_Type makes Higher or Lower Sales/Profits:
    6) In Which '5 Months' the Sales is better for each Coffee Type:
    7) Here is Coffee Types Ordered By Months: 
    8) What is the Most Size of Coffee we Selling:
*/
-- 'Light' Roast Type:
With Sales_CTE AS (

     Select Od.Coffe_Type_Name, Od.Roast_Type_Name, ROUND(SUM(Od.Sales), 2) AS Total_Sales, 
     Count(Distinct Od.Order_ID) AS Total_Orders, SUM(Od.Quantity) AS Total_Quantity, SUM(Pd.Profit) AS Total_Profits
     From Orders Od 
     Left Join Products_C Pd 
     ON Od.Product_ID = Pd.Product_ID
     Where Od.Roast_Type_Name = 'Light'
     Group By od.Coffe_Type_Name, od.Roast_Type_Name
), Sales_CTE_2 AS (

     Select Coffe_Type_Name, Roast_Type_Name, Total_Sales, Total_Sales/Sum(Total_Sales) Over() *100 AS Part_To_Whole,
     Total_Orders, Total_Quantity, Total_Profits, Total_Profits/SUM(Total_Profits) OVER() *100 AS Percent_Of_Profit
     From Sales_CTE
)
Select Coffe_Type_Name, Roast_Type_Name, Total_Sales, 
ROUND(Part_To_Whole, 2) AS Percent_Of_Sales, Total_Orders, Total_Quantity, 
ROUND(Total_Profits, 2) AS Total_Profits, ROUND(Percent_Of_Profit, 2) AS Percent_Of_Profit
From Sales_CTE_2
Order By Total_Sales DESC;

-- 'Medium' Roast Type:
With Sales_CTE AS (

     Select Od.Coffe_Type_Name, Od.Roast_Type_Name, ROUND(SUM(Od.Sales), 2) AS Total_Sales, 
     Count(Distinct Od.Order_ID) AS Total_Orders, SUM(Od.Quantity) AS Total_Quantity, SUM(Pd.Profit) AS Total_Profits
     From Orders Od 
     Left Join Products_C Pd 
     ON Od.Product_ID = Pd.Product_ID
     Where Od.Roast_Type_Name = 'Medium'
     Group By od.Coffe_Type_Name, od.Roast_Type_Name
), Sales_CTE_2 AS (

     Select Coffe_Type_Name, Roast_Type_Name, Total_Sales, Total_Sales/Sum(Total_Sales) Over() *100 AS Part_To_Whole,
     Total_Orders, Total_Quantity, Total_Profits, Total_Profits/SUM(Total_Profits) OVER() *100 AS Percent_Of_Profit
     From Sales_CTE
)
Select Coffe_Type_Name, Roast_Type_Name, Total_Sales, 
ROUND(Part_To_Whole, 2) AS Percent_Of_Sales, Total_Orders, Total_Quantity, 
ROUND(Total_Profits, 2) AS Total_Profits, ROUND(Percent_Of_Profit, 2) AS Percent_Of_Profit
From Sales_CTE_2
Order By Percent_Of_Sales DESC;

-- 'Dark' Roast Type:
With Sales_CTE AS (

     Select Od.Coffe_Type_Name, Od.Roast_Type_Name, ROUND(SUM(Od.Sales), 2) AS Total_Sales, 
     Count(Distinct Od.Order_ID) AS Total_Orders, SUM(Od.Quantity) AS Total_Quantity, SUM(Pd.Profit) AS Total_Profits
     From Orders Od 
     RIGHT Join Products_C Pd 
     ON Od.Product_ID = Pd.Product_ID
     Where Od.Roast_Type_Name = 'Dark'
     Group By od.Coffe_Type_Name, od.Roast_Type_Name
), Sales_CTE_2 AS (

     Select Coffe_Type_Name, Roast_Type_Name, Total_Sales, Total_Sales/Sum(Total_Sales) Over() *100 AS Part_To_Whole,
     Total_Orders, Total_Quantity, Total_Profits, Total_Profits/SUM(Total_Profits) OVER() *100 AS Percent_Of_Profit
     From Sales_CTE
)
Select Coffe_Type_Name, Roast_Type_Name, Total_Sales, 
ROUND(Part_To_Whole, 2) AS Percent_Of_Sales, Total_Orders, Total_Quantity, 
ROUND(Total_Profits, 2) AS Total_Profits, ROUND(Percent_Of_Profit, 2) AS Percent_Of_Profit
From Sales_CTE_2
Order By Total_Sales DESC;

-- Which Coffee_Type makes Higher or Lower Sales/Profits:
With Coffee_Type_CTE AS (

     Select od.Coffe_Type_Name, SUM(od.Sales) AS Total_Sales, COUNT(Distinct Order_ID) AS Total_Orders, 
     SUM(Quantity) AS Total_Quantity, SUM(pd.Profit) AS Total_Profits
     From Orders od 
     Left Join Products_C pd 
     ON od.Product_ID = pd.Product_ID
     Group By od.Coffe_Type_Name
), Percent_CTE AS (

     Select Coffe_Type_Name, ROUND(Total_Sales, 2) AS Total_Sales, Total_Orders, Total_Quantity, ROUND(Total_Profits, 2) AS Total_Profits,
     Total_Profits /SUM(Total_Profits) Over() *100 AS Percent_Of_Profit
     From Coffee_Type_CTE
)
Select Coffe_Type_Name, Total_Sales, Total_Orders, Total_Quantity, Total_Profits, 
ROUND(Percent_Of_Profit, 2) AS Percent_Of_Profit, 
CASE WHEN NTILE(3) OVER(ORDER BY Percent_Of_Profit DESC) = 1 THEN 'High Profit'
     WHEN NTILE(3) OVER(ORDER BY Percent_Of_Profit DESC) = 2 THEN 'Medium Profit'
     ELSE 'Low Profit'
END AS Percent_Changes
From Percent_CTE
Order By Percent_Of_Profit DESC;

-- Which Roast_Type makes Higher or Lower Sales/Profits:
With Roast_Type_CTE AS (

     Select od.Roast_Type_Name, SUM(od.Sales) AS Total_Sales, COUNT(Distinct Order_ID) AS Total_Orders, 
     SUM(Quantity) AS Total_Quantity, SUM(pd.Profit) AS Total_Profits
     From Orders od 
     Left Join Products_C pd 
     ON od.Product_ID = pd.Product_ID
     Group By od.Roast_Type_Name
), Percent_CTE AS (

     Select Roast_Type_Name, ROUND(Total_Sales, 2) AS Total_Sales, Total_Orders, Total_Quantity, ROUND(Total_Profits, 2) AS Total_Profits,
     Total_Profits /SUM(Total_Profits) Over() *100 AS Percent_Of_Profit
     From Roast_Type_CTE
)
Select Roast_Type_Name, Total_Sales, Total_Orders, Total_Quantity, Total_Profits, 
ROUND(Percent_Of_Profit, 2) AS Percent_Of_Profit, 
CASE WHEN NTILE(3) OVER(ORDER BY Percent_Of_Profit DESC) = 1 THEN 'High Profit'
     WHEN NTILE(3) OVER(ORDER BY Percent_Of_Profit DESC) = 2 THEN 'Medium Profit'
     ELSE 'Low Profit'
END AS Percent_Changes
From Percent_CTE
Order By Percent_Of_Profit DESC;

-- In Which '5 Months' the Sales is better for each Coffee Type:
Select TOP 5
Coffe_Type_Name, Datename(Month, Order_date) AS Monthly_Date, SUM(Sales) AS Total_Sales, COUNT(Distinct Order_ID) AS Total_Orders,
SUM(Quantity) AS Total_Quantity
From Orders
WHERE Coffe_Type_Name = 'Liberica'
Group By Coffe_Type_Name, Datename(Month, Order_date)
Order By Total_Sales DESC;

Select TOP 5
Coffe_Type_Name, Datename(Month, Order_date) AS Monthly_Date, SUM(Sales) AS Total_Sales, COUNT(Distinct Order_ID) AS Total_Orders,
SUM(Quantity) AS Total_Quantity
From Orders
WHERE Coffe_Type_Name = 'Excelsa'
Group By Coffe_Type_Name, Datename(Month, Order_date)
Order By Total_Sales DESC;

Select TOP 5
Coffe_Type_Name, Datename(Month, Order_date) AS Monthly_Date, SUM(Sales) AS Total_Sales, COUNT(Distinct Order_ID) AS Total_Orders,
SUM(Quantity) AS Total_Quantity
From Orders
WHERE Coffe_Type_Name = 'Arabica'
Group By Coffe_Type_Name, Datename(Month, Order_date)
Order By Total_Sales DESC;

Select TOP 5
Coffe_Type_Name, Datename(Month, Order_date) AS Monthly_Date, SUM(Sales) AS Total_Sales, COUNT(Distinct Order_ID) AS Total_Orders,
SUM(Quantity) AS Total_Quantity
From Orders
WHERE Coffe_Type_Name = 'Robusta'
Group By Coffe_Type_Name, Datename(Month, Order_date)
Order By Total_Sales DESC;

-- Here is Coffee Types Ordered By Months: 
WITH RankedData AS (
    SELECT
        Coffe_Type_Name,
        Month(Order_date) AS Monthly_Date,
        SUM(Sales) AS Total_Sales,
        COUNT(DISTINCT Order_ID) AS Total_Orders,
        SUM(Quantity) AS Total_Quantity,
        ROW_NUMBER() OVER (
            PARTITION BY Coffe_Type_Name
            ORDER BY SUM(Sales) DESC
        ) AS Ranked_Total
    FROM Orders
    WHERE Coffe_Type_Name IN ('Liberica', 'Excelsa', 'Arabica', 'Robusta')
    GROUP BY Coffe_Type_Name, Month(Order_date)
)
SELECT Coffe_Type_Name, Monthly_Date, ROUND(Total_Sales, 2) AS Total_Sales, Total_Orders, Total_Quantity, Ranked_Total
FROM RankedData
WHERE Ranked_Total <= 5
Order By Monthly_Date;

-- What is the Most Size of Coffee we Selling:
WITH Sales_CTE AS (

     Select od.Size, SUM(od.Sales) AS Total_Sales, Count(DISTINCT od.Order_ID) AS Total_Orders, SUM(od.Quantity) AS Total_Quantity,
     SUM(Profit) AS Total_Profits
     From Orders od
     Left Join Products_C pd
     ON od.Product_ID = pd.Product_ID
     GROUP By od.Size
)
Select Size, ROUND(Total_Sales, 2) AS Total_Sales, Total_Sales/SUM(Total_Sales) OVER() *100 AS Percent_Of_Sales,
Total_Orders, Total_Quantity, ROUND(Total_Profits, 2) AS Total_Profits
From Sales_CTE
Order By Total_Sales DESC;
/*
Insights:
* 'Liberica' und 'Excelsa' ist der stärkste Umsatz- und Profit-treiber.
* Bei 'Light' und 'Dark' Roast Types haben die 'Excelsa' und 'Liberica' die Hohe Umsatz und auch Profit.
* Bei 'Medium' Roast Types haben die 'Arabica' und 'Excelsa' die Hohe Umsatz und auch Profit. 
* Im Allgemeinen 'Arabica' und 'Robusta' haben hohe Sales, aber niedrigen Profit → Optimierung nötig
* 'Light Roast Types' performen deutlich besser
* Wir können sagen, dass bei jede Monat (Saisonal), gibt es eine bestimmte Coffee Type, die wir mehr als andere verkaufen können:
Januar --> Liberica 
Februar --> Arabica 
März --> Liberica / Excelsa / Robusta
April --> Liberica / Excelsa 
Mai -->
Juni --> Arabica / Excelsa / Robusta
Juli --> Arabica / Excelsa / Robusta
August --> Robusta
September --> Arabica / Robusta
Oktober --> Liberica 
November --> Liberica / Arabica 
Dezember --> Excelsa
* Die Meisten Verkaufte und Profitable Size Of Coffee ist 2,5 Kg und 1,0 Kg
*/

/*
-- Recommendations --
     * Fokus auf High-Profit Produkte
     * Ausbau der Top Märkte
     * Optimierung von Low-Margin Produkten
     * Stärkung des Loyalty Programms
     * Nutzung saisonaler Trends für Marketing
*/