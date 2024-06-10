# SQL-Northwind-Sales-Data-

# Project Title

### Northwind Sales Data Analysis

#### Introduction

This document presents the findings from the Northwind Sales dataset. The analysis aims to provide insights into sales performance, customer behavior, product popularity, employee effectiveness, and more.

#### Data Overview

The dataset contains information on orders, customers, products, and employees. Key columns include:

orderID, 

customerID, 

employeeID

orderDate, 

requiredDate,

shippedDate

productID, 

unitPrice, 

quantity

unitsInStock,

unitsOnOrder,

reorderLevel

categoryName, 

suppliers.companyName**

### Total Revenue, Total Quantity of Goods Sold, Total Number of Orders
    SELECT
    SUM(unitPrice * quantity) AS total_revenue,
    SUM(quantity) AS total_quantity_sold,
    COUNT(DISTINCT orderID) AS total_orders
    FROM
    NorthwindSales;


### Most Sold Products (Top 5) and Least Sold Products:
#### Most Sold Products (Top 5) and Least Sold Products

    SELECT
    productID,
    SUM(quantity) AS total_quantity_sold
    FROM
    NorthwindSales
    GROUP BY
    productID
    ORDER BY
    total_quantity_sold DESC
    LIMIT 5


#### Number of Discontinued Products and Active Products.
    SELECT
    productID,
    SUM(quantity) AS total_quantity_sold
    FROM
    NorthwindSales
    GROUP BY
    productID
    ORDER BY
    total_quantity_sold ASC
    LIMIT 5;

### Number of Customers, Suppliers, Employees:

    -- Number of Customers
    SELECT
    COUNT(DISTINCT customerID) AS total_customers
    FROM
    NorthwindSales;

-- Number of Suppliers

    SELECT
    COUNT(DISTINCT suppliers.companyName) AS   
    total_suppliers
    FROM
    NorthwindSales;

-- Number of Employees

    SELECT
    COUNT(DISTINCT employeeID) AS total_employees
    FROM
    NorthwindSales;
 

##### Employee Performance 

    SELECT 
    EmployeeID,COUNT(OrderID) AS TotalOrders,
    SUM(UnitPrice * Quantity) AS TotalSales
    FROM 
    Orders
    JOIN 
    Order_Details ON Orders.OrderID =              Order_Details.OrderID
    GROUP BY 
    EmployeeID
    ORDER BY 
    TotalSales DESC
    LIMIT 10;

    
### Employee Performance

    SELECT 
    EmployeeID, 
    COUNT(OrderID) AS TotalOrders,
    SUM(UnitPrice * Quantity) AS TotalSales
    FROM
    Orders
    JOIN 
    Order_Details ON Orders.OrderID =           Order_Details.OrderID
    GROUP BY 
    EmployeeID
    ORDER BY 
    TotalSales DESC
    LIMIT 10;

### Top 5 Customers Generating the Most Revenue

    SELECT
    customerID,
    SUM(unitPrice * quantity) AS total_revenue
    FROM
    NorthwindSales
    GROUP BY
    customerID
    ORDER BY
    total_revenue DESC


### Most 10 and Least 10 Product Units in Stock
    SELECT
    productID,
    unitsInStock
    FROM
    NorthwindSales
    ORDER BY
    unitsInStock DESC
    FETCH FIRST 10 ROWS ONLY;
-- Least 10 Products in Stock

    SELECT
    productID,
    unitsInStock
    FROM
    NorthwindSales
    ORDER BY
    unitsInStock ASC
    LIMIT


### Top 5 Most Sold Product Categories (Quantity)

    WITH CategorySales AS (
    SELECT
    categoryName,
    SUM(quantity) AS total_quantity_sold
    FROM
    NorthwindSales
    GROUP BY
    categoryName
    )
    SELECT
    categoryName,
    total_quantity_sold
    FROM
    CategorySales
    ORDER BY
    total_quantity_sold DESC
    LIMIT 5


### Number of Employees by Employee Title

    SELECT
    suppliers.contactTitle AS employee_title,
    COUNT(DISTINCT employeeID) AS number_of_employees
    FROM
    NorthwindSales
    GROUP BY
    suppliers.contactTitle;

### Calculate MoM% Revenue, YoY% Revenue
#### -- MoM% Revenue Growth
    SELECT
    TO_CHAR(orderDate, 'YYYY-MM') AS order_month,
    SUM(unitPrice * quantity) AS total_revenue,
    LAG(SUM(unitPrice * quantity)) OVER (ORDER BY   TO_CHAR(orderDate, 'YYYY-MM')) AS       previous_month_revenue,
    (SUM(unitPrice * quantity) - LAG(SUM(unitPrice * quantity)) OVER (ORDER BY TO_CHAR(orderDate, 'YYYY-MM'))) /
    LAG(SUM(unitPrice * quantity)) OVER (ORDER BY TO_CHAR(orderDate, 'YYYY-MM')) * 100 AS mom_growth_percentage
    FROM
    NorthwindSales
    GROUP BY
    TO_CHAR(orderDate, 'YYYY-MM');

-- YoY% Revenue Growth

    SELECT
    TO_CHAR(orderDate, 'YYYY') AS order_year,
    SUM(unitPrice * quantity) AS total_revenue,
    LAG(SUM(unitPrice * quantity)) OVER (ORDER BY   TO_CHAR(orderDate, 'YYYY')) AS          previous_year_revenue,
    (SUM(unitPrice * quantity) - LAG(SUM(unitPrice * quantity)) OVER (ORDER BY TO_CHAR(orderDate, 'YYYY'))) /
    LAG(SUM(unitPrice * quantity)) OVER (ORDER BY TO_CHAR(orderDate, 'YYYY')) * 100 AS yoy_growth_percentage
    FROM
    NorthwindSales
    GROUP BY
    TO_CHAR(orderDate, 'YYYY');

### Number of Customers Ordering Last Year vs. This Year
-- YoY% Revenue Growth

    -- Customers Ordering Last Year
    SELECT
    COUNT(DISTINCT customerID) AS customers_last_year
    FROM
    NorthwindSales
    WHERE
    TO_CHAR(orderDate, 'YYYY') = TO_CHAR(SYSDATE, 'YYYY') - 1;

-- Customers Ordering This Year

    SELECT
    COUNT(DISTINCT customerID) AS customers_this_year
    FROM
    NorthwindSales
    WHERE
    TO_CHAR(orderDate, 'YYYY') = TO_CHAR(SYSDATE,   'YYYY');

### Average Days Between Order Date and Shipping Date.
    SELECT
    AVG(shippedDate - orderDate) AS     avg_days_to_ship
    FROM
    NorthwindSales;

### Distribution of Number of Orders by Order Month
    SELECT
    order_month,
    number_of_orders
    FROM (
    SELECT
    TO_CHAR(orderDate, 'YYYY-MM') AS        order_month,
    COUNT(orderID) AS number_of_orders
    FROM
    NorthwindSales
    GROUP BY
    TO_CHAR(orderDate, 'YYYY-MM')
    )  
    ORDER BY
    order_month;

### Key Findings

#### Sales Performance
Total Revenue: Calculated as the sum of all product sales (unitPrice * quantity).

Total Quantity Sold: Sum of the quantity column.

Total Number of Orders: Count of unique orderIDs.

Top and Bottom Products

Top 5 Most Sold Products: Products with the highest total quantity sold.

Least Sold Products: Products with the lowest total quantity sold.

Product Availability

Discontinued Products: Count and list of products marked as discontinued.

Active Products: Count and list of products still available for sale.

Customer and Supplier Analysis
Number of Customers: Count of unique customerIDs.
Number of Suppliers: Count of unique suppliers.companyNames.

Top 5 Customers: Customers generating the most revenue.

#### Inventory Analysis.
Top 10 Products in Stock: Products with the highest unitsInStock.

Least 10 Products in Stock: Products with the lowest unitsInStock.

Top 5 Product Categories: Categories with the highest total quantity sold.

Employee Performance
Number of Employees: Count of unique employeeIDs.
Employee Performance: Sales performance by employee, based on the total revenue generated.

#### Time Intelligence Insights.
MoM% Revenue Growth: Month-over-Month percentage growth in revenue.
YoY% Revenue Growth: Year-over-year percentage growth in revenue.
Customer Retention: Difference in the number of customers who ordered last year vs. this year.
Order Processing Time: Average days between orderDate and shippedDate.
Monthly Order Distribution: Number of orders per month.

#### Recommendations
Promote Top-Selling Products: Focus marketing efforts on products with the highest sales to maximize revenue.
Inventory Management: Adjust inventory levels based on the analysis of top and bottom products in stock to reduce holding costs and avoid stockouts.
Customer Segmentation: Develop targeted marketing strategies for top-revenue-generating customers.
Employee Training: Identify lower-performance employees and provide targeted training to improve their sales skills.
Pricing Strategy: Review and adjust pricing strategies to ensure competitiveness and profitability.
Improve Shipping Efficiency: Analyze and optimize the order processing time to enhance customer satisfaction.
Seasonal Promotions: Use monthly order distribution insights to plan seasonal promotions and campaigns.

#### Limitations
Data Quality: The analysis is based on the provided dataset. Any inaccuracies or missing data could affect the insights.
Scope: The dataset may not cover all aspects of the business, such as marketing expenses or detailed customer demographics.
Assumptions: Certain assumptions were made during the analysis, I may not fully capture the complexity of the business operations.
Conclusion
The Northwind Sales dataset provides valuable insights into sales performance, customer behavior, product popularity, and employee effectiveness. By leveraging these insights and implementing the recommended strategies, Northwind Traders can optimize their operations, enhance customer satisfaction, and drive revenue growth.
