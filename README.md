# AdventureWorks SQL Queries: Extracting Insights for Informed Decision-Making to Improve Business Performance
      
![image](https://github.com/TianaFash/Descriptive-Exploratory-and-Predictive-Analysis/assets/166267039/6c75dc8f-672c-45f0-af31-fd6149684841)

## Introduction
AdventureWorks is a comprehensive database encompassing various aspects of a fictional company's operations, providing a rich dataset for analysis and decision-making. In this project, I analyzed a series of business queries to extract meaningful insights and facilitate informed decision-making to improve business growth with the use of SQL. Each query presents a unique scenario, prompting us to employ SQL queries to retrieve relevant data from the AdventureWorks database:

## Query 1: Optimizing Product Selection and Pricing Strategy
The first query focuses on product selection and pricing strategy. By retrieving information about products with specific color values and price ranges, we aim to optimize product offerings and pricing. Our recommendation includes filtering out products with undesirable colors such as red, silver/black, and white, while also considering the price range between £75 and £750 to ensure competitiveness. Additionally, renaming the 'StandardCost' column to 'Price' enhances clarity in the dataset. Sorting the results by list price in descending order enables better visualization of product pricing dynamics, aiding in strategic decision-making regarding product promotion and pricing adjustments:

```SQL for Product Selection and Pricing Strategy.
SELECT ProductID, Name, Colour, ListPrice AS Price
FROM Products
WHERE Colour IS NOT NULL
    AND Colour NOT IN ('red', 'silver/black', 'white')
    AND ListPrice BETWEEN 75 AND 750
ORDER BY ListPrice DESC;
```
## Query 2: Employee Recruitment and Diversity
Afterwards, I analyzed employee demographics and recruitment trends by identifying male employees born between 1962 to 1970 and female employees born between 1972 and 1975, along with specific hire date criteria between 2001 and 2002. I aimed to gain insights into workforce diversity and recruitment patterns. My recommendation involves refining recruitment strategies to target specific demographic segments, ensuring a diverse and inclusive workforce:

``` SQL Query for Employee Recruitment and Diversity
SELECT *
BusinessEntityID, 
    BirthDate, 
    Gender, 
    HireDate
FROM
HumanResources.Employee
WHERE (Gender = 'M' AND BirthDate BETWEEN '1962-01-01' AND '1970-12-31' AND HireDate > '2001')
   OR (Gender = 'F' AND BirthDate BETWEEN '1972-01-01' AND '1975-12-31' AND HireDate BETWEEN '2001-01-01' AND '2002-12-31');
```

## Query 3: Prioritizing High-Value Products
Creating a list of the ten most expensive products with specific product numbers enables businesses to prioritize high-value product categories. By focusing marketing efforts on these products, businesses can capitalize on their profitability and enhance customer satisfaction. This targeted approach helps businesses allocate resources effectively and maximize return on investment: 

``` SQL Query for Prioritizing High-Value Products
* SELECT TOP (10) 
    ProductID, 
    [Name], 
    Color
FROM 
    Production.Product
WHERE 
    ProductNumber LIKE 'BK%'
ORDER BY 
    ListPrice DESC;
  ```
## Query 4: Enhancing Customer Relationship Management
Identifying contact persons whose last names match the first four characters of their email addresses streamlines customer relationship management. Creating a new column combining first and last names for contacts with similar initials enhances personalized communication. Leveraging this information strengthens customer relationships and fosters loyalty, ultimately driving business growth.

``` SQL Query for Enhancing Customer Relationship Management
Select 
pp.BusinessEntityID, FirstName, LastName, CONCAT (FirstName,' ', LastName)
AS 'FULL NAME', Len(CONCAT (FirstName,' ',LastName)) AS 'LENGTH', pe.EmailAddress
From 
Person.Person pp 
Join Person.EmailAddress pe on pp.BusinessEntityID = pe.BusinessEntityID
where 
SUBSTRING (pp.LastName, 1, 4) = substring(pe.EmailAddress, 1, 4);
--1,072 rows returned
```

## Query 5: Streamlining Manufacturing Processes
Identifying product subcategories with longer manufacturing lead times enables businesses to streamline production processes. By optimizing manufacturing efficiency for these products, businesses can reduce costs and improve overall operational performance. This data-driven approach enhances competitiveness and ensures timely delivery of products to customers: 

``` SQL Query for Streamlining Manufacturing Processes
SELECT 
    pp.ProductSubcategoryID, 
    ps.[Name], 
    DaysToManufacture
FROM 
    Production.Product pp
LEFT JOIN 
    Production.ProductSubcategory ps ON pp.ProductSubcategoryID = ps.ProductSubcategoryID
WHERE 
    DaysToManufacture >= 3;
--97 rows returned
```

## Query 6: Tailoring Product Segmentation Strategies
Segmenting products based on price ranges and colors enables businesses to tailor marketing strategies to target specific market segments effectively. By categorizing products into predefined segments, businesses can customize promotional efforts and optimize sales strategies. This approach enhances customer engagement and drives sales growth:

``` SQL Query for Tailoring Product Segmentation Strategies
SELECT 
    ProductID, 
    [Name], 
    ListPrice, 
    Color,
    CASE 
        WHEN ListPrice < 200 THEN 'Low value'
        WHEN ListPrice BETWEEN 201 AND 750 THEN 'Mid value'
        WHEN ListPrice BETWEEN 751 AND 1250 THEN 'Mid to high value'
        ELSE 'Higher value'
```

## Query 7: Optimizing Organizational Structure
Analyzing distinct job titles within the organization helps streamline organizational structure and hierarchy. By consolidating job titles and roles, businesses can improve clarity and efficiency in internal processes. This optimization enhances communication and collaboration among employees, driving organizational success.

``` SQL Query for Optimizing Organizational Structures
SELECT DISTINCT 
    JobTitle
FROM 
    HumanResources.Employee;
--There are 67 distinct job titles in the Employee table
``` 

## Query 8: Understanding Employee Demographics
Calculating employee ages at the time of hiring provides insights into workforce demographics and recruitment trends. By understanding the age distribution of employees, businesses can develop targeted retention strategies and succession plans. This proactive approach ensures a stable and productive workforce for the long term:

``` SQL Query for Understanding Employee Demographics
SELECT 
    BusinessEntityID, 
    DATEDIFF(year, BirthDate, HireDate) AS AgeAtHiring
FROM 
    HumanResources.Employee;
``` 

## Query 9: Recognizing Employee Longevity
Identifying employees eligible for long service awards in the next 5 years enables businesses to recognize and reward loyalty and dedication. By acknowledging employees' contributions, businesses can enhance employee morale and retention rates. This fosters a positive work environment and strengthens employee engagement, ultimately driving organizational success:

``` SQL Query for  Recognizing Employee Longevity
SELECT 
    BusinessEntityID, 
    CURRENT_TIMESTAMP AS Today, 
    DATEDIFF(year, HireDate, CURRENT_TIMESTAMP) AS YearsOfBeingEmployed
FROM 
    HumanResources.Employee
WHERE 
    DATEDIFF(year, HireDate, CURRENT_TIMESTAMP) = 15;

--As of today, 148 employees will be due a long service award in the next 5 years
``` 

## Query 10: Retirement Planning for Employees
Calculating the remaining years until retirement helps employees plan for their future, while also allowing businesses to anticipate workforce changes. By providing retirement planning resources and support, businesses can help employees transition smoothly into retirement. This proactive approach demonstrates a commitment to employee well-being and fosters loyalty among staff members:

``` SQL Query for  Recognizing Employee Longevity
SELECT 
    BusinessEntityID, 
    DATEDIFF(year, BirthDate, CURRENT_TIMESTAMP) AS Age, 
    65 - (DATEDIFF(year, BirthDate, CURRENT_TIMESTAMP)) AS NoOfYearsUntil65
FROM 
    HumanResources.Employee
ORDER BY 
    BusinessEntityID;
``` 

# Query 11: Implementing Pricing Policies
Implementing a new pricing policy based on product color enables businesses to optimize pricing strategies and maximize profitability. By adjusting prices based on color categories, businesses can appeal to customer preferences and market trends. This data-driven pricing approach enhances competitiveness and ensures optimal pricing across product lines:

``` SQL Query for  Recognizing Employee Longevity
SELECT 
    ProductID,
    Name,
    Color,
    ListPrice AS OriginalPrice,
    CASE 
        WHEN Color = 'white' THEN ROUND(ListPrice * 1.08, 2)
        WHEN Color = 'yellow' THEN ROUND(ListPrice * 0.925, 2)
        WHEN Color = 'black' THEN ROUND(ListPrice * 1.172, 2)
        WHEN Color IN ('multi', 'silver', 'silver/black', 'blue') THEN ROUND(SQRT(ListPrice) * 2, 2)
        ELSE ListPrice
    END AS NewPrice,
    ROUND(CASE 
              WHEN Color = 'white' THEN ListPrice * 0.375 * 1.08
              WHEN Color = 'yellow' THEN ListPrice * 0.375 * 0.925
              WHEN Color = 'black' THEN ListPrice * 0.375 * 1.172
              WHEN Color IN ('multi', 'silver', 'silver/black', 'blue') THEN (SQRT(ListPrice) * 2) * 0.375
              ELSE ListPrice * 0.375
          END, 2) AS Commission
FROM 
    Production.Product;
```


## Query 12: Evaluating Salesperson Performance
Analyzing salesperson performance and sales quotas provides insights into sales effectiveness and revenue generation. By identifying top-performing salespeople, businesses can allocate resources effectively and incentivize high performance. This data-driven approach enhances sales team productivity and drives revenue growth:

``` SQL Query for Evaluating Salesperson Performance
SELECT 
    sp.BusinessEntityID, 
    pp.FirstName, 
    pp.LastName, 
    he.HireDate, 
    he.SickLeaveHours, 
    sp.SalesQuota, 
    cr.[Name] AS Region
FROM 
    Sales.SalesPerson sp 
JOIN 
    HumanResources.Employee he ON sp.BusinessEntityID = he.BusinessEntityID 
JOIN 
    Person.Person pp ON he.BusinessEntityID = pp.BusinessEntityID 
LEFT JOIN 
    Sales.SalesTerritory st ON st.TerritoryID = sp.TerritoryID
LEFT JOIN 
    Person.CountryRegion cr ON st.CountryRegionCode = cr.CountryRegionCode;

--17 rows returned
``` 

## Query 13: Analyzing Sales Transactions
Analyzing sales transactions helps businesses understand product performance, customer preferences, and regional sales trends. By extracting key information such as product names, salesperson details, and transaction dates, businesses can identify opportunities for growth and optimization. This data-driven analysis guides strategic decision-making processes and marketing strategies:

``` SQL Query fo Analyzing Sales Transactions
SELECT 
    p.Name AS ProductName,
    pc.Name AS ProductCategoryName,
    psc.Name AS ProductSubcategoryName,
    CONCAT(per.FirstName, ' ', per.LastName) AS SalesPerson,
    sod.LineTotal AS Revenue,
    MONTH(soh.OrderDate) AS MonthOfTransaction,
    DATEPART(QUARTER, soh.OrderDate) AS QuarterOfTransaction,
    cr.[Name] AS Region
FROM 
    Sales.SalesOrderDetail sod
LEFT JOIN 
    Sales.SalesOrderHeader soh ON soh.SalesOrderID = sod.SalesOrderID
LEFT JOIN 
    Production.Product p ON sod.ProductID = p.ProductID
LEFT JOIN 
    Production.ProductSubcategory psc ON p.ProductSubcategoryID = psc.ProductSubcategoryID
LEFT JOIN 
    Production.ProductCategory pc ON psc.ProductCategoryID = pc.ProductCategoryID
LEFT JOIN 
    Sales.SalesPerson sp ON soh.SalesPersonID = sp.BusinessEntityID
LEFT JOIN 
    Sales.SalesTerritory st ON sp.TerritoryID = st.TerritoryID
LEFT JOIN 
    Person.Person per ON per.BusinessEntityID = sp.BusinessEntityID
LEFT JOIN 
    Person.CountryRegion cr ON st.CountryRegionCode = cr.CountryRegionCode;

--121,317 rows returned consistent with the first table, SalesOrderDetail
```
 
## Query 14: Understanding Order Details
Examining order details provides insights into customer behaviour, salesperson effectiveness, and commission earnings. By analyzing order numbers, dates, amounts, and customer-seller relationships, businesses can optimize order fulfilment processes and sales performance. This data-driven approach enhances customer satisfaction, and drives repeat business:

``` SQL Query for Analyzing Sales Transactions
SELECT 
    soh.SalesOrderNumber AS OrderNumber,
    soh.OrderDate AS OrderDate,
    ROUND(soh.SubTotal, 2) AS AmountOfOrder,
    CONCAT(c.FirstName, ' ', c.LastName) AS CustomerName,
    soh.CustomerID,
    CONCAT(s.FirstName, ' ', s.LastName) AS SalesPersonName,
    soh.SalesPersonID,
    sp.CommissionPct AS CommissionPercentage,
    ROUND((soh.SubTotal * sp.CommissionPct), 2) AS CommissionAmount 
FROM 
    Sales.SalesOrderHeader soh
LEFT JOIN 
    Sales.SalesPerson sp ON soh.SalesPersonID = sp.BusinessEntityID
LEFT JOIN 
    Sales.Customer sc ON soh.CustomerID = sc.CustomerID 
LEFT JOIN 
    Person.Person c ON sc.PersonID = c.BusinessEntityID
LEFT JOIN 
    Person.Person s ON sp.BusinessEntityID = s.BusinessEntityID;

--31,465 rows returned
``` 

## Query 15: Calculating Product Commission and Margin
Calculating product commissions and margins helps businesses optimize pricing strategies and profitability. By adjusting prices based on color categories and standard costs, businesses can maximize profit margins and sales revenue. This data-driven pricing approach ensures competitive pricing and profitability across product lines:

``` SQL Query for Analyzing Sales Transactions
SELECT 
    ProductID,
    Name AS ProductName,
    StandardCost,
    StandardCost * 0.1479 AS Commission
FROM 
    Production.Product;

B)

SELECT 
    ProductID, 
    Color, 
    StandardCost, 
    ListPrice,
    SUM(
        CASE 
            WHEN Color = 'Black' THEN Round((StandardCost + 0.22), 2)
            WHEN Color = 'Red' THEN Round((StandardCost - 0.12), 2)
            WHEN Color = 'Silver' THEN Round((StandardCost + 0.15), 2)
            WHEN Color = 'Multi' THEN Round((StandardCost + 0.05), 2)
            WHEN Color = 'White' THEN Round(((StandardCost * 2) / SQRT(StandardCost)), 2)
            ELSE StandardCost
        END
    ) AS NewStandardCost,
    (ListPrice - 
    SUM(
        CASE 
            WHEN Color = 'Black' THEN Round((StandardCost + 0.22), 2)
            WHEN Color = 'Red' THEN Round((StandardCost - 0.12), 2)
            WHEN Color = 'Silver' THEN Round((StandardCost + 0.15), 2)
            WHEN Color = 'Multi' THEN Round((StandardCost + 0.05), 2)
            WHEN Color = 'White' THEN Round(((StandardCost * 2) / SQRT(StandardCost)), 2)
            ELSE StandardCost
        END
    )) AS Margin
FROM 
    Production.Product
GROUP BY 
    ProductID, Color, StandardCost, ListPrice;
```

## Query 16: Creating a View for Product Analysis
Creating a view to identify the top five most expensive products for each color category facilitates product analysis and decision-making. By organizing product data based on color and price, businesses can identify trends and opportunities for product differentiation. This view provides valuable insights for product development and marketing strategies:

``` SQL Query for CREATING VIEW Top 5 Most Expensive Products Per Color 
A)
SELECT 
    ProductID,
    [Name] AS ProductName,
    Color,
    ListPrice,
    StandardCost,
    ROW_NUMBER() OVER (PARTITION BY Color ORDER BY ListPrice DESC) AS Rank
FROM 
    Production.Product;


B)

Query to find out the top 5 most expensive products by color

SELECT 
    ProductID,
    ProductName,
    Color,
    ListPrice
FROM 
    Top5MostExpensiveProductsPerColor
WHERE 
    Rank <= 5
```
#### Leveraging data from the AdventureWorks database enables businesses to extract valuable insights and drive informed decision-making across various business functions. By analyzing data systematically and implementing data-driven strategies, businesses can enhance operational efficiency, optimize performance, and achieve sustainable growth

## Enhancing SQL Query Performance: Best Practices and Recommendations
In today's data-driven world, optimizing SQL queries is paramount for efficient data retrieval and processing. Here are some general recommendations to enhance SQL query performance:

### Indexing
Appropriate indexing significantly improves query performance, especially on columns used for filtering and sorting. Indexes help speed up data retrieval by providing quick access to specific data subsets. Regularly analyze query execution plans to identify opportunities for index optimization.
### Query Review
Regularly reviewing SQL queries and their execution plans is essential to identify potential bottlenecks and optimize query performance. Analyze query execution times and resource utilization to fine-tune queries for improved efficiency.
### Database Design
Ensure that the database design aligns with query requirements, balancing data integrity with performance considerations. Normalize database schemas to minimize data redundancy and maintain data integrity. However, consider denormalization in cases where performance gains outweigh normalization benefits, such as in read-heavy workloads.
### Performance Monitoring
Implement robust monitoring and logging mechanisms to track database performance metrics, including query execution times, resource consumption, and database throughput. Proactively monitor performance trends and identify slow-running queries or resource-intensive operations for optimization.
### Security Considerations
Prioritize data security in SQL query development and execution. Implement stringent access controls to safeguard sensitive data and prevent unauthorized access. Use parameterized queries to prevent SQL injection attacks and sanitize user inputs to mitigate security risks.
By following these recommendations and adopting a proactive approach to SQL query optimization, organizations can ensure optimal database performance, streamline data retrieval processes, and mitigate potential security vulnerabilities. Regularly assess and fine-tune SQL queries to adapt to changing business requirements and evolving database environments.

# Conclusion
Exploring a series of SQL queries analyzing the AdventureWorks database provides actionable insights for strategic decision-making. From optimizing product selection and pricing strategies to evaluating employee demographics and streamlining manufacturing processes, each query offers valuable perspectives for enhancing operational efficiency and driving business growth. 

### Thank you for your interest.


