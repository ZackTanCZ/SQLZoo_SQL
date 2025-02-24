# [Resit Questions](https://sqlzoo.net/wiki/AdventureWorks_resit_questions)
## Question 1
List the SalesOrderNumber for the customer 'Good Toys' 'Bike World'

<details>
  <summary>SQL Query</summary>

```
SELECT 
c.CompanyName, 
COALESCE(soh.SalesOrderID,"Not Found") as 'Sales Order No.'
FROM Customer as c
LEFT JOIN SalesOrderHeader as soh
ON (c.CustomerID = soh.CustomerID)
WHERE c.CompanyName = 'Bike World' OR c.CompanyName = 'Good Toys'

```

</details>

<details>
  <summary>Optimised SQL Query</summary>

```

```

> Generated with Gemini
>
</details>

<details>
  <summary>Expected Query Output</summary>

  ![image](https://github.com/user-attachments/assets/3d3d606f-0e2d-4326-a3cf-0fc4f041cbc8)

</details>

## Question 2
List the **ProductName** and the **quantity** of what was ordered by **'Futuristic Bikes'**
<details>
  <summary>SQL Query</summary>

```
SELECT 
c.CompanyName,
p.Name as 'Product Name',
sod.OrderQty as 'Quantity'
FROM Customer as c
JOIN SalesOrderHeader as soh 
ON (c.CustomerID = soh.CustomerID)
JOIN SalesOrderDetail as sod 
ON (soh.SalesOrderID = sod.SalesOrderID)
JOIN Product as p
ON (sod.ProductID = p.ProductID)
WHERE CompanyName = 'Futuristic Bikes'

```

</details>

<details>
  <summary>Optimised SQL Query</summary>

```

```

> Generated with Gemini
>
</details>

<details>
  <summary>Expected Query Output</summary>

  ![image](https://github.com/user-attachments/assets/bc353cb8-f321-47f9-bcf4-e21562d9a8f9)

</details>

## Question 3
List the name and addresses of companies containing the word 'Bike' (upper or lower case) and companies containing 'cycle' (upper or lower case).\
Ensure that the 'bike's are listed before the 'cycles's. 

<details>
  <summary>SQL Query</summary>

```
SELECT
c.FirstName,
c.CompanyName,
adds.AddressLine1,
CASE
WHEN c.CompanyName LIKE '%Bike%' THEN 'Bike'
ELSE 'Cycle'END AS 'Category'
FROM Customer as c
JOIN CustomerAddress as custadds
ON (c.CustomerID = custadds.CustomerID)
JOIN Address as adds
ON (custadds.AddressID = adds.AddressID)
WHERE c.CompanyName LIKE '%Bike%' OR c.CompanyName LIKE '%Cycle%'
ORDER BY Category ASC

```

</details>

<details>
  <summary>Optimised SQL Query</summary>

```

```

> Generated with Gemini
>
</details>

<details>
  <summary>Expected Query Output</summary>

  ![image](https://github.com/user-attachments/assets/a624364c-2e88-48a2-8743-557015f5a443)

</details>

## Question 4
Show the total order value for each CountryRegion.\
List by value with the highest first.

<details>
  <summary>SQL Query</summary>

```
SELECT
adds.CountyRegion,
SUM(soh.SubTotal) as 'TotalOrderValue'
FROM Address as adds
JOIN CustomerAddress as cadds
ON (adds.AddressID = cadds.AddressID)
JOIN SalesOrderHeader as soh
ON (cadds.CustomerID = soh.CustomerID)
GROUP BY adds.CountyRegion
ORDER BY TotalOrderValue DESC

```

</details>

<details>
  <summary>Optimised SQL Query</summary>

```

```

> Generated with Gemini
>
</details>

<details>
  <summary>Expected Query Output</summary>

  ![image](https://github.com/user-attachments/assets/e64ff05a-3200-483c-bb8b-2e5a847a692d)

</details>

## Question 5
Find the best customer in each region.

<details>
  <summary>SQL Query</summary>

```
WITH RankedCust AS (
SELECT
ROW_NUMBER() OVER (PARTITION BY adds.CountyRegion ORDER BY soh.SubTotal DESC) AS rank,
c.CompanyName as 'CompanyName',
adds.CountyRegion as 'CountyRegion',
soh.SubTotal as 'SubTotal'
FROM Address as adds
JOIN CustomerAddress as cadds
ON (adds.AddressID = cadds.AddressID)
JOIN Customer as c
ON (cadds.CustomerID = c.CustomerID)
JOIN SalesOrderHeader as soh
ON (cadds.CustomerID = soh.CustomerID)
ORDER BY adds.CountyRegion, soh.SubTotal DESC
)

SELECT 
rc.CompanyName as 'Name',
rc.CountyRegion as 'Region',
rc.SubTotal as 'Total'
FROM RankedCust as rc
WHERE rank = 1
```

</details>

<details>
  <summary>Optimised SQL Query</summary>

```

```

> Generated with Gemini
>
</details>

<details>
  <summary>Expected Query Output</summary>

  ![image](https://github.com/user-attachments/assets/ae1eb898-dee6-404c-aea8-fc0819e5f487)

</details>
