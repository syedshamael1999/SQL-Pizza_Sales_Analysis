# 🍕 SQL Queries for Pizza Sales Analysis  

## 📌 Overview  
This file contains SQL queries used to analyze **pizza sales data**, covering key performance metrics, sales trends, and best-selling products. These queries were executed using **MS SQL Server** and help derive **actionable insights** for business decision-making.  

## 📊 Key Performance Indicators (KPIs)  

### Total Revenue
```sql
SELECT SUM(total_price) AS Total_Price
FROM pizza_sales
```
### Avg Order Value
```sql
SELECT SUM(total_price) / COUNT(DISTINCT order_id) AS Avg_Order_Value
FROM pizza_sales
```

### Total Pizzas Sold 
```sql
SELECT SUM(quantity) AS Total_Pizzas_Sold
FROM pizza_sales
```

### Total Orders 
```sql
SELECT COUNT(DISTINCT order_id) AS Total_orders
FROM pizza_sales
```

### Avg Pizzas Sold per Order 
```sql
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) /
CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2))
AS Avg_Pizzas_per_Order
FROM pizza_sales
```

## 📈 Trends

### Hourly Trend for total orders
```sql
--DATEPART() to extract hour from order_time
SELECT DATEPART(HOUR, order_time) AS Order_hours, COUNT(DISTINCT order_id) 
AS Total_Orders
FROM pizza_sales
GROUP BY DATEPART(HOUR, order_time)
ORDER BY DATEPART(HOUR, order_time)
```

### Weekly Trend for Total Orders
```sql
SELECT DATEPART(ISO_WEEK, order_date) AS Week_Number, YEAR(order_date) AS Order_Year,
COUNT(DISTINCT order_id) AS total_orders
FROM pizza_sales
GROUP BY DATEPART(ISO_WEEK, order_date), YEAR(order_date)
ORDER BY DATEPART(ISO_WEEK, order_date), YEAR(order_date)
```

### Percentage of Sales by Pizza category
```sql
SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) AS Total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales)
AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_category
```

### Percentage of Sales by Pizza Size
```sql
SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) AS Total_Revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) FROM pizza_sales) 
AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_size
ORDER BY pizza_size
```

### Top 5 Best Sellers by Revenue
```sql
SELECT TOP 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue DESC
```

### Bottom 5 Best Sellers by Revenue
```sql
SELECT TOP 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue ASC
```

### Top 5 Best Sellers by Total Quantity 
```sql
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Quantity
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Quantity DESC
```

### Bottom 5 Best Sellers by Total Quantity 
```sql
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Quantity
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Quantity ASC
```

### Top 5 Best Sellers by Total Orders
```sql
SELECT TOP 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders DESC
```

### Bottom 5 Best Sellers by Total Orders
```sql
SELECT TOP 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders ASC
```

📊 **[Read Analysis Report](Pizza_Sales_Analysis_Report.md)** | 📓 **[View SQL Notebook](Pizza_Sales_Analysis.ipynb)** 
