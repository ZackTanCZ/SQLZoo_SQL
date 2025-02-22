# [Medium Questions](https://sqlzoo.net/wiki/AdventureWorks_medium_questions)
## Question 1
A "Single Item Order" is a customer order where only one item is ordered. Show the SalesOrderID and the UnitPrice for every Single Item Order.

<details>
  <summary>SQL Query</summary>

```
SELECT c.CustomerID,sod.SalesOrderID, sod.SalesOrderDetailID, sod.ProductID, sod.UnitPrice, sod. OrderQty
FROM SalesOrderDetail as sod
JOIN SalesOrderHeader as soh
ON (sod.SalesOrderID = soh.SalesOrderID)
JOIN Customer as c
ON (soh.CustomerID = c.CustomerID)
WHERE OrderQty = 1
```

</details>

<details>
  <summary>Optimised SQL Query</summary>

```

```


</details>


## Question 2
Where did the racing socks go?\
List the product name and the CompanyName for all Customers who ordered ProductModel 'Racing Socks'.

<details>
  <summary>SQL Query</summary>

```
SELECT 
c.FirstName, c.CompanyName,
pm.Name as ProductModelName, 
p.Name as ProductName
FROM ProductModel as pm
JOIN Product as p
ON (pm.ProductModelID = p.ProductModelID)
JOIN SalesOrderDetail as sod
ON (p.ProductID =  sod.ProductID)
JOIN SalesOrderHeader as soh
ON (sod.SalesOrderID = soh.SalesOrderID)
JOIN Customer as c
ON (soh.CustomerID = c.CustomerID)
WHERE pm.Name = 'Racing Socks'
```

</details>

<details>
  <summary>Optimised SQL Query</summary>

```

```  
</details>

## Question 3
Show the product description for **culture 'fr'** for product with **ProductID 736**. 

<details>
  <summary>SQL Query</summary>

```
SELECT pd.Description
FROM ProductDescription AS pd
JOIN ProductModelProductDescription AS pmpd ON pd.ProductDescriptionID = pmpd.ProductDescriptionID
WHERE pmpd.ProductID = 736 AND pd.Culture = 'fr';
```  
</details>

<details>
  <summary>Optimised SQL Query</summary>

```

```  
</details>

## Question 4
Use the SubTotal value in SaleOrderHeader to list orders from the largest to the smallest.\ 
For each order show the CompanyName and the SubTotal and the total weight of the order

<details>
  <summary>SQL Query</summary>

```
SELECT c.CompanyName, soh.SubTotal, soh.Freight
FROM SalesOrderHeader as soh
JOIN Customer as c
ON (soh.CustomerID = c.CustomerID)
ORDER BY soh.SubTotal DESC
```  
</details>

<details>
  <summary>Optimised SQL Query</summary>

```

```  
</details>

## Question 5
How many products in ProductCategory 'Cranksets' have been sold to an address in 'London'?

<details>
  <summary>SQL Query</summary>

```
SELECT p.Name as ProductName, pc.Name as ProductCatName,
adds.City as City, soh.SalesOrderID as 'Sales ID',
sod.SalesOrderDetailID as 'Sales Order ID'
FROM ProductCategory as pc
JOIN Product as p
ON (pc.ProductCategoryID = p.ProductCategoryID)
JOIN SalesOrderDetail as sod
ON (p.ProductID = sod.ProductID)
JOIN SalesOrderHeader as soh
ON (sod.SalesOrderID = soh.SalesOrderID)
JOIN CustomerAddress as custadds
ON (soh.CustomerID = custadds.CustomerID)
JOIN Address as adds
ON (custadds.AddressID = adds.AddressID)
WHERE pc.Name = 'Cranksets' AND adds.City = 'London'
```  
</details>

<details>
  <summary>Optimised SQL Query</summary>

```

```  
</details>
