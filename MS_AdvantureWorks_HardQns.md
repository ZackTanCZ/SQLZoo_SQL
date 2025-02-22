# [Hard Questions](https://sqlzoo.net/wiki/AdventureWorks_hard_questions)
## Question 1
For every customer with a 'Main Office' in Dallas show AddressLine1 of the 'Main Office' and AddressLine1 of the 'Shipping' address - if there is no shipping address leave it blank. Use one row per customer.
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
  <summary>Optimised SQL Query</summary>

```

```  
</details>

## Question 3
Show the best selling item by value. 

>[!NOTE]
> What is considered 'Best Selling'?\
> What metric is 'value' referring to (Unit Price, List Price)?\
> The assumption is that 'value' is refered to as **OrderQty** multipled by **Unit Price**.

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
  <summary>Optimised SQL Query</summary>

```

```  
</details>

## Question 4
Show how many orders are in the following ranges (in $): 

<details>
  <summary>SQL Query</summary>

```

```  
</details>

<details>
  <summary>Optimised SQL Query</summary>

```

```  
</details>

