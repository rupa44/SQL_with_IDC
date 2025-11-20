# 🍕 The Great Pizza Analytics Challenge - IDC Pizza Mini Project

## Project Overview
This project serves as a comprehensive practice of foundational and intermediate SQL concepts, covering topics from database inspection to complex joins and aggregations. The goal is to analyze raw sales data for **IDC Pizza** and generate actionable insights.

---

## 💾 Phase 1: Foundation & Inspection

These queries focus on inspecting the database structure and cleaning preliminary data.

### 1. Unique Pizza Categories
```sql
SELECT DISTINCT category 
FROM pizza_types;
````

### 2\. Handling Missing Ingredients

*Displays `pizza_type_id`, `name`, and ingredients, replacing `NULL` ingredients with "Missing Data".*

```sql
SELECT 
    pizza_type_id,
    name,
    COALESCE(ingredients, 'Missing Data') AS ingredients 
FROM pizza_types 
LIMIT 5;
```

### 3\. Check for Pizzas Missing a Price

```sql
SELECT * FROM pizzas 
WHERE price IS NULL;
```

-----

## 🔍 Phase 2: Filtering & Exploration


### 1\. Orders Placed on '2015-01-01'

```sql
SELECT * FROM orders 
WHERE date = '2015-01-01';
```

### 2\. Pizzas Listed by Price (Descending)

```sql
SELECT 
    pt.name AS pizza_name,
    p.price 
FROM pizzas p 
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id 
ORDER BY p.price DESC;
```

### 3\. Pizzas Sold in Sizes 'L' or 'XL'

```sql
SELECT 
    pt.name AS pizza_name,
    p.size 
FROM pizzas p 
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id 
WHERE p.size IN ('L', 'XL');
```

### 4\. Pizzas Priced Between $15.00 and $17.00

```sql
SELECT 
    pt.name AS pizza_name,
    p.price AS price 
FROM pizzas p 
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id 
WHERE p.price BETWEEN 15.00 AND 17.00;
```

### 5\. Pizzas with "Chicken" in the Name

```sql
SELECT name 
FROM pizza_types 
WHERE name ILIKE '%chicken%';
```

### 6\. Orders on '2015-02-15' or Placed After 8 PM

```sql
SELECT * FROM orders 
WHERE date = '2015-02-15' OR time >= '20:00:00';
```

-----

## 📊 Phase 3: Sales Performance & Advanced Analysis


### 1\. Total Quantity of Pizzas Sold

```sql
SELECT SUM(quantity) AS total_pizzas_sold 
FROM order_details;
```

### 2\. Average Pizza Price

```sql
SELECT ROUND(AVG(price), 2) AS average_pizza_price 
FROM pizzas;
```

### 3\. Total Order Value Per Order

```sql
SELECT 
    od.order_id,
    SUM(od.quantity * p.price) AS order_value 
FROM order_details od 
JOIN pizzas p ON od.pizza_id = p.pizza_id 
GROUP BY od.order_id 
ORDER BY od.order_id;
```

### 4\. Total Quantity Sold Per Pizza Category

```sql
SELECT 
    pt.category,
    SUM(od.quantity) AS quantity_sold 
FROM order_details od 
JOIN pizzas p ON od.pizza_id = p.pizza_id 
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id 
GROUP BY pt.category 
ORDER BY quantity_sold DESC;
```

### 5\. Categories with More Than 5,000 Pizzas Sold

```sql
SELECT 
    pt.category,
    SUM(od.quantity) AS total_quantity_sold 
FROM order_details od 
JOIN pizzas p ON od.pizza_id = p.pizza_id 
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id 
GROUP BY pt.category 
HAVING SUM(od.quantity) > 5000;
```

### 6\. Pizzas Never Ordered (Inventory Check)

*Uses a `LEFT JOIN` to identify pizzas without a corresponding entry in `order_details`.*

```sql
SELECT 
    p.pizza_id, 
    p.pizza_type_id 
FROM pizzas p 
LEFT JOIN order_details od ON p.pizza_id = od.pizza_id 
WHERE od.pizza_id IS NULL;
```

### 7\. Price Differences Between Different Sizes of the Same Pizza

*Uses a `SELF JOIN` on the `pizzas` table to compare prices.*

```sql
SELECT 
    pt.name,
    p1.size,
    p1.price,
    p2.size,
    p2.price,
    (p2.price - p1.price) AS price_diff 
FROM pizzas p1 
JOIN pizzas p2 ON p1.pizza_type_id = p2.pizza_type_id 
JOIN pizza_types pt ON p1.pizza_type_id = pt.pizza_type_id 
WHERE p1.size < p2.size;
```

```
```
## 🙌 Acknowledgement
Thanks to **@DPDzero** and **@indiandataclub** for the amazing #SQLWithIDC challenge!
