# [Hard Questions](https://sqlzoo.net/wiki/AdventureWorks_hard_questions)
## Question 1
For every customer with a 'Main Office' in Dallas\
show AddressLine1 of the 'Main Office'\
show AddressLine1 of the 'Shipping' address (if there is no shipping address leave it blank).\
Use one row per customer.

<details>
  <summary>SQL Query</summary>

```
WITH OfficeAddress AS(
SELECT
Customer.CustomerID as 'CustomerID',
Address.AddressID as 'AddressID',
Address.AddressLine1 as 'OfficeAddress',
CustomerAddress.AddressType as 'AddressType',
Address.City
FROM Address
JOIN CustomerAddress
ON (Address.AddressID = CustomerAddress.AddressID)
JOIN Customer
ON (CustomerAddress.CustomerID = Customer.CustomerID)
WHERE Address.City = 'Dallas' 
AND CustomerAddress.AddressType = 'Main Office'
), 
ShippingAddress AS (
SELECT
Customer.CustomerID as 'CustomerID',
Address.AddressID as 'AddressID',
Address.AddressLine1 as 'ShippingAddress',
CustomerAddress.AddressType as 'AddressType',
Address.City
FROM Address
JOIN CustomerAddress
ON (Address.AddressID = CustomerAddress.AddressID)
JOIN Customer
ON (CustomerAddress.CustomerID = Customer.CustomerID)
WHERE Address.City = 'Dallas' 
AND CustomerAddress.AddressType = 'Shipping'
)
SELECT cadds.CustomerID, c.FirstName, c.CompanyName,
oadds.City as 'Office City',
oadds.AddressType as 'Address Type',
oadds.OfficeAddress as 'Office Address',
COALESCE(sadds.City, ' ') as 'Shipping City',
COALESCE(sadds.AddressType, ' ') as 'Address Type',
COALESCE(sadds.ShippingAddress, ' ') as 'Shipping Address'
FROM Customer as c
JOIN CustomerAddress as cadds
ON (c.CustomerID = cadds.CustomerID)
JOIN OfficeAddress as oadds
ON (cadds.AddressID = oadds.AddressID)
LEFT JOIN ShippingAddress as sadds
ON (oadds.CustomerID = sadds.CustomerID)
ORDER BY sadds.City DESC
```

> The approach to this question is create two CTEs (Common Table Expression).\
> The first CTE 'OfficeAddress' contains a table for **main office addresses** in Dallas.\
> The second CTE 'ShippingAddress' contains a table for **shipping addresses** in Dallas.\
> Finally the customer's infomation is merged with the two CTEs through a series of JOINs.  


</details>

<details>
  <summary>Optimised SQL Query</summary>

```
SELECT
    c.CustomerID,
    c.FirstName,
    c.CompanyName,
    MAX(CASE WHEN ca.AddressType = 'Main Office' THEN a.City END) AS 'Office City',
    MAX(CASE WHEN ca.AddressType = 'Main Office' THEN a.AddressLine1 END) AS 'Office Address',
    MAX(CASE WHEN ca.AddressType = 'Shipping' THEN a.City END) AS 'Shipping City',
    MAX(CASE WHEN ca.AddressType = 'Shipping' THEN a.AddressLine1 END) AS 'Shipping Address'
FROM
    Customer AS c
JOIN
    CustomerAddress AS ca ON c.CustomerID = ca.CustomerID
JOIN
    Address AS a ON ca.AddressID = a.AddressID
WHERE a.City = 'Dallas' AND (ca.AddressType = 'Main Office' OR ca.AddressType = 'Shipping')
GROUP BY
    c.CustomerID, c.FirstName, c.CompanyName
ORDER BY
    MAX(CASE WHEN ca.AddressType = 'Shipping' THEN a.City END) DESC;
```

> Generated with Gemini
>
</details>

<details>
  <summary>Expected Query Output</summary>
  
  ![image](https://github.com/user-attachments/assets/33c1b609-ee37-4f24-abf1-d7913ce63410)

</details>


## Question 2
For each order show the SalesOrderID and SubTotal calculated three ways:\
A) From the SalesOrderHeader\
B) Sum of OrderQty*UnitPrice\
C) Sum of OrderQty*ListPrice 

<details>
  <summary>SQL Query</summary>

```
SELECT 
soh.SalesOrderID,
soh.SubTotal as 'A',
SUM(sod.UnitPrice * sod.OrderQty) as 'B',
SUM(p.ListPrice * sod.OrderQty) as 'C'
FROM SalesOrderHeader soh
JOIN SalesOrderDetail sod
ON (soh.SalesOrderID = sod.SalesOrderID)
JOIN Product as p
ON (sod.ProductID = p.ProductID)
GROUP BY soh.SalesOrderID
ORDER BY soh.SalesOrderID ASC
```
> The SubTotal computed in (B) and (C) differs from (A)\
> This is because I'm not familiar with the Componenets of SubTotal
</details>

<details>
  <summary>Approach to SQL Query</summary>

</details>

<details>
  <summary>Expected Query Output</summary>

  ![image](https://github.com/user-attachments/assets/79faf8ec-01de-4a3b-9d89-c7878672a3c9)

</details>

## Question 3
Show the best selling item by value. 

>[!NOTE]
> What price metric is 'value' referring to (Unit Price, List Price)?\
> The assumption is that 'value' is refered to as **OrderQty** multipled by **Unit Price**.\
> 'Best Selling' is determined as the highest value.

<details>
  <summary>SQL Query</summary>

```
SELECT
p.ProductID as 'Product ID',
p.Name as 'Product Name',
SUM(sod.UnitPrice * sod.OrderQty) as 'Qty x UnitPrice'
FROM Product as p
JOIN SalesOrderDetail as sod
ON (p.ProductID = sod.ProductID)
JOIN SalesOrderHeader as soh
ON (sod.SalesOrderID = soh.SalesOrderID)
JOIN Customer as c
ON (soh.CustomerID = c.CustomerID)
GROUP BY p.ProductID
ORDER BY SUM(sod.UnitPrice * sod.OrderQty) DESC
LIMIT 1
```  
</details>

<details>
  <summary>Approach to SQL Query</summary>

</details>

<details>
  <summary>Expected Query Output</summary>

  ![image](https://github.com/user-attachments/assets/f5a9d380-add1-4b42-b6aa-9d667c75b5ed)

</details>

## Question 4
Show how many orders are in the following ranges (in $):\
(A): 0 -99\
(B): 100 - 999\
(C): 1000 - 9999\
(D): 10,000 - 

>[!NOTE]
> What is an 'Order'?\
> Is it referring to **single** SalesOrderHeader or SalesOrderDetail?\
>The assumption is to use the **'SubTotal' column** in each **SalesOrderHeader** to group the records.  

<details>
  <summary>SQL Query</summary>

```
With CategoryTable AS (
SELECT
CASE 
WHEN soh.SubTotal BETWEEN 0 AND 99 THEN '0-99'
WHEN soh.SubTotal BETWEEN 100 AND 999 THEN '100-999'
WHEN soh.SubTotal BETWEEN 1000 AND 9999 THEN '1000-9999'
ELSE '10000-'
END as 'category', 
soh.SubTotal as 'values'
FROM SalesOrderHeader as soh
ORDER BY soh.SubTotal ASC
)
Select
category as 'Category',
COUNT(*) as 'No. of Rows' , 
SUM(CategoryTable.values) as 'Total Value'
FROM CategoryTable
GROUP BY Category
```  
</details>

<details>
  <summary>Approach to SQL Query</summary>

</details>

<details>
  <summary>Expected Query Output</summary>

  ![image](https://github.com/user-attachments/assets/3ea8cd78-f3ec-445e-9167-dee4a9a6d97a)

</details>

## Question 5
Identify the three most important cities.\
Show the break down of top level product category against city.

>[!NOTE]
> How do we consider an city important?\
> It is the city with the highest 'SubTotal'?\
> what is a 'top level product category'?

<details>
  <summary>SQL Query</summary>

```
With TopThreeCities AS (
SELECT 
adds.City as 'City',
SUM(soh.SubTotal)
FROM Address as adds
JOIN CustomerAddress as cadds
ON (adds.AddressID = cadds.AddressID)
JOIN SalesOrderHeader as soh
ON (cadds.CustomerID = soh.CustomerID)
GROUP BY adds.City
ORDER BY SUM(soh.SubTotal) DESC
LIMIT 3
)
SELECT 
adds.City as 'City',
pc.Name as 'Product Category',
COUNT(pc.Name)
FROM Address as adds
JOIN CustomerAddress as cadds
ON (adds.AddressID = cadds.AddressID)
JOIN SalesOrderHeader as soh
ON (cadds.CustomerID = soh.CustomerID)
JOIN SalesOrderDetail as sod
ON (soh.SalesOrderID = sod.SalesOrderID)
JOIN Product as p
ON (sod.ProductID = p.ProductID)
JOIN ProductCategory as pc
ON (p.ProductCategoryID = pc.ProductCategoryID)
WHERE adds.City IN (SELECT TopThreeCities.City FROM TopThreeCities)
GROUP BY adds.City,pc.Name
ORDER BY soh.SalesOrderID DESC


```  
</details>

<details>
  <summary>Approach to SQL Query</summary>
The CTE returns a Table of the **three** cities with the highest total 'SubTotal'\

</details>

<details>
  <summary>Expected Query Output</summary>
  
</details>


