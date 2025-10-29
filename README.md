# 🛍️ Online Retail Data Analysis using SQL

---

## 📘 **Project Summary**
Analyzed **Online Retail data** consisting of *Customers, Products, Categories, Orders,* and *Order Items* using **SQL** to uncover:
- Sales trends  
- Top-performing products  
- Customer behavior  
- Regional and category performance  

This project applies SQL-based **data modeling, joins, KPIs, and analytics** to enable **data-driven decision-making** for retail businesses.

---

## ❓ **Problem Statement**
Retailers often struggle to identify:
- Top-selling products and categories  
- High-value or repeat customers  
- Seasonal sales trends  

These insights are essential for **marketing**, **inventory planning**, and **profit optimization**.

---

## 💡 **Solution**
Using SQL, data was extracted, cleaned, and analyzed to:
- Identify best-sellers by category  
- Find repeat and high-spending customers  
- Calculate KPIs (Revenue, Avg Order Value, Repeat Buyer Rate)  
- Detect stock-out products and recent order activity  

---

## 🧩 **Database Schema**
The project uses a **normalized relational database** with the following tables:

| Table | Description |
|--------|--------------|
| `Customers` | Stores customer details like name, contact, and country |
| `Products` | Contains product details, price, and stock |
| `Categories` | Product categories (Electronics, Clothing, Books, etc.) |
| `Orders` | Records customer orders and order amount |
| `Order_Items` | Links orders to products with quantity and price |

---

## 🧱 **SQL Table Creation**

### 🔹 Customers Table
```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY IDENTITY(1,1),
    FirstName NVARCHAR(50),
    LastName NVARCHAR(50),
    Email NVARCHAR(100),
    Phone VARCHAR(50),
    Address NVARCHAR(225),
    City NVARCHAR(50),
    State NVARCHAR(50),
    Zipcode NVARCHAR(50),
    Country NVARCHAR(50),
    CreatedAt DATETIME DEFAULT GETDATE()
);
```
🔹 Products Table
```sql
CREATE TABLE Products (
    ProductID INT PRIMARY KEY IDENTITY(1,1),
    ProductName NVARCHAR(100),
    CategoryID INT,
    Price DECIMAL(10,2),
    Stock INT,
    CreatedAt DATETIME DEFAULT GETDATE()
);
```
🔹 Categories Table
```sql
CREATE TABLE Categories (
    CategoryID INT PRIMARY KEY IDENTITY(1,1),
    CategoryName NVARCHAR(100),
    Description NVARCHAR(225)
);
```
🔹 Orders Table
```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY IDENTITY(1,1),
    CustomerID INT,
    TotalAmount DECIMAL(10,2),
    OrderDate DATETIME DEFAULT GETDATE(),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```
🔹 Order_Items Table
```sql
CREATE TABLE Order_Items (
    OrderItemID INT PRIMARY KEY IDENTITY(1,1),
    OrderID INT,
    ProductID INT,
    Quantity INT,
    Price DECIMAL(10,2),
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
);
```
⚙️ Data Analysis Using SQL
1️⃣ Retrieve all orders for a specific customer
```sql
SELECT o.OrderID, o.OrderDate, o.TotalAmount, p.ProductName, oi.Quantity, oi.Price
FROM Orders o
JOIN Order_Items oi ON o.OrderID = oi.OrderID
JOIN Products p ON oi.ProductID = p.ProductID
WHERE o.CustomerID = 1;
```

2️⃣ Total sales for each product
```sql
SELECT p.ProductName, SUM(oi.Quantity * oi.Price) AS TotalSales
FROM Order_Items oi
JOIN Products p ON p.ProductID = oi.ProductID
GROUP BY p.ProductName;
```

3️⃣ Average order value
```sql
SELECT AVG(TotalAmount) AS AverageOrderValue FROM Orders;
```

4️⃣ Top 3 customers by spending
```sql
SELECT TOP 3 c.FirstName, c.LastName, SUM(o.TotalAmount) AS TotalSpending
FROM Customers c
JOIN Orders o ON o.CustomerID = c.CustomerID
GROUP BY c.FirstName, c.LastName
ORDER BY TotalSpending DESC;
```

5️⃣ Most popular product category
```sql
SELECT TOP 1 c.CategoryName, SUM(oi.Quantity) AS TotalQuantitySold
FROM Order_Items oi
JOIN Products p ON p.ProductID = oi.ProductID
JOIN Categories c ON c.CategoryID = p.CategoryID
GROUP BY c.CategoryName
ORDER BY TotalQuantitySold DESC;
```

6️⃣ Out-of-stock products
```sql
SELECT ProductID, ProductName, Stock
FROM Products
WHERE Stock = 0;
```

7️⃣ Recent orders (last 30 days)
```sql
SELECT c.FirstName, c.LastName, o.OrderDate
FROM Customers c
JOIN Orders o ON o.CustomerID = c.CustomerID
WHERE o.OrderDate >= DATEADD(DAY, -30, GETDATE());
```

8️⃣ Monthly order count
```sql
SELECT YEAR(OrderDate) AS Year, MONTH(OrderDate) AS Month, COUNT(OrderID) AS TotalOrders
FROM Orders
GROUP BY YEAR(OrderDate), MONTH(OrderDate);
```


🧾 Key Insights

- Electronics and Clothing were top-performing categories.
- Customers Sameer and Jane showed the highest total spending.
- USA had the most active customers.
- Product Keyboard was identified as out of stock.
- March and April recorded peak order volumes.
  

🏁 Results & Business Impact
- Improved inventory management and stock planning.
- Identified high-value customers for loyalty programs.
- Optimized marketing targeting and product strategy.
- Enhanced decision-making using SQL-driven KPIs.

🔮 Future Enhancements
- Integrate Power BI for visualization of SQL insights.
- Add Python-based forecasting models for sales prediction.
- Automate daily ETL pipelines for data refresh.


👤 Author & Contact

Kailas Kakde
📊 Data Analyst | SQL | Power BI | Python | Excel
📧 Email: kakdekailas0@gmail.com
🌐 LinkedIn: linkedin.com/in/kailas-kakde-b62ab2289
